# CLM_FortranCode_Notebook

## 工作流

### 数据读取

- 输入数据集以netCDF等标准化格式准备和存储；
- 初始化
  - 调用外部函数和数据类型type
  - 声明内部数据类型type和内部函数
    - 初始化 subrountine init()
- 打开数据集文件

```
open( newunit=nu_nml, file=trim(NLFilename), status='old', iostat=nml_error ) 
```

- 检查维度
- 读取原始数据

```
read(nu_nml, nml=urbantv_streams,iostat=nml_error)
```

- 转换为预期的数据类型

```
# 分配内部变量
allocate(dataptr2d(lsize,isturb_MIN:isturb_MAX))
```



- 将读取的数据存储到内部数组和数据结构



- 根据物理计算的需求对输入数据进行插值或者子集化



- 完成后，数据关闭文件并释放资源。

```
deallocate(dataptr2d)
```



## code location

- 所有的code在“[CTSM/src/](https://github.com/ESCOMP/CTSM.git)”中

  - CTSM/src/main:
    - clm_driver.F90
    
    - clm_initializeMod.F90
    
    - clm_instMod.F90
    
    - clm_varcon.F90

    - clm_varctl.F90
    
    - clm_varpar.F90
    
    - clm_varsur.F90
    
    - #### LandunitType.F90
    
      - Lun：是landunit_type的简称
    
        ```
        type(landunit_type), public, target :: lun  !geomorphological landunits
        ```
    
    - decompMod.F90
    
      - bounds：用于数据索引
    
        ```
        type bounds_type
             integer :: begg, endg       ! beginning and ending gridcell index
             integer :: begl, endl       ! beginning and ending landunit index
             integer :: begc, endc       ! beginning and ending column index
             integer :: begp, endp       ! beginning and ending pft index
          end type bounds_type
        ```
    
        
    
  - CTSM/clm/src/biogeophys:
    - UrbanAlbedoMod.F90
    - UrbanFluxesMod.F90
    - UrbanParamsType.F90
      - module UrbanParamsType
      - The urban parameters (from the surface dataset) are read-in by UrbanParamsType.F90;
    - UrbanRadiationMod.F90
    - UrbanTimeVarType.F90
    - UrbBuildTempOlseon2015Mod.F90
  
  - CTSM/cpl/share_esmf
    - UrbanTimeVarType.F90:
      - The maximum building temperature is read-in using a special module since it is a streams file

## 注释

```
! !ARGUMENTS: 
！声明

! !LOCAL VARIABLES:
!在subroutine中声明局部变量

! !USES: 
！在subroutine中使用module
```

### module parameter & variable

- Parameter 是事先根据某种要求固定或先待定然后更具要取其值的量；
- Variable 是变化的量

```
在y=kx+b，其中k,b为parameter，而x,y是variable
```

### F90.in文件

（.in）文件是一种典型的输入文件，包含初始数据，作为计算机程序的输入

## code 

```
!对column_varcon模块的调用分成两部分
use column_varcon , only : icol_roof, icol_sunwall, icol_shadewall !building相关变量
use column_varcon , only : icol_road_perv, icol_road_imperv !road相关变量
```

### clm_varsur.F90

clm_varsur.F90中定义了module clm_instur

- module containing 2-d surface boundary data information surface boundary data, these are all "gdc" local Note that some of these need to be pointers (as opposed to just allocatable arrays) to match the **ncd_io** interface; for consistency, we make them all pointers







# NetCDF文件处理

[ref](https://docs.unidata.ucar.edu/netcdf-fortran/current/f90-use-of-the-netcdf-library.html)

## fortran 读取netcdf文件

[ref](https://www.heywhale.com/mw/project/625c3328e22b670017083476)

fortran中读取nc文件主要分为以下几步：

1. USE netcdf 
2. NF90_OPEN函数打开文件 
3. NF90_INQ_DIMID获取维度ID 
4. NF90_INQUIRE_DIMENSION获取各维度的长度 
5. ALLOCATE分配动态数组 
6. NF90_INQ_VARID获取变量ID 
7. NF90_GET_VAR获取变量值

```
program main 
    USE netcdf
    implicit none 

    integer:: status, fidA, dimID_TIME, dimID_LAT, dimID_LON, T2_ID, LAT_ID, LON_ID, U10_ID
    integer:: nx, ny, nt
    real   , dimension(:,:,:) , allocatable :: t2m, u10
    real   , dimension(:,:)   , allocatable :: lat, lon
    CHARACTER(LEN=100) :: file_in

    file_in  = 'Test.nc'

    !----------------------------------------------------------------
    ! 打开文件 :
    status = NF90_OPEN(TRIM(file_in),0,fidA)
    call erreur(status,.TRUE.,"read_A")

    !- read ID of dimensions of interest and save them in-----------------
    ! 获取维度变量ID
    status = NF90_INQ_DIMID(fidA,"time",dimID_TIME)
    call erreur(status,.TRUE.,"inq_dimID_TIME")

    status = NF90_INQ_DIMID(fidA,"y",dimID_LAT)
    call erreur(status,.TRUE.,"inq_dimID_LAT")

    status = NF90_INQ_DIMID(fidA,"x",dimID_LON)
    call erreur(status,.TRUE.,"inq_dimID_LON")

    ! 获取维度的值:
    status = NF90_INQUIRE_DIMENSION(fidA,dimID_TIME,len=nt)
    call erreur(status,.TRUE.,"inq_dim_TIME")

    status = NF90_INQUIRE_DIMENSION(fidA,dimID_LAT,len=ny)
    call erreur(status,.TRUE.,"inq_dim_LAT")

    status = NF90_INQUIRE_DIMENSION(fidA,dimID_LON,len=nx)
    call erreur(status,.TRUE.,"inq_dim_LON")

    print*,nx,ny,nt 

    !- Allocation of arrays :
    ALLOCATE(  t2m(nx, ny, nt)  )
    ALLOCATE(  u10(nx, ny, nt)  )
    ALLOCATE(  LAT(nx, ny)  )
    ALLOCATE(  LON(nx, ny)  )


    !- 获取变量ID :
    status = NF90_INQ_VARID(fidA,"T2",T2_ID)
    call erreur(status,.TRUE.,"inq_T2_ID")

    status = NF90_INQ_VARID(fidA,"U10",U10_ID)
    call erreur(status,.TRUE.,"inq_U10_ID")

    status = NF90_INQ_VARID(fidA,"lat",LAT_ID) !latitude 纬度
    call erreur(status,.TRUE.,"inq_LAT_ID")

    status = NF90_INQ_VARID(fidA,"lon",LON_ID) !longtitude 经度
    call erreur(status,.TRUE.,"inq_LON_ID")


    !- 获取变量值 :
    status = NF90_GET_VAR(fidA,T2_ID,t2m)
    call erreur(status,.TRUE.,"getvar_T2")
   
    status = NF90_GET_VAR(fidA,U10_ID,u10)
    call erreur(status,.TRUE.,"getvar_U10")

    status = NF90_GET_VAR(fidA,LAT_ID,LAT)
    call erreur(status,.TRUE.,"getvar_LAT")

    status = NF90_GET_VAR(fidA,LON_ID,LON)
    call erreur(status,.TRUE.,"getvar_LON")

    !- close netcdf file :
    status = NF90_CLOSE(fidA)
    call erreur(status,.TRUE.,"close_A")

    print*,t2m(:,1,1)
    print*,u10(:,1,1)

    print*,lat(:,1)

    print*,lon(:,1)

end 


SUBROUTINE erreur(iret, lstop, chaine)
! used to provide clear error messages :
USE netcdf
INTEGER, INTENT(in)                     :: iret
LOGICAL, INTENT(in)                     :: lstop
CHARACTER(LEN=*), INTENT(in)            :: chaine
!
CHARACTER(LEN=80)                       :: message
!
IF ( iret .NE. 0 ) THEN
  WRITE(*,*) 'ROUTINE: ', TRIM(chaine)
  WRITE(*,*) 'ERROR: ', iret
  message=NF90_STRERROR(iret)
  WRITE(*,*) 'WHICH MEANS:',TRIM(message)
  IF ( lstop ) STOP
ENDIF
!
END SUBROUTINE erreur


编译方法：

# bash
NETCDF=xxx/netcdf/4.4.1  
NC_INC="-I ${NETCDF}/include"    
NC_LIB="-L ${NETCDF}/lib -lnetcdf -lnetcdff"  
ifort -c $NC_INC io_netcdf.f90  
ifort -o run_example io_netcdf.o $NC_LIB  
./run_example  
```

