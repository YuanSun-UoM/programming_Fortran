# CLM_FortranCode_Notebook

## model workflow

### initial 

- 输入数据集以netCDF等标准化格式准备和存储；
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

### fortran 读取netcdf文件 

[ref](https://www.heywhale.com/mw/project/625c3328e22b670017083476) fortran中读取nc文件主要分为以下几步：

1. USE netcdf 
2. NF90_OPEN函数打开文件 
3. NF90_INQ_DIMID获取维度ID 
4. NF90_INQUIRE_DIMENSION获取各维度的长度 
5. ALLOCATE分配动态数组 
6. NF90_INQ_VARID获取变量ID 
7. NF90_GET_VAR获取变量值



## CTSM code location

- 所有的code在“[CTSM/src/](https://github.com/ESCOMP/CTSM.git)”中

  - CTSM/src/main:
    - clm_driver.F90
    
    - clm_initializeMod.F90
    
    - clm_instMod.F90
    
    - clm_varcon.F90

    - clm_varctl.F90
    
      - varctl是variable control的简称
    
    - clm_varpar.F90
    
    - clm_varsur.F90
    
    - LandunitType.F90
    
      - Lu是landunit_type的简称
    
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
      - UrbanTimeVarType.F90 的位置随版本变化

### module parameter & variable 区别

- Parameter 是事先根据某种要求固定或先待定然后更具要取其值的量；
- Variable 是变化的量

```
在y=kx+b，其中k,b为parameter，而x,y是variable
```

### F90.in文件

（.in）文件是一种典型的输入文件，包含初始数据，作为计算机程序的输入
