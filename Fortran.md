# Fortran 学习笔记

fortran用来输入一堆参数，经过复杂的计算，得到一堆参数。计算过程分离成一组组对象的方法



## 学习资源

[Fortran Coder 程序员聚集地 (fcode.cn)](http://fcode.cn/)

- 中文
- [网盘资源](http://pan.fcode.cn/)

[The Fortran Programming Language — Fortran Programming Language (fortran-lang.org)](https://fortran-lang.org/)

- 在线编译环境

[学习Fortran|WIKI教程 (iowiki.com)](https://iowiki.com/fortran/)

# 基础知识

[ref](https://zhuanlan.zhihu.com/p/490977046)

## 编辑器

linux系统下的编辑器

- Vi (Vim)
- Emacs
- gVim
- gEditor

## 编译器

- 编译器是实现Fortran底层代码不可或缺的一个工具，可供我们选择的编译器有许多，但是要根据自己的需要进行编译器的选择，选择时主要考虑如下几项内容：操作系统、32位/64位、版权许可和命令行/可视化。各个平台主流编译器的全面对比可以参考网站：http://fcode.cn/content-6-28-1.html
- [FAQ之 基本概念 ](http://fcode.cn/guide-29-1.html)
- ubuntu-Gfortran 编译器；
- 运行时库（Runtime Library）：被编译器用来实现编程语言内置函数

**Linux 系统下的编译器**

- gfortran
  - [gfortran 9 user guideline](https://gcc.gnu.org/onlinedocs/gcc-9.5.0/gfortran.pdf)
- Intel
- Portland Group
- IBM，SGI，CRAY，SUN，ABSOFT，SALFORD ，NAG，Compaq/HP

**在ubuntu中安装gfortran**

[Refs](https://blog.csdn.net/James_yaoshi/article/details/115385994?spm=1001.2101.3001.6650.6&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-6-115385994-blog-118296873.235%5Ev38%5Epc_relevant_anti_vip&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-6-115385994-blog-118296873.235%5Ev38%5Epc_relevant_anti_vip&utm_relevant_index=11)

- VScode编译环境

**在win10中安装gfortran和VScode**

- [refs](https://mp.weixin.qq.com/s/DMZWzNgUJH9oRTX4xnh4lA)
  - 这个版本的教程有点老了，新版的Modern Fortran 和其他插件会冲突，只要安装Modern Fortran插件即可，参考[refs](https://blog.csdn.net/donkeydog/article/details/125685571)；
  - mingw64：C语言编译器，MinGW是GCC的windows版本；
- [配置文件](https://blog.csdn.net/qq_24197463/article/details/89634794)
  - launch.json 中：“externalConsole”: false (false 是在VScode中出现结果，True是另外生成外部控制台)

## 程序开发

Fortran程序开发主要的过程可以分为三段：编译、链接、运行。

- 编译是针对一个源代码文件，输出为目标代码；
- 链接是针对一个或多个目标代码，输出为可执行程序；
- 编译、链接是创造可执行程序的过程，是开发者应该做的事情；
- 运行是使用可执行程序的过程，是用户应该做的事情。
- 我们在Fortran程序开发过程中既担任开发者也担任用户。

![运行](https://pic3.zhimg.com/v2-6cb9ec8b42aa83554f2886ffb726c312_r.jpg)

![](https://pic4.zhimg.com/v2-a3f841d13fb2a7eb7664cc438a23b647_b.jpg)

## 程序的嵌套

Fortran程序是由程序单元，如一个主程序，模块和外部子程序或程序的集合。

​	每个程序包括一个主程序和可以或可以不包含其它程序单元。主程序的语法如下：

```
program program_name
implicit none      

! type declaration statements      
! executable statements  

end program program_name
```

## 错误排查

报错的原因由三部分组成：编译的错误、链接的错误和运行的错误

- 编译是将一个fcode.f90文件编译为fcode.o文件的过程；
- 编译的错误由编译器抛出；
  - 语法规范 Syntax Error
  - 编译错误只看第一个，先解决第一个错误
    - 中英文切换错误， unrecoginized token 表明编译器未识别出中文全角的语句，应改为英文；
  - 减少编译错误：
    - 使用IMPLICIT NONE语句，申明程序的每一个变量类型
    - 返回所有输入值
    - 初始化所有变量
    - 用圆括号使赋值语句的功能更清晰
- 链接的错误由链接器抛出；
  - 链接是拼接各代码的过程，是编译器检查代码完整性的过程；
  - 链接错误种类最少，涉及用户代码部分容易解决，涉及编译器运行时库runtime.lib、第三方函数库、混编时不宜解决；
  - 链接错误通常不能获得发生错误的准确代码行数；
  - 链接错误也可以先看第一个错误，后面无论有多少个，可以先不管；
- 运行过程产生的错误是由runtime.lib文件给出的（runtime.lib指Run-time Library“运行时库”的作用是文件的读取、输入输出、给出运行错误）

## 格式

代码的编写格式有两种，自由格式(Free Format)和固定格式(Fixed Format)。程序代码扩展名为.F */* .FOR为固定格式，.F90为自由格式。推荐使用自由格式进行编码。

但是自由格式就是完全自由，可以随意编码吗？答案当然是否定的。与固定格式相比，它不会规定每一行的第几个字符有什么作用，但是它也有需要遵守的规则

- 每行可以编写132个字符
- 叹号“！”后面的文本都是注释，没有(编程)意义
- 代码不为区分大小写
- 两行代码可以用符号“&”进行连接
- 变量取名需要以字母开头，可以表现为字母和数字混合，且长度不超过32

**固定格式转自由格式**

- ForQuill 鹅毛笔 http://quill.fcode.cn

# 程序单元

程序源代码与文章具有相似的概念：

| 一个字   | 一个词语 | 一个句子 | 一个段落 | 一个章节   | 一篇文章 |
| -------- | -------- | -------- | -------- | ---------- | -------- |
| 一个字符 | Token    | 语句     | 程序单元 | 模块module | 程序     |

**主程序 main program**

主程序只是默认被系统首次调用的，具有程序入口点作用的函数；

## **子例行程序**

子例行程序,（子例程） subroutine，是没有返回值的函数；[ref](https://zhuanlan.zhihu.com/p/593584092)

- 通过在一个单独的CALL语句中引用其名称进行调用的过程，并且可以通过调用参数来返回多个结果；
- 每个子例程是个独立的程序单元，子程序把整个程序划分为一个个小的段落并隔离起来;
- 子例程的编译独立于主程序和其他的过程；
- 子程序之间通过**虚参实参结合（函数）**、common(建议弃用)、**module**等交换数据，通过相互调用实现程序流程的控制；一旦使用了module，就不要再用common了；
- 接口interface是控制子程序之间相互调用或传递参数的指导。

```fortran
SUBROUTINE subroutine_name (argument_list)
    ...
    (Declaration section)
    ...
    (Excution section)
    ...
RETURN
END SUBROUTINE [subroutine_name]
```

### **call: 调用子例程**

```fortran
CALL subroutine_name(argument_list) !参数列表中的实际参数的顺序和类型必须和子例程中定义的形参的顺序和类型相匹配
```

```fortran
!勾股定理
SUBROUTINE calc_hypotenuse(side_1,side_2,hypotenuse)
IMPLICIT NONE
!数据字典：声明调用参数类型和定义(指定intent属性)
REAL,INTENT(IN)::side_1
REAL,INTENT(IN)::side_2
REAL,INTENT(OUT)::hypotenuse
!数据字典：声明局部变量名和类型
REAL::temp
temp=side_1**2+side_2**2
hypotenuse=SQRT(temp)
END SUBROUTINE calc_hypotenuse
```

```fortran
PROGRAM test_calc_hypotenuse
IMPLICIT NONE
!数据字典：声明变量类型和定义
REAL::s1     !边长1
REAL::s2     !边长2
REAL::hypot  !斜边
!获得两个边的长度
WRITE(*,*)'Program to test subroutine calc_hypotenuse:'
WRITE(*,*)'Enter the length of side 1:'
READ(*,*)s1
WRITE(*,*)'Enter the length of side 2:'
READ(*,*)s2
!调用calc_hypotenuse
CALL calc_hypotenuse(s1,s2,hypot) !也可以写成CALL calc_hypotenuse(side_1=s1,side_2=s2,hypotenuse=hypot)
!输出斜边
WRITE(*,1000)hypot
    1000 FORMAT('The length of the hypotenuse is: ',F10.4)
END PROGRAM test_calc_hypotenuse
```

通过写驱动程序，对子例程进行调用测试。

### 虚参的INTENT属性

- 需要interface 人机界面

- 明确指定虚参的目的：输入参数、输出参数、中性参数

  - INTENT(IN)：声明输入值，仅用于向子程序传递输入数据，在子程序内部不允许改变

    Integer, Intetnt (IN) :: input_arg 

  - INTENT(OUT): 声明输出变量，仅用于将结果返回给调用程序，在子程序返回前必须改变（对应实参不能是常数，也不能是表达式）

    In'teger, Intent(OUT) :: output_arg

  - INTENT(INOUT) or INTENT(IN OUT):声明输入输出变量。即用于向子程序输入数据，也用来向调用程序返回结果。中性参数

    In'teger,  Intent( INOUT ) :: neuter_arg

    Integer :: neuter_arg !未指定Intent，则为中性

- **每一个过程中要始终记得声明每一个形参（虚参）的INTENT属性**；



**虚参的特殊用法**

- 可选参数：函数的某些参数在某些情况下，可以不赋予。运行时动态决定参数的个数。
  - 例如，open语句，大多数子句不需要书写，可以被认为是可选参数的函数。

- 函数名作为参数：实参和虚参都是函数名，让子程序本身以另外的子程序作为参数。此时虚参可以用external定义，也可以用interface定义（推荐）

```
! func是一个外部函数
Real Function integral( func , low_bound , up_bound , delta ) result ( y )
    !Real , External :: func ！用external定义外部函数
    Interface ！用inerface定义外部函数
      Real Function func( x )
        real :: x
      End Function func
    End Interface
    Real , intent( IN ) :: low_bound , up_bound , delta
    integer :: i
    real :: x
    y = 0.0
    x = low_bound + delta/2.0
    Do !// Do x = low_bound , up_bound , delta
      y = y + func( x )
      x = x + delta
      if ( x > up_bound ) exit
    End Do
    y = y * delta
End Function integral
```



### 变量传递

Fortran程序和它的子例程之间用地址传递(pass-by-reference)方案来进行通信：

当调用子例程时，主程序传递一个**指针**来指向**实参表**中各个参数的存储位置。 子例程查找调用程序所指向的内存位置， 以获得它需要的形参值。 

- 必须确保**调用参数列表中的值**与**子例程的调用参数**在**个数**、 **类型**、 **次序**方面都**完全匹配**。

```
Subroutine sub(a, b, c)
  Integer :: a, b
  Integer, optional :: c !可选参数
  write( * , * ) a, b
  if(present (c)) write(*,*)'optional:', c
End Subroutine sub  
```



### 传递数组给子例程

为确保子例程知晓数组的大小，有三种方式可指明形参数组的大小：

1. 在子例程调用时，将数组每一维度的边界值作为参数传递给子例程，并且将相应的形参数组声明为该长度

```fortran
!声明数组data1、data2，它们的宽度为n,然后在数组中处理nvals个元素值， 如果子例程中发生了引用越界错误，可以被检测到并报告。
SUBROUTINE process(data1,data2,n,nvals)
INTEGER,INTENT(IN)::n,nvals
REAL,INTENT(IN),DIMENSION(n)::data1
REAL,INTENT(OUT),DIMENSION(n)::data2
DO i= 1,nvals
data2(i)=3.*data(i)
END DO
END SUBROUTINE process
```

2. 把子例程中的所有形参数组声明为不定结构的形参数组， 以创建一个子例程的显式接口。
3. 第三种（也是最古老的）一种方法，用星号（＊）来声明每一个形参数组的长度， 称为不定大小的形参数组。(这种方式写出的子例程很难调试, 这种子例程也不能操作整个数组或部分数组 )

- 永远不要在子例程中使用 **STOP** 语句。 如果这么做，可能发布给用户一个一旦遇到某些特定数据集就会神秘终止的程序。
- 如果在一个子例程中可能存在错误条件，那么应该对错误进行检测，并设置错误标志，以返回给调用程序。调用程序在调用子例程后，应该对错误条件进行检测，并采取适当的操作。



## **函数子程序**

- 函数（function subprogram），通过在表达式中引入函数名来进行调用的过程，返回单个数值，该值用来参与表达式求值；
- 在函数内用虚参，在函数外（使用函数的角度）用的是实参；
- 在运用函数的过程中，要清楚处在函数内还是函数外；

### **函数的书写和调用**

返回变量 = 名称 （[实参1， 实参2....]）

[形容词] [返回类型] Function 名称（[虚参1， 虚参2...]）

[虚参的声明]

[局部变量的定义]

函数内部实现

名称 = 返回值

[return]

END [Function [名称]]

### **函数与子例程的区别**

| 函数                  | 子例行程序           |
| --------------------- | -------------------- |
| 有返回值              | 无返回值             |
| 调用 var = 函数名（） | 调用call子程序明（） |
| 可以通过虚参输出数据  | 可以通过虚参输出数据 |
| 允许有多个输出数据    | 允许有多个输出数据   |
| 可以包括文件操作      | 可以包含文件操作     |

由于fortran是默认传址的，所以可以通过虚参输出数据达到返回值的作用。因此，函数和子例程没有区别。

### **虚参和实参**

虚参和实参是调用者和子程序互换数据的最直接方式，通过虚参和实参的结合，在程序单元间传递数据：

- 调用者指定实参（actual arguments,实元）
- 子程序指定虚参（dummy arguments, 哑元，形参）

虚参和实参的传递方式有两种：传址（By reference）和传值（By value）

- 实参传递到子程序中，成为虚参
- 要求严格匹配实参和虚参（包括数据类型、kind值、数组的维度）

**程序单元**

- 程序单元的存在，可以提高代码重复利用率。各程序代码越“独立”，越能体现重复利用的作用。
- 程序单元间的变量，一般是互相不通的。
- Implicit None 应该写在每一个程序单元。
- 程序单元之间应该尽可能彼此独立，独立进行编译。

**result**

函数中result作为后缀，通过对于函数内返回值的重命名，以便于理解或书写；

```
Module typedef
  Implicit None
  Integer , parameter :: DP = Selected_Real_Kind( p = 9 )
  Type ST_Degree
    Integer(Kind=2) :: d , m
    real :: s    
  End Type ST_Degree
  
contains

  Type( ST_Degree ) Function RDegree_To_TypeDegree( rrDeg ) result( stt ) !result()作为函数的后缀，stt是函数内虚参的返回值
    Real(Kind=DP) ,Intent( IN ):: rrDeg
    real(Kind=DP) :: rr
    stt%d = int(rrDeg)
    rr = rrDeg - stt%d
    stt%m = ( rr ) * 60.0_DP
    rr = rr - stt%m/60.0_DP
    stt%s = ( rr ) * 60.0_DP**2
  End Function RDegree_To_TypeDegree
  
End Module typedef
```

### **递归子程序**

自个儿调用自个儿的子程序！在特殊的循环或者迭代中非常有效，但是容易出问题，对堆栈的使用开销较大。

所以不到万不得已，不使用递归子程序。

- 在不涉及链表的数据结构中，能不用递归尽量不用
- 一定要控制好递归终止条件，否则很容易无穷递归
- 递归函数尽量少使用局部变量
- 多级、一进多出的递归函数尤其需要特殊的优化

```
Program www_fcode_cn
  Implicit None
  Integer , parameter :: DP = Selected_Real_Kind( p = 12 )  
  real(Kind=DP) :: r = sqrt( 2.0_DP )
  integer :: i
  Do i = 1 , 60
    r = sqrt( 2.0_DP * r ) ! 递归
    write(*,*) r
  End Do
End Program www_fcode_cn

! 递归子程序可以用循环来达成
module def
  Integer , parameter :: DP = Selected_Real_Kind( p = 12 ) !获取需要的精度类型
end module def

Program www_fcode_cn
  use def
  Implicit None
  Real(Kind=DP) :: Func , r
  r = Func( 5 ) 
  write(*,*) r 
End Program www_fcode_cn

Recursive Function Func( n ) result( r ) !递归子程序要在开头加recursive
  use def
  Implicit None
  Real(Kind=DP) :: r
  Integer :: n
  !write(*,*) 'in:',n
  if ( n > 0 ) then !当n>0,则持续递归
    r = sqrt( 2.0_DP * Func(n-1) ) ！此为一进一出的递归（更复杂的一进多出的递归，往往与链表结合使用）
  else
    r = sqrt( 2.0_DP )
  end if
  !write(*,*) 'out:',n,r
End Function Func
```

**复杂递归子程序**

例如： 二叉树，要便利这个结构，就可以用到递归函数

### Interface

建议在任何函数调用时，都使用interface，告诉调用者，被调用这的各类信息

```
Interface
中间 !重新写了一遍子程序，但是不包含被调用者的执行语句、局部变量定义
End Interface

Interface 
   Subroutine sub( x )
     Real :: x(:,:) ！只需要写对外的一些内容，比如虚参的定义
   End Subroutine sub
End Interface
```

interface必须是用的情景：

- 函数返回值是数组、指针
- 参数为假定形状数组
- 参数具有intent、value属性
- 参数有可选参数、改变参数顺序

此外，在函数名作为虚参或者实参时，也推荐使用interface

```
Program www_fcode_cn
  Implicit None
  Interface !使用interface
    Subroutine sub( x )
      Real :: x(:,:)
    End Subroutine sub
  End Interface
  
  Real :: a( 30 , 30 )
  call sub( a )
  call call_sub()
End Program www_fcode_cn

Subroutine sub( x )
  Implicit None
  Real :: x(:,:)
  write(*,*) size(x,dim=1) , size(x,dim=2) !执行语句在interface中不需要出现
End Subroutine sub

Subroutine call_sub()
  Implicit None
  Interface ！在子程序中也需要interface,因为子程序也调用了sub()函数
    Subroutine sub( x )
      Real :: x(:,:) !假定形状数组
    End Subroutine sub
  End Interface
  Real :: b( 15 , 15 )
  call sub( b )
End Subroutine call_sub

```

**因为interface需要包含在每一个调用者函数中，是很麻烦的事情。这个问题通过module解决（module自带interface）**



## **模块**

一组程序单元及一组相关联的变量，可组成模块module。因此，module是数据和子程序的封装；

- 避免手动书写interface
- 数据共享（函数通过虚实结合共享数据，而module中数据可以自由使用，变量的值是保持一致的）
- 数据与过程封装、保护、继承
- module和type结合使得fortran的面向对象编程成为可能
- module把一系列相关函数、数据再封装起来，实现一系列的功能组合
- module会降低编译效率

```
Module mod_name ！module对外提供变量和子程序/函数
  Implicit None
  real , public     :: pub_var   =1.
  real , private    :: prvat_var =2.
  real , protected  :: prtct_var =3.

contains

  Subroutine sub1( dum_a , dum_b ) ！sub1内部不可以使用f1的虚参(局部变量还是相互隔离的)
    Real , Intent( IN ) :: dum_a 
    Real , Intent( INOUT ) :: dum_b
    real :: loc_1 , loc_2
    write(*,*) dum_a , dum_b , pub_var , prvat_var , prtct_var
    loc_1 = 5.0
    write(*,*) f1( loc_1 , dum_b ) ！sub1内部可以调用f1
  End Subroutine sub1

  Integer Function f1( dum_1 , dum_2 )
    Real , Intent( IN ) :: dum_1 
    Real , Intent( INOUT ) :: dum_2
    real :: loc_1 , loc_2
    f1    = dum_1 + dum_2 + pub_var + prvat_var + prtct_var
    dum_2 = 40.0
  End Function f1 !f1也可以调用sub1

End Module mod_name

Program main
  use mod_name
  Implicit None
  !call sub1( prvat_var , pub_var )
  !call sub1( pub_var , prtct_var ) 
  Intent(OUT)
  call sub1( prtct_var , pub_var )
End Program main
```

模块是个独立编译的**程序单元**，它包含了希望在程序单元间**共享**的数据的**定义**和**初始**值。

如果程序单元中使用了包含模块名的USE语句，那么在该程序单元中可以使用**模块中声明的数据**。每个使用同一个模块的程序单元可以访问同样的数据，所以模块提供了一种程序单元间的共享数据的方式。

### Save 语句

```fortran
MODULE shared_data
!目的：声明在两个程序之间共享的数据
IMPLICIT NONE
SAVE !SAVE:确保模块中声明的数据值在不同过程间引用时会被保留。在任何声明了可共享数据的模块中都应包含这条语句。
INTEGER,PARAMETER::num_vals= 5 !数组中的最大数值个数
REAL,DIMENSION(num_vals)::values  !数值值
END MODULE shared_data
```

```fortran
！通过USE语句调用模块
PROGRAM test_module
USE shared_data !USE:要使用模块中的数值，程序单元必须使用USE语句声明模块名
IMPLICIT NONE
REAL,PARAMETER::PI=3.141592 !Pi
values=PI*[1.,2.,3.,4.,5.] 
CALL sub1 !调用子程序
END PROGRAM test_module
!*****************************
!*****************************
SUBROUTINE sub1
USE shared_data
WRITE(*,*)values
END SUBROUTINE sub1
```



### use语句

module可以被继承，mod1可以use mod2, 从而获得mod2向外提供的所有公共变量和子程序

- 一个mudule可以use多个module，也可以被多个module使用;
- module的继承会形成类似树形的模块树；
- 防止出现环状依赖；

```
Module monitorsys
   use smartHome !完全使用
   
Module monitorsys
   use smartHome, only : tv , pc !使用其中的一部分； only可以避免命名冲突，加快编译速度，但不能节约内存
   
Module monitorsys   
   use smartHome, only : screen =>tv , pc !使用的时候，可以修改名称
```

**USE 语句必须出现在程序单元中的其他任何语句之前**

声明局部变量时，它的名字不应该和从关联 USE 继承的变量同名。这种对变量名的重复定义将会产生编译错误



### **contain语句**

contains 告诉编译器后面得语句被包含在过程中。

这些过程被作为模块的一部分进行编译，并且可以通过在程序单元中使用包含模块名的USE 语句使模 块过程在程序单元中有效，例如：

```fortran
!建立一个模块
MODULE my_subs
IMPLICIT NONE
!（在这里声明共享数据）
CONTAINS !在contains前不能书写任何执行语句
    SUBROUTINE subl(a,b,c,x,error)
    IMPLICIT NONE
    REAL,DIMENSION(3),INTENT (IN)::a !intent(in):将变量声明为只读，若在函数块中修改了其值，编译时会报错
    REAL,INTENT(IN)::b,c
    REAL,INTENT(OUT)::x !intent(out)：将变量指定为要输出的变量，若在函数快中没有赋值一个新的值给它，则编译时会报错
    LOGICAL,INTENT(OUT)::error
    END SUBROUTINE subl
END MODULE my_subs
```

如果程序单元的第一个非注释语句是“ USE my_subs"，那么调用程序单元中可以使用子例程subl。

```fortran
PROGRAM main_prog
USE my—subs
IMPLICIT NONE
CALL subl(a,b,c,x,error)
END PROGRAM main—prog
```

### module中数据

Module 向外提供变量（数据共享）和子程序，有不同的权限属性

| 权限属性  | 变量（数据）           | 子程序       |
| --------- | ---------------------- | ------------ |
| public    | 外部可以读取、可以修改 | 外部可以调用 |
| Protected | 外部只能读取、不能修改 | -            |
| Private   | 外部无法访问           | 外部无法调用 |

```
Module name
   Implicit None
   Private
   real , protected :: a, b, c
   real , public :: d, e, f
   real :: u, v, w
   public :: sub1, sub2, func
contains
   ...
```



# 基本语法

## 变量申明

### IMPLICIT NONE

```
IMPLICIT NONE #
```

- IMPLICIT可以将程序中以某一字母开头的所有变量指定为所需类型。指定了以A与C字母开头的所有变量都是整型变量，以字母I至K开头的所有变量为实型变量;
- implicit none即设计任何和隐含说明语句无效，所有变量都要显式地人工声明，不能未声明就直接使用，有效地避免了可能的大量错误。

**Fortran对字母大小写不敏感，A和a是两个相同的字母。**

```
Real(Kind=8),parameter, private :: rVar =20.0d0
！类型，形容词 :: 变量名(数组外形)=值，变量名2（数组外形）=值
Character(Len=32),Intent(IN)::cStr(5,8)
Integer m !没有形容词时可以不用::
Integer, save :: n = 30, m =40 !如果定义的同时赋值，则默认具有save属性，需要用双冒号
```

| 旧写法                                                | 新写法                                                       |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| REAL A                                                | Real :: a(30)                                                |
| DIMENSION A (30) ! 声明数组                           | Real, dimension(30) :: a                                     |
| REAL*8 A                                              | Real(Kind=8) :: a                                            |
| DOUBLE PRECISION A                                    | Integer, parameter :: DP = kind(0.0d0) 或者 Real(Kind=DP) :: a(30) |
| PARAMETER (N=30,M=40)                                 | Integer, parameter :: N=30, M=40                             |
| CHARACTER A*30 或 CHARACTER * 30 A 或 CHARACTER(30) A | Character(len=30) :: a                                       |



```
! executable statements 可执行语句
output = input1+input2  !求和输入值
output = input1 & 
		+input2        !求和输入值
999 output = input1 &   !求和输入值，999是语句标号，&表示两行代码之间的连接
			+input2
```

- &：进行标记并在下一行继续这一行书写，直到结束。
- ！：注释说明。
- 语句标号：以数字开始，语句标号可以是1~99999中任何一个数字，是Fortran语句中的“名字”，使用它可以在程序的其他地方引用这条语句。标号数字在程序单元中必须是唯一的。

```
PROGRAM my_first_program  !声明程序名称;
!  声明程序中用到的变量
INTEGER::i,j,k  !所有变量i,j,k均为整型;
!  获取存入变量i和j的值
WRITE(*,*)'Enter the numbers to multiply' !它输出信息，提示用户键入两个待相乘的数据;
READ(*,*)i,j !读入两个用户提供的整型数;
k = i * j !求两个数的相乘，结果存储在变量k
WRITE(*,*)'Result = ',k !输出计算结果
STOP !完成
END PROGRAM my_first_program
```

- 申明部分：一组不可执行语句组成，位于程序开头(PROGRAM)，定义程序名称和程序引用的数据及其变量的类型
  - 程序名称、变量的第一个字符必须是字母。
  - 如果存在PROGRAM语句，必须是程序的第一个语句行。
  - **变量声明的位置要在执行部分之前**
- 执行部分：多条语句构成，描述程序完成的操作。
- 终止部分：一条语句或终止程序执行的语句组成，告诉编译器程序结束。
  - 终止部分由STOP和END PROGRAM语句组成。
  - **STOP语句告诉计算机停止运行。**
  - END PROGRAM语句告诉编译器程序中不再有语句需要编译。

```
! STOP语句格式
STOP !只使用STOP语句，则执行将停止;
STOP 3 !STOP语句与数字一起使用，则程序停止时将打印出该数字，通常将作为错误代码返回给操作系统;
STOP 'Error stop' !如果STOP语句与字符串一起使用，则程序停止时将打印出该字符串;
ERROR STOP 'Cannot access database' !停止程序，但它也通知操作系统程序无法正常执行。
```

一般Fonran编程原则：

- 保留字都大写，如PROGRAM、READ和WRITE
- 程序的变量用小写字母表示。
- 名字中的下划线出现在两个字之间。
- 大写字母作为常量名。
- 由于大写和小写字母在Fortran中作用相当，所以程序按任何一种方式来书写都可以。

## 常数、变量

- 常数是**数据对象**，它定义在**程序执行之前**，且在**程序执行期间取值不可改变**。当Fortran编译器遇到常数时，它将常数放置在一个位置已知的内存单元，无论何时程序使用常数，就**引用该存储位置。**

- **fortran的每一数据必须申明类型，且每一个数据只有一种类型**；

- 程序单元中的每个Fortran变量有唯一的名字，变量名是内存中特定位置的标号，该标号方便人类记忆和使用。Fortran中的变量名可以长达**63个字符**，由**字母**、**数字**和**下划线字符**的任意组合构成，但是名字的**第一个字符总必须是字母**。

- Fortran有5个自带或“内置”的常数和变量数据类型：

  - 数字类：**INTEGER,REAL,COMPLEX**
    - INTEGER：整数，整型

      任何以字母i,j,k,l,m,n开头的变量名假定为**INETEGER**，其他字母开头的变量名则假定为**REAL**。默认情况下没有变量的类型为字符型。

      ```
      integer :: x,y,z !申明整数类型的数据；
      
      real(9)=9.0 !整数和实数之间的转化用编译器提供的real()函数
      int(7.8)=7 !int()函数丢掉小数部分
      nint(7.8)=8 !nint()函数进行四舍五入
      ceiling(7.8)=8 !大于等于的整数
      floor(7.8)=7 !小于等于的整数
      ```

      

    - Fortran中表征浮点数的有real（实型）和complex（复数型）两类。

      - 实数包括有理数和无理数
      - 复数包括实数和虚数
      
    - REAL：实数类型/ 浮点型/实型。单精准度（REAL*4）,双精准度（REAL*8）
    
      精度问题。利用kind进行精度说明，值取4为单精度，值取8为双精度。实数的精度设置十分重要，将会影响最后的计算结果是否正确。
    
      ```
      real(kind=4)   ::   distance !kind=4表示单精度; kind=8 表示双精度
      
      real, parameter :: PI = 3.1415926 ! parameter是形容词, 定义了PI这个常量
      ```
    
      两个实数相等问题。输出后，由于ab精度不同，他们的值也不同：
    
      ```
      program test
      implicit none
      real(KIND=4) :: a
      real(KIND=8) :: b
      a=0.123456789123456
      b=0.123456789123456
      write(*,*) 'a,b=',a,b
      end 
      !!!输出结果： a,b = 0.123456791  0.12345679104328156
      ```
    
      **浮点数存在误差，应尽量避免以下操作：**
    
      1. 避免对浮点数进行相等判断：
    
      ```
      if(a==1.3) !建议改为 if(abs(a-1.3)<1.0e-5)
      ```
    
      2. 避免用浮点数作为数组角标
    
      ```
      b =a(2.0) !建议改为 b =a(2)
      ```
    
      3. 避免用浮点数作为循环变量
    
      ```
      Do r = 0.0, 2.0, 0.1 
      !建议改为 Do i = 0,20
                r = i/10.0
      ```
    
      **浮点数误差的累加和放大**
    
      a = L * tan(x) !放大误差
    
      a = x**3 !放大误差
    
      累加 !积累误差
    
    - COMPLEX：复数（不常用）
    
  - 逻辑类：**LOGICAL**

    对于较长的代码，灵活使用逻辑型变量，可以提高代码的多样性

    ```
    logical :: a,b
    a = .true.
    b = .false.
    if(a)then
       …
    endif
    ```

    ```
    LOGICAL::debug=.false. !LOGICAL 变量的数值可以在声明时初始化
    ```

    

  - 字符类：**CHARACTER**

    ```
    charcter(len=16) :: atmosphere !指定字符长度len。若不指定，len的默认值为1.
    charcter(len=20) :: ocean
     
    atmosphere = ‘1234567890’
    ocean = ‘123456789123456’ !字符赋值注意使用引号
    ```

  - Type 派生类型（上述类型的组合）

    

- **默认方式**：任何以字母i,j,k,l,m,n开头的变量名假定为**INETEGER**，其他字母开头的变量名则假定为**REAL**。默认情况下没有变量的类型为字符型。

- 格式

  ```
  INTEGER:: var1[,var2,var3,...]
  REAL::[,var2,var3,...]
  CHARACTER(LEN=<len>)::var1[,var2,var3,...]
  ```

  - <len>是变量中的字符数目，是可选的（默认为1）
  - 假如圆括号中有数字，那么这个数字是语句声明的字符变量的长度。



### Kind 种别

kind用于区分同一种数据类型，但不同长度、精度、编码方式的一种代号。

- kind 受编译器的影响，具体的数值可能有差异
- **常数也有kind值**
- 对于Integer, Kind值影响整数能表达的最大范围
  - Integer 的kind值，常见1，2，4，8。kind默认是4，表示占4个字节。
  - Real的kind值，常见4，8，16。
- 对Real 和Complex，Kind值影响实数的最大范围和最小精度
- 对Character, Kind值表示编码（通常为ASCII编码）
- 对Logical, Kind值表示长度，对逻辑型无影响



### 0._r8

- _r8 后缀指定常量 0 是 8 字节（64 位）双精度浮点数；
- 句点表示它是浮点常量而不是整数；
- 如果没有后缀，0. 默认为单精度（32 位）；
- 8 表示双精度的类型参数值；
- fabd(p,ib) = 0._r8 表示将双精度值0.0分配给数组fabd中索引p和索引ib处的元素。
- fert_cft(begg:,cft_lb:) = 0.0_r8 表示将0.0（8个字节的实数）赋值给二维数组fert_cft中的数组切片，从begg开始，到cft_lb结束



## 变量初始化

在Fortran程序中有三种有效技术初始化变量：**赋值语句**、**READ语句**和**类型声明语句**中的**初始化**。

**赋值语句初始化**

```
PROGRAM init_1
INTEGER:: i
i = 1
WRITE(*,*)i
END PROGRAM init_1
```

READ语句初始化

```
PROGRAM init_2
INTEGER:: i
READ(*,*)i
WRITE(*,*)i
END PROGRAM init_2
```

类型声明语句初始化

```
PROGRAM init_3
INTEGER::i=1 !在定义时初始化，具有变量的save属性
WRITE(*,*)i
END PROGRAM init_3
```

## 数学运算

数学运算符的运算优先级，由高到低排列如下：

1. ( ) 括号里优先计算

2. ** 乘幂

   ```
   distance = 0.5 * acceleration * time ** 2
   ```

3. *乘法 / 除法

4. *加法 - 减法

## 混合运算

**影响混合运算结果的可能性：数据类型、数据精度**

含有实数和整数的表达式被称为混合模式的表达式，涉及实数和整数操作的运算称为混合模式运算。**在进行实数与** **整数操作的情况下，计算机将整数转换为实数，然后进行实数运算，结果是实数类型。**

- 当运算操作是在两个实型数据上完成，则结果的类型为REAL；
- 操作是在两个整型数上执行，则结果是INTEGER；
- 在进行实数与整数操作的情况下，结果是实数类型；
- 当除数与被除数的类型不同，计算的结果会以浮点数来表示；
- **在浮点数的表达式中，尽量把常数写成浮点数**
- 

| 混合运算 | 运算结果                                 |
| -------- | ---------------------------------------- |
| 1/2      | 0 ！整型与整型的计算结果依然是整型       |
| 1+1/4    | 1                                        |
| 1.+1/4   | 1                                        |
| 1+1./4   | 1.25 !整型与浮点数的计算结果一般是浮点数 |

## 转换函数

整型和实数型之间的转换

| 函数名和参数 | 参数X类型 | 结果类型 | 返回值说明                                         |
| ------------ | --------- | -------- | -------------------------------------------------- |
| INT(X)       | REAL      | INTEGER  | 实 ——> 整；X的整型部分(X被截尾）                   |
| NINT(X)      | REAL      | INTEGER  | 实 ——> 整；接近X的整数(X被四舍五入）               |
| CEILING(X)   | REAL      | INTEGER  | 实 ——> 整；大于或等于X最小的整数值；**≥ a** 的整数 |
| FLOOR(X)     | REAL      | INTEGER  | 实 ——> 整；小于或等于X最大的整数值；**≤ a** 的整数 |
| REAL(I)      | INTEGER   | REAL     | 整 ——> 实；整数转换为实数                          |

## 内置函数

| 函数名和参数 | 参数类型     | 结果类型 | 说明                                           |
| ------------ | ------------ | -------- | ---------------------------------------------- |
| SQRT(X)      | REAL         | REAL     | 大于0的平方根                                  |
| ABS(X)       | REAL/INTEGER | *        | 求X绝对值                                      |
| ACHAR(I)     | INTEGER      | CHAR(I)  | 返回在ASCII表上I位置上的字符                   |
| SIN(X)       | REAL         | REAL     | X的正弦(X必须是弧度值）                        |
| SIND(X)      | REAL         | REAL     | X的正弦(X必x须是角度值）                       |
| COS(X)       | REAL         | REAL     | X的余弦(X必须是弧度值）                        |
| COSD(X)      | REAL         | REAL     | X的余弦(X必x须是角度值）                       |
| TAN(X)       | REAL         | REAL     | X的正切(X必须是弧度值）                        |
| TAND(X)      | REAL         | REAL     | X的正切(X必须是角度值）                        |
| EXP(X)       | REAL         | REAL     | e的X次幂                                       |
| LOG(X)       | REAL         | REAL     | X的自然对数，其中X>0                           |
| LOGIO(X)     | REAL         | REAL     | 基数10的对数，其中X>O                          |
| IACHAR(C)    | CHAR(I)      | INTEGER  | 返回字符C在ASCll表上对照顺序的位置值           |
| MOD(A,B)     | REAL/INTEGER | *        | 模函数的余数                                   |
| MAX(A,B)     | REAL/INTEGER | *        | A和B中的更大值                                 |
| MIN(A,B)     | REAL/INTEGER | *        | A和B中的更小值                                 |
| ASIN(X)      | REAL         | REAL     | X的反正弦,-1<=x<=1 (结果是弧度值)              |
| ASIND(X)     | REAL         | REAL     | X的反正弦,-1<=x<=1 (结果是角度值)              |
| ACOS(X)      | REAL         | REAL     | X的反余弦,-1x\le1 (结果是弧度值)               |
| ACOSD(X)     | REAL         | REAL     | x的反正切,-\pi/2<=x<=\pi/2（结果是弧度值)      |
| ATAN(X)      | REAL         | REAL     | x的反正切,-90<=x<=90（结果是角度值)            |
| ATAND(X)     | REAL         | REAL     | x的反正切,-\pi/2<= x<=\pi/2（结果是弧度值)     |
| ATAN2(Y/X)   | REAL         | REAL     | x四象限的反切函数-\pi<= x<=\pi（结果是弧度值） |
| ATAN2D(Y,X)  | REAL         | REAL     | x四象限的反切函数-180<= x<=180（结果是弧度值） |

```
side = hypot * cos(theta)
```

# 程序设计与分支结构

[ref](https://zhuanlan.zhihu.com/p/593577982)

## 逻辑常数和变量

逻辑数据类型：**TRUE**、**FALUE**。

逻辑常数：**.TRUE.**、**.FALUE.**，数值的两边需要有句点以区别于变量名。

逻辑变量：保存逻辑数据类型数值的变量。

声明方式：

```fortran
LOGICAL::var1[,var2,var3,...]
```

## 赋值语句与逻辑计算

逻辑计算是用赋值语句来完成的：

```fortran
logical_variable_name = logical_expression ！等号右边的表达式可以是任何有效的逻辑常量、逻辑变量和逻辑运算符的组合。逻辑运算符是数字、字符或逻辑数据的运算符，产生逻辑结果。逻辑运算符有两种基本类型：关系运算符和组合运算符。
```

**关系运算符**

运算的结果是.TRUE.或者.FALSE.

| 新形式 | 旧形式 | 意义       |
| ------ | ------ | ---------- |
| ==     | .EQ.   | 等于       |
| /=     | .NE.   | 不等于     |
| >      | .GT.   | 大于       |
| >=     | .GE.   | 大于或等于 |
| <      | .LT.   | 小于       |
| <=     | .LE.   | 小于或等于 |

**组合逻辑运算符**

| 运算符   | 功能       | 定义                                         |
| -------- | ---------- | -------------------------------------------- |
| a.AND.b  | 逻辑与     | 如果a和b为真，则结果真                       |
| a.OR.b   | 逻辑或     | 如果a和b任一为真，则结果为真                 |
| a.EQV.b  | 逻辑等值   | 如果a和b相同（同真或同假），则结果为真       |
| a.NEQV.b | 逻辑非等值 | 如果a和b其中一个为真，另一个为假，则结果为真 |
| .NOT.a   | 逻辑非     | 如果a为假，则结果为真，如果a为真，则结果为假 |

```
# .and. 的用法：
if (lun%itype(l) == istice_mec .and. &
            glc_behavior%has_virtual_columns_grc(g))
```



## 分支 if / select case

分支是允许跳过其他代码段而选择执行特定代码段(称为程序块)的Fortran语句。主要有IF语句和SELECT CASE结构。

### **IF**

逻辑表达式包括以下几种：

a==5 ! 判断数值是否相等

str =='fcode' !判断字符串是否相等

isf = =.TRUE. ！判断真假是否一致

.TRUE. !直接用逻辑类型

```fortran
IF(logical_expr) THEN !IF(…)THEN是单个Fortran语句，必须一起写在同一行上
Statement 1 !要执行的语句必须占用IF (…)THEN语句下面的单独的一行
Statement 2
....
END IF  ！EDN IF 必须另起一行，在包含END IF的行上不能有行号
```

```
IF(logical_expr) statement !逻辑IF语句这种形式与在IF块中只带有一个语句的IF结构块等价
```

**ELSE 和ELSE IF 子句**

复杂的逻辑判断

```
[name:]IF(logical_expr1)THEN ![name:]可加可不加，加了表示给IF结构块一个制定名称
Statement 1
Statement 2
....
ELSE IF(logical_expr2)THEN
Statement 1
Statement 2
....
ELSE
Statement 1
Statement 2
....
END IF [name] ！如果给IF指定一个名称，那么在关联的END IF上也必须出现同样的名称
```

```
[name:] IF(logical_expr1)THEN
Statement 1
Statement 2
....
ELSE IF(logical_expr2)THEN [name] !对于结构的ELSE和ELSE IF语句，名称是任选的，但如果使用了名称，他们必须与IF上的名称相同
Statement 1
Statement 2
....
ELSE [name]
Statement 1
Statement 2
....
END IF [name]
```

**有且只有一个IF ...THEN语句**，可以有任意多个树木的ELSE IF 子句，只能有一个ELSE子句

### **SELECT CASE**

SELECT CASE(Expression)，其中expression可以是:

- CASE(10)
- CASE('fcode')
- CASE(5:) ：当数值大于5的时候，执行某些语句
- CASE(-5:5)
- CASE(.TRUE.)
- CASE(.FALSE.)

```
[name:] SELECT CASE(case_expr) !case_expr可以是任意的整数、字符或者逻辑表达式
CASE(situation_1)[name]
statement 1
statement 2
....
CASE(situation_2)[name]
statement 1
statement 2
....
CASE DEFAULT [name] !在CASE结构中总是包含一个CASE DEFAULT子句来捕捉可能在程序中发生的任何逻辑错误或非法输入
statement 1
statement 2
....
END SELECT [name]
```

**由于四舍五入误差可能引起两个应该相等的变量在测试相等性时失败，因此注意IF结构中实数变量的相等性测试问题。取代的方法是，在所使用的计算机上测试变量在四舍五入误差范围内是否与期望的值近似相等**

### **SELECT TYPE**



# 循环语句结构

[ref](https://zhuanlan.zhihu.com/p/593579267)

## DO循环结构

循环结构有两种基本形式：**当循环**和**迭代循环**(或计数循环)。

**当循环**

当循环中的代码循环次数不确定，直到满足了某个用户指定的条件为止。 

- 在DO 和 END DO之间的语句块重复执行；
- 当IF 逻辑表达式变为真，执行EXIT语句。执行EXIT语句后，控制程序转到END DO之后的第一条语句处；
- 

```
DO
...
IF(逻辑表达式)EXIT
...
END DO
```

```
!奇数
Do i = 1, 100, 2
```

```
DO i = 1， 100
  ...
END DO
WRITE (*,*) i !fortran标准没有规定循环正常结束后循环变量的值。i值无定义，不同编译器给出结果可以不同

！但在使用EXIT退出循环时，循环变量是退出前的值
DO i = 1 , 100
   ...
   IF(i == 99) EXIT
END DO
WRITE(* , *) i !i值有定义，i = 99
```



```
PROGRAM STATS_1
!求平均值和标准方差
IMPLICIT NONE
!数据字典：声明变量类型，定义和计量单位
INTEGER::n=0          !输入样本个数
REAL::std_dev=0       !样本的标准差
REAL::sum_x=0         !REAL::sum_x=0 不是执行语句，因此多次调用函数，并不是每次进入函数sum_x2都赋值为0
REAL::sum_x2=0
REAL::x=0
REAL::x_bar
!循环计数
DO
READ(*,*)'Enter the number:',x
WRITE(*,*)"The number is:",x
IF(x<0) EXIT
n=n+1
sum_x=sum_x+x
sum_x2=sum_x2+x**2
END DO
x_bar=sum_x/n
xtd_dev=SQRT((n*sum_x2-(sum_x)**2)/(n*(n-1)))
WRITE(*,*)"The mean this data set is: ",x_bar
WRITE(*,*)"The standard deviation is：",std_dev
WRITE(*,*)"The number of data points is：",n
END PROGRAM STATS_1
```

**迭代循环**

迭代循环中的代码循环次数是确定的，重复次数在循环开始前是已知的。

- index是一个**整型变量**，作为循环计数器使用；
- 整型数istant、iend、incr是计数循环的参数，它们控制变量index在执行期间的数值
- incr是可选的（默认为1，可设置为负数）；
- 三个循环参数istart、iend、incr可以是常量、变量或表达式。如果是变量或表达式，其值是在循环开始前进行计算，得到的数值用于控制循环；
- 在DO循环执行的开始处，程序将数值istart赋值给控制变量index。如果index * incr<=iend * incr, 程序执行循环体内的语句。
- 在循环体内的语句被执行后，控制变量重新计算为：index = index + incr

```fortran
DO index=istart,iend,incr
statement_1
statement_2
...
statement_n
END DO
```

```fortran
PROGRAM STATS_2
IMPLICIT NONE
INTEGER::n     !输入样本数据集的个数
INTEGER::i     !计数器
REAL::x        !样本数据
REAL::X_bar    !平均值
REAL::std_dev  !方差
REAL::sum_1
REAL::sum_2 
WRITE(*,*)'Enter number of points:'
READ(*,*),n
IF(n<2)THEN
    WRITE(*,*)'At least 2 values must be entered.'
ELSE
    DO i=1,n !此处没有指定incr,默认为1; i取值1,2,3...n
        WRITE(*,*)'Enter the number:'
        READ(*,*),x
        sum_1=sum_1+x
        sum_2=sum_2+x**2
    END DO
    x_bar=sum_1/n
    std_dev=SQRT((n*sum_2-(sum_1)**2)/(n*(n-1)))
END IF
WRITE(*,*)"The mean this data set is: ",x_bar
WRITE(*,*)"The standard deviation is：",std_dev
WRITE(*,*)"The number of data points is：",n
END PROGRAM STATS_2
```

```
Do i = -1, 3 !i取值 -1,0,1,2,3
```

```
！无穷循环的DO
DO
...
END DO
```

```
i = 0
DO
  i = i + 1
  ...
  IF(ConditionA) i = i -1 !通过无穷循环
  IF(i == 100) EXIT
END DO  
```



```
!署名的DO循环(署名的作用：流程控制，特别是在多层嵌套的情况下)
Outer: DO i = 1, 10
   Inner: DO j = 1, 20
   ...
   ...
   END DO Inner
END DO Outer   
```

```
n = 10
DO i = 1, n * 100 !循环变量的上下限使用表达式进行指定时，循环的上下限在进入循环时计算，并且在循环期间不再改变
   ...
   IF(conditionA) n = n -1
END DO   
```



### 显式循环和隐式循环

**显式循环**

```
PROGRAM DO1_test
IMPLICIT NONE
INTEGER , DIMENSION(5) :: a = (/1,2,3,4,5/)   
INTEGER:: i 
DO i=1,5 !每一次循环会执行一条write语句，共有五条write语句
    WRITE(*,100) a(i) , 2*a(i) , 3*a(i) ！ 每一条write语句有三个变量
    100 FORMAT(6I3) ！至多输出6次，每次输出前三个字符(实际结果为3I3即可)
END DO
STOP
END PROGRAM DO1_test
```

结果：

  1  2  3
  2  4  6
  3  6  9
  4  8 12
  5 10 15

**隐式循环**

implied do, 不具有流程控制的作用，大多是用来作为输入和输出的简化，以及数组初始化的简化

```
INTEGER ::a(10)
INTEGER :: i
a = [(i , i = 1, 10)] !初始化数组，数组中的字符包括从1到10

!可以改写为如下显式循环
INTEGER ::a(10)
INTEGER ::i
DO i = 1, 10
   a(i) = i
END DO   

```



```
PROGRAM DO1_test
IMPLICIT NONE
INTEGER , DIMENSION(5) :: a = (/1,2,3,4,5/)   
INTEGER:: i 

WRITE(*,100) (a(i) , 2*a(i) , 3*a(i) , i=1,5) !隐式循环尽管执行五次，但只有一条write语句，变量个数大于格式数目，格式可以重复利用。因此，FORMAT(6I3)为每6个变量使用一轮格式再进行换行。
100 FORMAT(6I3) 

STOP
END PROGRAM DO1_test
```

结果：

  1  2  3  2  4  6
  3  6  9  4  8 12
  5 10 15

### DO WHILE 语句

```
DO WHILE (Expression)
 ...
END DO
```

```
PROGRAM DOWHILE
    IMPLICIT NONE
    INTEGER :: sum, i
    sum = 0
    i = 0
    DO WHILE (sum<1000) !不指定循环的次数，而是指定一个表达式，例如要求sum值小于1000
         i = i + 1
         sum = sum + i
    END DO
    WRITE(* , *)i,sum
END PROGRAM    
```



## CYCLE和EXIT语句

Cycle 可以与continue类比

- 忽略本轮循环剩余的内容，直接进入下一轮循环；

```fortran
PROGRAM test_cycle
INTEGER::i
DO i=1,5
IF(i==3) CYCLE
WRITE(*,*)i
END DO
WRITE(*,*)'END OF LOOP!'
END PROGRAM test_cycle ！输出1,2,4,5
```

Exit 可以与break类比

- （用于循环时）忽略循环剩余的内容，跳出（指定）循环

```
PROGRAM test_cycle
INTEGER::i
DO i=1,5
    IF(i==3) EXIT
    WRITE(*,*)i
END DO
WRITE(*,*)'END OF LOOP!'
END PROGRAM test_cycle ! 输出1,2（当i==3时，停止）
```

用CYCLE和EXIT循环和退出特定署名的循环

```
Outer: Do i = 1, 10
   Inner: Do j = 1, 20
       If (conditionA) THEN
          CYCLE Outer
       ELSE IF (ConditionB) THEN
          EXIT Outer !此处直接跳出了外层循环Outer
          ! EXIT （如果不署名，不指定署名的循环，则只跳出了内层循环）
          END IF
END DO Outer       
```



## 命名的循环

```
[name:]DO
    statement
    ...
    IF(逻辑表达式)CYCLE[name]
    ...
    IF(逻辑表达式)EXIT[name]
    END DO[name]
    !
[name:]DO index=istart,iend,incr
    statement
    ...
    IF(逻辑表达式)CYCLE[name]
    ...
    IF(逻辑表达式)EXIT[name]
    END DO[name]
```

## 嵌套循环和IF块循环

**嵌套循环**

```
PROGRAM nested_loops
INTEGER::i,j,product
DO i =1,3
    DO i=1,3
        product =i*j
        WRITE(*,*)i,"*",j,"=",product
    END DO
END DO
END PROGRAM nested_loops
```

# 字符操作

- 字符串常用于文件名、格式符

## 字符赋值和字符操作

**字符赋值**

- 如果字符表达式短于它所赋值的字符变量的长度，那么变量的多余部分就用空格填充。

- 如果字符表达式长于它所赋值的字符变量的长度，那么超出字符变量的部分就被省略。

```
CHARACTER(len=3)::file_ext
file_ext='f'
```

**子字符串提取**

子串（子字符串）提取选择字符变量的一部分，并将该部分看作一个独立的字符变量

```
PROGRAM test_charl 
CHARACTER(len=8) ::a,b,c 
a='ABCDEFGHIJ' 
b='12345678' 
C=a(5:7) !字符串提取
b(7:8)=a(2:6) 
END PROGRAM test_charl
```

**//:连接操作符**

```
PROGRAM test_char2 
CHARACTER (len=10)::a
CHARACTER(len=8)::b,c 
a ='ABCDEFGHIJ' 
b ='12345678' 
c =a(l:3)//b(4:5)//a(6:8) 
END PROGRAM test_char2  !变量c包含字符串'ABC45FGH'。
```

Trim函数：用于截取字符串末尾空格的函数

```
！依次打开1.txt, 2.text...利用字符串实线“批量文件处理”
PROGRAM CHARACTERTRIM
Integer :: i
Character(len=12)::cFile !定义一个字符串
Do i = 1, 10 !表示循环10次
 write( cFile, *) i ！把i的值写入cFile文件中
 Open(10+i, File = trim(cFile)//'.txt') ！"File = "用于指定文件名称；通过//连接字符串
End Do
END PROGRAM CHARACTERTRIM
```

```
！利用字符串实现“动态format”
PROGRAM DYNAMICFORMAT
Character(len=5)::cFmt = "(???f13.6)"
write(cFmt(2:4),'(i3)')N ！在字符串的第2到4位输入3个数//假设N=365; (i3)控制输入格式是三个字符宽度
!cFmt = "(365f13.6)"
write(*,cFmt)rArr(1:N) ！把cFmt输出
```

### **字符数据的关系运算符**

利用关系运算符**＝＝、／＝、＜、＜＝、＞**和**＞＝**，字符串可以在逻辑表达式中进行比较。比较的结果为逻辑值**真**或**假**。

1. 单个字符是基于执行程序的计算机上的字符排序序列。字符排序序列是字符在特定的字符集内出现的顺序( 码)。
2. 字符串比较是比较从每个字符串的第一个字符开始。如果它们是相同的，那么再比较第二个字符

例如：**'AAAAAB'>'AAAAAA'**。

1. 如果两个字符串的长度不同,比较操作从每个字符串的第一个字符开始，每个字符进行比较，直到发现了4为止。如果两个字符串一直到其中一个结束时始终是相同，那么就认为另一个字符串为大。
1. == 等于，/=不等于，>大于，<小于，>=大于或等于，<=小于或等于

例如：**'AB'>'AAAA' and 'AAAAA'>'AAAA'**

## 内置字符函数

| 函数名和参数   | 参数类型                    | 结果类型 | 注释                                                 |
| -------------- | --------------------------- | -------- | ---------------------------------------------------- |
| ACHAR(ival)    | INT                         | CHAR     | 整型 ——> 字符型；返回ival在ASCII排序序列中相应的字符 |
| IACHAR(char)   | CHAR                        | INT      | 字符型 ——> 整型；返回char在ASCII排序序列中相应的整数 |
| LEN(str1)      | CHAR                        | INT      | 字符型 ——> 整型；返回str1的字符长度                  |
| LEN_TRIM(str1) | CHAR                        | INT****  | 字符型 ——> 整型；返回str1的长度，不包括尾部的空格    |
| TRIM(str1)     | CHAR                        | CHAR     | 字符型 ——> 字符型；返回被截去尾部空格的str1          |
| LLT(str1,str2) | 字符串小于 little than      |          | 当str1 < str2时，返回真                              |
| LLE(str1,str2) | 字符串小于等于 little equal |          | 当str1<=str2时，返回真                               |
| LGT(str1,str2) | 字符串大于                  |          | 当str1 > str2时，返回真                              |
| LGE(str1,str2) | 字符串大于等于              |          | 当str1>=str2时，返回真                               |

# I/O概念

## 文件读写

由于文件形式（两种：文本文件、二进制文件）的不同，fortran语法规定了不同的方式来读取：

|                                                  | 文本文件（有格式文件）               | 二进制文件（无格式文件）                                  |
| ------------------------------------------------ | ------------------------------------ | --------------------------------------------------------- |
| 顺序读取：按照先后顺序读取文件                   | 顺序读取有格式文件（用得最多）       | 顺序读取无格式文件（在记录前后各增加4字节，表示记录长度） |
| 直接读取：指定特定的行直接读取（需每行长度一样） | 直接读取有格式文件（要求每行一样长） | 直接读取无格式文件（用得较多）                            |
| 流文件                                           |                                      | 流文件（非常方便，强大）                                  |

[ref](https://zhuanlan.zhihu.com/p/593580397)

- 对于少量的、起控制作用的、经常需要修改的数据，采用顺序读写文本文件（文本文件对于人类易懂，对于计算机不能直接识别，因此文本文件的读写非常慢）；
- 对于较大量的、规则排列的数据，采用直接读写文本文件；
- 对于大量的数据，采用二进制文件（用于机器阅读，人类读懂较困难。但是它的格式与内存一样，因此是简单地复制到文件里，速度非常快，适合大量的数据、中间数据、不希望普通用户看懂和随意更改的数据）；

Fortran文件的控制语句

| I/O语句   | 作用                                  |
| --------- | ------------------------------------- |
| OPEN      | 将特定磁盘文件与特定的i/o单元号相关联 |
| CLOSE     | 结束特定磁盘文件与特定i/o单元号的关联 |
| READ      | 从特定i/o单元号读取数据               |
| WRITE     | 向特定i/o单元号写入数据               |
| REWIND    | 移动到文件夹的开头                    |
| BACKSPACE | 在文件中向前移动一条记录              |

读写文件需要用到4个语句： open 打开文件，write写入文件，read读取文件，close关闭文件

### OPEN 语句

open语句将一个文件与一个给定的I/O单元号关联起来，。

```
#open()命令的默认选项
open(UNIT=number, FILE=‘filename’, FORM=‘FORMATTED’, STATUS=‘UNKNOWN’, ACCESS=‘SEQUENTIAL’, RECL=length, ERR=label, IOSTAT=iostat, BLANK=‘NULL’, POSITION=‘ASIS’, ACTION=‘READWRITE’, PAD=‘YES’, DELIM=‘NONE’)


open(unit=iounit, file=name,form=formatted iostat=ios, &
     status="old/new/replace/scratch/unknown", action="read/write/readwrite", access="sequential/direct"sequential/direct)
if ( ios /= 0 ) stop "Error opening file name"
```



- unit需为整数，避免1、2、5、6；通道号通常大于10；Fortran2003后增加了NewUnite语句；
- form可以取值formatted（用文本文件形式保存）和unformatted（用二进制文件格式来保存）
- status取值old（表示文件原本就存在）、new（表示这个文件原本不存在，是第一次打开）、replace（文件已经存在，会覆盖掉之前的内容；如果文件不存在，会生成一个新文件）、scratch（打开一个暂存盘，不需要指定文件名称，程序会主动去取一个文件名）、unknown（由编译器决定）
- access取值sequential（顺序读取）、direct（直接读取）
- Recl：指length的意思，表示一次可以读出length长的数据

```
OPEN(open_list) !open_list包含一组子句，分别指定i/o单元号、文件名和关于如何存取文件的信息，列表中的子句用逗号隔开

！打开文本文件
OPEN(子句 = 值, 子句 = 值, 子句 = 值) ！有必要的子句是unit和通道号

OPEN(Unit = 通道号， File = "文件名")
OPEN(通道号， File = "文件名") ！文件通道号由程序员给定，一般用大于10的数字（10以下的数字由不同编译器预留）

```

### **通道号**

对于**文本文件**，通道号是以后操作该文件的“凭证”，在read/write/close 时指定，也可以用变量/循环来打开多个文件。

- 文件打开后，读取位置默认在文件开始处

```
OPEN(12, File = 'a.txt')

Do i = 100, 110
  Open(i, File = cname(i))
End Do

Read(12,*) x,y,z ！*星号表示表控格式list-direct,让变量列表自动控制格式，让变量列表； 在不特殊指定格式的情况下，每个read语句读取整的N行。
Write(13,*)u,v,w

Do i = 100, 100
 Close(i)
End Do
```

- 通道号和句柄的区别是：句柄由过程内部自动分配，外部不能决定是多少。而通道号由程序给定，万一给错额程序就会出错。
- fortran2003后，增加了NewUnit这个子句

```
integer :: FILE_IN
OPEN (NewUnit = FILE_IN, File = "2d_text.txt") !Fortran2003后增加了NewUnit子句，由编译器自动分配通道号，需保存这个通道号（FILE_IN）
```



| 子句项   | 作用                                | 格式                                                         |
| -------- | ----------------------------------- | ------------------------------------------------------------ |
| **UNIT** | 指明与文件关联的i/o单元号（通道号） | UNIT=int_expr(非负整数)                                      |
| FILE     | 指定要打开的文件名                  | FILE=char_expr                                               |
| STATUS   | 指定要打开文件的状态                | STATUS=char_expr (OLD,NEW,REPLACE,SCRATCH,UNKNOW)            |
| ACTION   | 指定一个文件打开方式                | ACTION=char_expr (READ,WRITE,READWRITE)                      |
| IOSTAT   | 指定一个整数变量                    | IOSTAT=int_var (OPEN执行成功，赋值变量为0，否则返回错误相应正数值) |
| IOMSG    | 指定一个字符变量名                  | IOMSG=chart_var(OPEN执行成功，字符变量内容不变，否则返回错误信息) |

- new: 文件原先不存在,如果打开时文件已存在，则会报错。 
- old: 文件已经存在, 如果打开时文件不存在，则会报错。 
- replace: 如果文件已经存在则会覆盖老文件, 如果不存在则新建。 
- scratch: 文件会被自动命名, 系统将会打开一个**暂存盘**, 程序运行结束后文件消失。 
- unknown: 不同编译器不同, 一般为replace。

```
write(*,*) "nice to meet you!" !第一个*表示输出位置使用默认值，即屏幕；第二个*表示不特别设置输出格式(让变量列表自动控制格式)；

read(12, *) a,b,c,d !12是文件通道号
```

```
PROGRAM test
INTEGER::i,j,k
READ(*,*)i,j
k = i*j
WRITE(*,*)"result=",k
STOP
END PROGRAM test
```

```
Program Readtwodimension
   Implicit None
   Real :: a(3,4) !定义二维数组
   Integer :: 
   File_in , i !定义File_in(文件通道号)
   Open(NewUnit = File_in, File = "2d_text.txt") 
   Do i = 1, size(a, dim =2)
      Read(File_in, *) a( : , i ) !a( : , i )表示每次循环读取一行
      write(* , *) a( :, i )
   End Do
   Close(File_in)
End Readtwodimension  

！亦可用隐式循环
Program Readtwodimension
   Implicit None
   Real :: a(3,4) !定义二维数组
   Integer :: File_in , i !定义File_in(文件通道号)
   Open(NewUnit = File_in, File = "2d_text.txt") 
   Read(File_in , *)(a(j,i), j=1:3)
   Close(File_in)
End Readtwodimension
```



### READ语句

#### **顺序读取有格式文件**（文本文件）

- 顺序读取需从前往后依次读取（可以想象成读磁带）
- 一条read()语句读取一行；下一条read()语句不会从上一行结束的位置开始读取，而是重新一行开始读。
- 只用fortran可以用顺序读写；与其他软件/语言交互，通常不使用顺序读写；
- 

```
!顺序读取有格式文件（二维数组）
Program main
  Implicit None
  Real :: a( 3 , 4)
  integer :: File_in , i
  Open( NewUnit = File_in, File = "2d_text.txt")
  Do i = 1, size(a, dim = 2)
     Read( File_in , *) a( : , i)
     write( * , * ) a( : , i)
  End Do
  Close(File_in)
End main  
```

```
!read()顺序读取（跳过n行）
!read先读行，再读列
Program Readpartial
  Implicit None
  Real :: a, r
  Integer :: File_in, i
  Open(NewUnit = File_in, File = "2d_text.txt")
  Read( File_in , * ) !read()括号后面什么都不写，跳过了第一行
  Read( File_in , * ) !第二行也跳过了
  Read( File_in , * ) r, r, a ！读取第三行中的第三列的数值a; r的作用是用于跳过，即通过读取一个不使用的变量r跳过若干列；一个r表示跳过一列
  write( *, *) a
End Program Readpartial  

！可以改为隐循环
Read(File_in, *)((r),i=1,9),a !循环9次，跳过了9列，读第10列的数
```



#### **直接读取有格式文件**（文本文件）

- 不建议使用
- 直接读取可以跳转，但要求每记录（行）的长度一致;
- 如果每行长度不一样，可以使用空格补充，但比较难
- 直接读取在read()时必须指定读取格式和读取记录，因此对文件的格式要求较高，不能使用表控格式

```
Open( NewUnit = FID, File = "text_direct.txt", form = "formatted", access = "Direct", RecL =64) 
!form = "formatted"指定有格式文件（文本文件）； 在顺序读取时form是默认值（unformatted）可以不指定。
!access="Direct"指定为直接读取方式；在顺序读取时可以指定sequential,其为默认值可以不指定。
！RecL=64 指定记录程度record length是64位，仅在直接读取时指定

#方法1
read(FID,'(i7,3(1x, g15.6))', rec=5)i, a, b, c ！需要指定读取格式和读取记录

#方法2：使用字符串str读入，然后再使用str使用表控格式读入数据
read(FID, '(a64)', rec=5) str  !因为一定为64个长度，这样是读入第5行的整行
Read (str, *) i,a,b,c
write (*,*) i,b,c ！第一个*， 表示输出的位置，默认为屏幕，第二个*为输出的格式
close(FID))
```



#### 顺序读取无格式文件(二进制文件)

顺序写入无格式文件，以记录为基本单元，读写过程分若干笔记录：

- 每次一笔记录，在记录前后各多出4个字节，用来记录本次写入的长度。顺序读取时，通过这4个字节知悉当前记录的长度。
- 我们再读取此文件时，程序会首先读入前 4 个字节，获得本笔记录的长度。随后读入后 4 个字节，以验证是否与前 4 个字节相同（如不同则报错）最后，把中间的字节按顺序，读取到变量列表里。

```
Open(NewUnit=FID, file="example.txt", form="unformatted",access="direct", Recl=12) ！指定每个记录的最大为12


Program main
  Implicit None
  Integer :: FID， a = 1000, c = 2338021
  real :: b = 3.141592654
  character(len = 8) :: s = "fcode.cn"
  Open(NewUnit = FID, File = "myfile.bin", form = 'unformatted')
  Write( FID ) a, s !顺序写入无格式文件，以记录为基本单元，读写过程分若干笔记录。每次一笔记录，在记录前后各多出4个字节，用来记录本次写入的长度。顺序读取时，通过这个4个组合知悉当前记录的长度。
  Write( FID ) b , c !write()不能有*，因为是unformatted无格式
  Close( FID )
End Program main  
```



#### 直接读取无格式文件（二进制文件）

直接读取无格式文件，也以记录为基本单元，读写过程分若干笔记录：

- 每一笔记录的程度是固定的，在open时指定;
- 直接读写 access = 'direct', 需指定记录长度recl;
- 而不少二进制文件，其结构不规则，间或有 2 字节、4 字节 或 8 字节、甚至其他长度的数据，例如 SEGY 记录、Surfer 的 grid 网格化文件，此时使用直接读取会显得非常笨拙，常常需要多次 Open 同一个文件，使用不同的 RecL，以便读写特定的数据。

```
Open(NewUnit = FID, File = "mydirect.bin", form = "unformatted", access = 'direct', RecL = 12)
```



#### 流文件读写无格式文件（二进制文件）

流文件，是 Fortran2003 的新语法！它完胜前 2+1 种读取方式！它允许自由控制读写位置，自动判断本次读写的长度！强烈推荐！

- 流文件读写 access = 'stream'
- 文件打开后，操作位置就在文件的第一个字节上，每次读取多少字节，就自动向后移动多少字节，下一次读写紧接着此处
- 可任意跳转读写顺序
- 允许每次不一样长，无需指定recl
- 自动判断位置和长度

```
！流文件在打开时不指定recl,打开后读取位置自动在文件开始处。
Open(NewUnit = FID, File = "fcode_test.grd",form = 'unformatted', access = 'stream')

！流文件在执行read语句时，不指定rec，读取长度L根据后面的变量表自动计算。读取后，读取位置自动向后移动L个字节。一切都是自动完成的。
！流文件为无格式读写，不能指定格式，也不能写‘*’
Read(FID) flag, m, n, minX, maxX, minY, maxY, minZ, maxZ

！pos指定读写的位置，表示在第pos个字节处开始读取
read(FID, pos=5) m, n !直接跳到第5个字节读取
write(*,'("m=", g0, " n=", g0)') m, n

！经过若干read语句或者write语句后，可能不清楚当前读取位置在哪里，可以通过inquire查询
！执行inquire语句后，i值会变成FID文件当前操作位置（字节数）
inquire(FID, pos=i)

！关闭流文件
close(FID)
```



**打开文件进行READ输入**

```
INTEGER::ierror
OPEN(UNIT=8,FILE='example.dat',STATUS='OLD',ACTION='READ',IOSTAT=ierror,IOMSG=err_string)
```

**打开文件进行WRITE输出**

```
INTEGER::ierror,unit
CHARACTER::err_string,filename
unit=25
filename='OUTCAR'
OPEN(UNIT=unit,FILE=filename,STATUS="NEW",ACTION='WRITE',IOSTAT=ierror,IOMSG=err_string) !创建OUTCAR文件
```

```
program outputdata   
implicit none
real, dimension(100) :: x, y  
real, dimension(100) :: p, q
integer :: i  
do i=1,100  
x(i) = i * 0.1 
y(i) = sin(x(i)) * (1-cos(x(i)/3.0))  
end do   
open(1, file = 'data1.dat', status = 'new')  !创建data1.dat文件
do i=1,100  
write(1,*) x(i), y(i)  !将x和y写入data1.dat文件中
end do  
close(1) 
end program outputdata
```

**打开一个临时文件（给临时文件指定文件名是错误的）**

```
OPEN(UNIT=12,STATUS='SCRATCH',IOSTAT=ierror)
```

#### 格式化READ语句

| 描述符 | 含义                                   | 一般格式 |
| ------ | -------------------------------------- | -------- |
| I      | 整数输入                               | rIw      |
| F      | 实数输入                               | rFw.d    |
| L      | 逻辑输入                               | rLw      |
| A      | 字符输入                               | rA、rAw  |
| X      | 水平定位(跳过输入数据中不想读取的区域) | nX       |
| T      | 水平定位(跳过输入数据中不想读取的区域) | Tc       |
| /      | 垂直定位                               | /,&      |

```
READ(*,100) increment
100 FORMAT(6X,I6) !跳过缓冲区的前六列（六个空格），内容7~12列将被解释为证书，存入increment
```

```
READ(*,'(3F10.4)') a,b,c
!假设输入：1.5    0.15e+01   15e-01
!都将被计入为1.5
```

```
Read(FID, '(i7,3(1x,g15.6))',rec=5) i, a , b , c
！i7 表示i是一个长度为7的整数
！3()对应了a, b, c 这三个值
! 1x, g15.6表示跳过一个空格之后，长度为15的浮点数
！rec=5 表示读取第5笔记录（对应文件的第5行）
```

**格式化读入对于数据的规整性要求较高，因此能用表控格式（读到字符串中）尽量用表控格式**

- 在不规则的文件读写中，经常使用：先用字符串读入，然后再从字符串里按照一定规则读数；

### WRITE语句：输出

- write(,) 输出格式包括： I, F, E, A, X
- Iw：以w个字符的宽度输出整数，至少输出m个数字；

```
write(*, "(I5)")100 ！输出5个“100”
```

- Fw.d：以w个字符的宽度输出浮点数，小数部分占d个宽度字符

```
write(*, "(F9.3)")123.45
#输出结果为123.450,整数位不足9个字符会填上空白，小数部分不足3个字符会补0
```

- Ew.d: 科学计数法，以w个字符的宽度输出浮点数，小数部分占d个字符，

[ref](https://www.jianshu.com/p/1f0fb55caebf)









```
read/write(unit=iounit, fmt="(format string)", iostat=ios, advance='NO') variables
if ( ios /= 0 ) stop "Write error in file unit iounit"
```

- rec: 指record，直接读取文件的时候设置所要读写的文件模块外置；
- Iostat: <0 文件终止，=0操作征程，>0 读取出现错误

- I/O 格式化用法：用来指定程序打印输出变量的具体形式，如位置、有效位数等；

- `Read(* , *)`和`WRITE(* , *)`

  - ( * , * )表示含有读入操作的控制信息，第一个数据域表示哪个**输入／输出单元**（或io单元）读入数据（输入／输出单元）。这个域中的**星号**意味着数据是从**计算机的标准输入设备上读入**，通常在交互模式下是键盘。

  - 圆括号的第二个数据域：指明读入**数据的格式**。第二个`*`号均表示采用**自由格式**，此时输出中经常有多个额外的空格，且格式往往不统一。因此，可以采用**格式化**输出模式（采用**格式描述符**），来指定具体样式。

  - 如果有多个变量，用逗号进行分隔；
  - read()读取尽量采用表控格式（*）

```
Read(*,*) i 
```

[refer](https://blog.csdn.net/weixin_43780543/article/details/89928567)

#### 格式化WRITE语句

- 格式可用来指定程序打印输出变量的确切方式。通常来说，格式可以指定变量在纸上的水平和垂直位置，以及要打印输出的有效位数。
- format语句包含write语句所使用的格式化信息

```
! 用于整数i和实数变量result的典型格式化WRITE语句
WRITE(*,100)i,result !100是描述如何打印输出i和result数值的FORMAT语句的语句标号
100 FORMAT('The result for iteration ',I3,' is ',F7.3)  !100是语句标号；I3和F7.3分别是变量i和result相关的格式描述，分别将i和result用I3和F7.3形式输出
```

```
INTEGER:: a = 123
WRITE(*,100) a
100 FORMAT( '  ' , I5 )    ! 100是语句标号，将a变量用I5形式输出(输出5个字符)； '  '前面放置空格，实际上可以不要
```

```
INTEGER:: a = 123
WRITE(*,"(I5)") a     ! 单引号也可以，如 WRITE(*,'(I5)')
```



### CLOSE语句

close语句关闭一个文件并释放与之关联的I/O单元号。

- close_list中必须包含一个指定o/i单元号的子句。
- 如果在程序中没有包含对给定文件的CLOSE语句， 这个文件将在程序结束运行时被自动关闭

```
CLOSE(close_list)
```

## 文件定位

需要在执行期间多次读取一块数据或者多次处理整个文件。

**BACKSPACE**

每次调用都可以回退一条记录：

```fortran
BACKSPACE(UNIT=unit)
```

**REWIND**

从文件头重新开始文件：

```fortran
REWIND(UNIT=unit)
```

**两条语句还可以包含IOSTAT和IOMSG，检测回退或倒回操作期间是否发生错误，以不引起程序异常中断。**



## 格式描述符

有许多不同的格式描述符。 它们分为四个基本的类别： 

1. 描述文本行**垂直位置**的格式描述符。

2. 描述行中数据**水平位置**的格式描述符。

3. 描述特定数值的**输出格式**的格式描述符。

4. 控制格式中一部分的重复的格式描述符。[ref](https://blog.csdn.net/weixin_40903417/article/details/107420816)

   

| 符号 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| c    | 列号（一行中的第c个字节位置）                                |
| d    | 实数输入或输出的小数位右边的位数（不足则四舍五入）           |
| m    | 要显示的最小位数；至少输出的位数，若为2，则至少输出两位，即是变量的字符数小于2，也要输出两位 |
| n    | 要跳过的空格数（插入空格的数目）                             |
| r    | 重复计数，一个或一组描述符的使用次数（描述符的使用次数，如`2I2`，表示紧邻的两个变量用`I2`格式输出，而不需要重复写两次） |
| w    | 域宽，输入或输出占用的字符数                                 |

| 描述符 | 含义                                                         | 一般格式                                     |
| ------ | ------------------------------------------------------------ | -------------------------------------------- |
| I      | 整数输出                                                     | rIw（重复输出r次，每次输出前w个字符）、rIw.m |
| F      | 实数输出                                                     | rFw.d                                        |
| E      | 实数输出，指数表示(0.1~1e)                                   | rEw.d (w$\ge$d+7)                            |
| ES     | 真正的科学计数(1~10e)                                        | rESw.d(w$\ge$d+7)                            |
| L      | 逻辑输出                                                     | rLw                                          |
| A      | 字符输出                                                     | rA、rAw                                      |
| X      | 水平定位(插入空格数n)                                        | nX                                           |
| T      | 水平定位(跳入指定列c,可能会重叠)，移动至同一数据行某一字符位置 | Tc                                           |
| /      | 换行，改变输出行(一般结合&进行代码分行)                      | /,&                                          |

```
CHARACTER(len=10)::first_name = 'James'
CHARACTER::initial = 'R'
CHARACTER(len=16)::last_name = 'Johnson'
CHARACTER(len=9):: class1 = 'COSC 2301'
INTEGER::grade = 92
WRITE(*,100)first_name,inital,last_name,grade,class1
100 FORMAT(A10,1X,A1,1X,A10,4X,I3,T50,A9)
!A10: 字符输出first_name
!1X:插入一个空格
!结果：James       Johnson        92                   COSC 2301
```

**I （整数输出）**

- 整数在输出域内为右对齐；
- 字符个数超出域宽，输出时会用`*`号填充；
- `I0`是特殊描述符，可以输出任意整数，而不需担心域宽不足。

```
PROGRAM my_test
IMPLICIT NONE
INTEGER:: a = -123 , b  , c = 5 , d = 12345
b = a+123
WRITE(,100) a , b , c , d
WRITE(,101) a , b , c , d
WRITE(,200) a , b , c , d
WRITE(,201) a , b , c , d
WRITE(,300) a , b , c , d
WRITE(,400) a , b , c , d
100 FORMAT( ’ ’ , I5 , I5 , I6 , I10)
101 FORMAT( ’ ’ , 2I5 , I6 , I10) ! 用了rIw中的r
200 FORMAT( ’ ’ , 2I5.0 , I6 , I10.6) ! 用了rIw.m中的r和m，m是至少显示的位数，这里是0，则输出必须要≥0位，由于b的数值为0，因此没有显示值
201 FORMAT( ’ ’ , 2I5.2 , I6 , I10.6) ! 输出必须≥1位，b要显示出“00”
300 FORMAT( ’ ’ , 2I5 , I6 , I3) ! d的数值有5位，但域宽只有3位，装不下，因此输出“ * ”作为警告s
400 FORMAT( ’ ’ , 4I0) ! 特殊零长度描述符，使整数完整显示出来，此时的域宽w就是整数的长度，总是能够容纳进去
STOP
END PROGRAM my_test
```



# 数据

# 枚举 parameter



```
INTEGER, PARAMETER::SUN=0,MON=1,TUE=2,WED=3,THU=4,FRI=5,SAT=6
```



# 数组 dimension

[REF](https://zhuanlan.zhihu.com/p/593583073)

- 数组，除了所有元素之外，还有维度、每个维度的上下限、大小等信息。

- 最多可以声明7个维度

## **一维数组**

长度与数组中个数相同，使用（）可以访问具体数值。

**fortran数组的索引是从1开始的，不是从0开始的**

### **声明一维数组**

type, DIMENSION(bound)[,attr]::name

type [,attr] :: name (bound)

INTEGER, DIMENSION(N) :: array  ! N表示N个元素，默认为1,2,3，...N

INTEGER, DIMENSION(A:B):: array ! 下标从a开始，至b结束，总共有b-a+1个元素

REAL :: FARR(1:100), CARR(0:99) ! 浮点型

```
!声明数组方法一
real :: a(5) !声明大小为5的一维数组，默认下标从1开始，a(1), a(2)...a(5)
real :: a(-2:2) !自定义一维数组的下标范围，包括a(-2),a(-1),a(0),a(1),a(2)这5个元素

!声明数组方法二
integer, dimension(10)::a2

!声明数组方法三
integer a3
dimension a3(10)

!声明数组方法四
Type :: person
  real :: height, weight
End type
Type(person)::a4(10) !用person这个新类型来声明数组
a4(1)%heght=180.0 !在变量后面加%来使用person类型中的元素
a4(1)%weight=70
```

CHARACTER(LEN=256) :: CONTENTS(80) !派生数据数组

REAL, DIMENSION(5) :: RARR,RXARR(10) 混合定义

```
INTEGER,DIMENSION(10)::number ! 定义长度为10的整数型数组number，同理可换成其它类型
REAL,DIMENSION(15)::voltage !DIMENSION:说明被定义数组的大小
CHARACTER(len=20),DIMENSION(50)::last_name !如上，有50个元素，每个元素为20个字符长度变量的数组
```

### 一维数组初始化(赋值)

**用赋值语句初始化**

1. 当元素较少的时候，通过数组构造器[]直接赋值，这样声明数组并赋值，要求里面的数据类型必须一致。

```
REAL , DIMENSION(10)::array1
array1 = [1. , 2. , 3. , 4. , 5. , 6. , 7. , 8. , 9. , 10. ] !数组的初始化
```

2. 元素较多且有规律的时候，结合Do循环语句 （数组的访问通常和循环语句绑定在一起）

```
REAL , DIMENSION(100)::array2
DO i = 1,100
	array2( i ) = REAL( i ) !通过循环语句，访问array2中的各个元素
END DO
```

```
Do i = 1, 5
  a(i) = i * 1.0
End Do  
```

```
Program Test
   IMPLICIT NONE
   INTEGER :: I ！I用来赋值一维数组ARR
   INTEGER, DIMENSION(5)::ARR
   DATA ARR/1, 2, 3, 4, 5/ !DATA赋值语句是fortran77中的老语法
   
   WRITE(*,'(A,3X,A)')I,ARR(I)
   DO I = 1, 5
      WRITE(*,'(2I5)')I,ARR(I)
   ENDDO
   
   PRINT *
   PRINT(*,'(A,1X,A)')'MAX_LOC','MAX_VAL'
   WRITE(*,'(2I5)')MAXLOC(ARR),MAXVAL(ARR) !MAXLOC()访问数组中的最大元素的位置；MAXVAL()访问数组中最大元素的值
   
   PRINT *,"PROGRAM TERMINATED"
END PROGRAM Test   
```

3. 数组中所有元素均为同一值，可直接赋值

```
REAL , DIMENSION(10)::array3
array3 = 1.0     ! 所有元素均为1.0
```

**使用类型声明语句初始化**

1. 元素较少时直接在声明语句中初始化

```
REAL , DIMENSION(10)::array4 = [1. , 2. , 3. , 4. , 5. , 6. , 7. , 8. , 9. , 10. ]
```

2. 元素较多且有规律时，可结合隐式Do循环语句

```
REAL , DIMENSION(100)::array5 = [(i , i =1 , 1000) ]
```

3. 数组中所有元素均为同一值，可直接赋值

```
REAL , DIMENSION(10)::array6 = 1.0  ! 所有元素均为1.0
```

**用read语句初始化**

### 数组的访问方式（使用）

**整体**



**元素**

**片段**

- 连续片段 ： A(3:6) = 0 ! 3和6分别表示下标的起始和终止位置
- 下标三元法，不连续的元素组成数组片段 A(10 : 30 : 2), A( : 20 : 4), A(8 : : 10)
  - 数组名(e1 : e2 : e3), e1表示起始下标，e2表示终止下标，e3表示间隔（步长）

**不连续段**



### **一维数组运算**

1. 当两个数组的结构（行和列数）相同时，可用内置操作符（如`+`）进行操作，此时两个数组的下标值不一定要相同：

```
PROGRAM array_test
IMPLICIT NONE
REAL , DIMENSION(4) :: a = [1. , 2. , 3. , 4.] 
REAL , DIMENSION(-1:2) :: b = [5. , 6. , 7. , 8.]   ! 下标不一致也不影响结果
REAL , DIMENSION(4) :: c    
    
c = a + b    
WRITE(*,*) c
WRITE(*,*) -c       ! 所有元素取反号

STOP
END PROGRAM array_test
```

**一维数组切片**

array( i : j : m), i为起始下标，j为结束下标，m为增量

```
PROGRAM array_test
IMPLICIT NONE
REAL , DIMENSION(10) :: a 
INTEGER::count,i

count = 1
DO i=1,20,2
    a( count ) = i
    count = count + 1
END DO    

WRITE(*,100) a    
WRITE(*,100) a( : )
WRITE(*,100) a( 2 : 7 )
WRITE(*,100) a( 2 :  )
WRITE(*,100) a(  : 7 )
WRITE(*,100) a( : : 2)

100 FORMAT(*(F4.1,1X))  ! *号表示无限循环使用

STOP
END PROGRAM array_test
```

**一维数组的输出**

```
REAL , DIMENSION(5) :: a 
WRITE(*,100)  a(1) , a(2), a(3) , a(4) , a(5)   ! 输出
READ(*,100)  a(1) , a(2), a(3) , a(4) , a(5)   ! 输入
100 FORMAT(5F4.1) 
```

**隐式DO循环**

当有多个数据需要输出时，通过隐式DO循环

隐式DO循环作为控制变量的函数来多次输出/输入数组数据，语句为：
WRITE(unit,format) (arg1,arg2,...,index = istart,iend,incr)
READ(unit,format) (arg1,arg2,...,index = istart,iend,incr)

```
INTEGER, DIMENSION(200)::array2=[(i,i=1,200)]
```



```
REAL , DIMENSION(5) :: a 
INTEGER::i
WRITE(*,100)  (a(i),i=1,5)
READ(*,100)  (a(i),i=1,5)
100 FORMAT(5F4.1)  !结果：0.0 0.0**** 0.0 0.0
```

```
REAL , DIMENSION(5) :: a = (/1.,2.,3.,4.,5./)   
INTEGER::i
WRITE(*,100)  (a(i),2*a(i),3*a(i),i=1,5) ! 每个i有三个数，共有15个数
100 FORMAT(15F5.1)  !  1.0  2.0  3.0  2.0  4.0  6.0  3.0  6.0  9.0  4.0  8.0 12.0  5.0 10.0 15.0
```

如果将输出格式改为100 FORMAT(5F5.1) 

  1.0  2.0  3.0  2.0  4.0
  6.0  3.0  6.0  9.0  4.0
  8.0 12.0  5.0 10.0 15.0

### **嵌套的隐式循环**

最里面的括号为内层，外面的为循环外层，如例子中的`j`是内层循环，`i`是外层循环：

```
INTEGER::i , j
WRITE(*,100) (( i , j , j = 1 , 3 ) , i =1 , 2)
100 FORMAT(I1,1X,I1) 
```

结果：

1 1
1 2
1 3
2 1
2 2
2 3

```
INTEGER::i,j
WRITE(*,110)((i,j,i=1,3),2*j,j=1,3)!j是外循环，i是内循环
110 FORMAT(30I1) ! 1    1    2    1    3    1    2    1    2    2    2    3    2    4    1  3    2    3    3    3    6
!对外层循环中的每一步，都完全执行一遍内层循环。
```

隐式DO循环与常量嵌套混合生成数组

```
INTEGER,DIMENSION(25)::array2=[((i,i=1,4),6*j,j=1,5)]
!生成数组
j=1: 1 2 3 4 6
j=2: 1 2 3 4 12
j=3: 1 2 3 4 18
j=4: 1 2 3 4 24
j=5: 1 2 3 4 30
```

**在数组声明中使用命名常数**

通过设置命名常数，很容易去改变程序中数组的大小。

```fortran
INTEGER,PARAMETER::Max_size=1000
REAL::array1(Max_size)
REAL::array2(2*Max_size)
```

建议在Fortran程序中始终用参数声明数组的大小，以保证程序很容易修改

## **二维数组**

Real :: b ( 10 : 20, 201 : 300) !第一维的下限10，上限20，大小20-10+1 =11； 第二维的下限201，上限300，大小300-201+1=100

### 声明二维数组

```
real :: a(5,3) ！5列3行
real :: a(-2:2, -2:2)
real :: a(5, 0:5) ！a(1-5,0-5)
```

### 二维数组的运算

```
Program arrayProg
  integer :: matrix(3,3), i, j !定义二维数组
  Do i = 1, 3
     Do j = 1, 5
        matrix(i, j) = 1+j
     End Do
  End Do
End Program arrayProg  
```

```
implicit none
    !二维数组作为矩阵使用
    !让用户输入两个2*2矩阵的值，再把这两个矩阵相加
    integer,parameter::row=2 ！row 行
    integer,parameter::col=2 ! col 列
    integer::matrixA(row,col)
    integer::matrixB(row,col)
    integer::matrixC(row,col)
    integer r !用来赋值row
    integer c !用来赋值col

    !读取矩阵A的内容
    write(*,*)"Matrix A"
     do r=1,row
        do c=1,col
           write(*,"('A(',I1,',',I1,')=')")r,c
           read(*,*)matrixA(r,c)
         end do
      end do

      !读取矩阵B的内容
     write(*,*)"Matrix B"
     do r=1,row
        do c=1,col
           write(*,"('B(',I1,',',I1,')=')")r,c
           read(*,*)matrixB(r,c)
         end do
      end do


    !把矩阵A,B相加并输出结果
    write(*,*)"Matrix A+B="
    do r=1,row
       do c=1,col
          matrixC(r,c)=matrixB(r,c)+matrixA(r,c)
          write(*,"('(',I1,',',I1,')=',I3)")r,c,matrixC(r,c)
       end do
    end do
```



## 数组的传递

传递数组（例如给子程序）的方式有：自动数组、假定大小、假定形状

```
Program arrayToProcedure      
  implicit none      
  integer, dimension (5) :: myArray  
  integer :: i
  call fillArray (myArray)      
  call printArray(myArray(2:))
End program arrayToProcedure

Subroutine fillArray (a)      
    implicit none      
    integer, dimension (5), intent (out) :: a
    ! local variables     
    integer :: i     
    do i = 1, 5         
      a(i) = i      
    end do  
End subroutine fillArray

Subroutine printArray(a)
    integer, dimension (4) :: a  
    integer::i
    do i = 1, 4
      Print *, a(i)
    end do
End subroutine printArray
```

**自动数组**

```
Subroutine sub(a, m, n)
  Integer m,n
  Real a(m,n)
  a = 2
End Subroutine sub !只传递地址，维度由定义确定，各维度上下限由其他虚参确定，缺点是参数多  
```

**假定大小**

```
Subroutine sub(a)
  Real a(*)
  a(1:6) = 1
End Subroutine sub !只传递地址，虚参只能是1维，下限为1，不传递上限，缺点是容易越界  
```

**假定形状**

```
Subroutine sub (a)
  Real a(:,:)
  a = 1
End Subroutine sub !传递地址，各维度大小，下限可自动，上限自动计算，缺点是需要interface  
```



# 结构体type

Type语句写在程序的说明部分：

- 用来定义变量、命名常量或其他派生类型；
- 也可以作为参数传递给子程序；
- type中的数据类型可以不一样，可以提高程序的复用性和可维护性；

```
TYPE [[,att-list]::] name [(type-param-name-list)]
  [type-param-def-stmts]
  [PRIVATE statement or SEQUENCE statement]
  [component-definition]
  [type-bound-procedure-part]
END TYPE [name]  
  
```



## %

% 表示结构中成员的一般形式：结构名%成员名，这样的成员可以像访问变量一样被访问，包括对成员进行赋值、打印、引用等；

```
TYPE :: STUDENT !声明type，type名称为student
   INTEGER :: ID !声明结构体的成员名
   CHARACTER(LEN=100)::NAME
   INTEGER::CHINESECO, MATHSCO, ENGLISHSCO
END TYPE

TYPE(STUDENT):: FLYBIRD !初始化type中的元素
FLYBIRD%ID = 001  !访问结构体成员变量的方式TYPE%MEM
FLYBIRD%NAME = "FLYBIRD"
FLYBIRD%CHINESESCO = 60
FLYBIRD%MATHSCO = 97
FLYBIRD%ENGLISHSCO = 17

```



# 类 class

类是在结构体基础上，向面向对象特性的延伸,允许程序员将数据和方法组合在一起形成一个对象。

- class是通过Type定义的，type定义了一个数据类型，而class定义了一个对象类型；
- class是一种非常强大的编程概念，可以组织和管理程序；

```
type, extends(parent) :: my_type
   real :: x
   contains
      procedure, pass(this) :: double_x => double_x 
contains

   subroutine double_x(this)
      this%x = 2 * this%x ! Double x field
   end subroutine
end type
```



```
Type :: Person !定义了一个名为Person的class,其有两个私有变量(只能在class内部访问)和三个公共成员函数procedure（可以在class外部访问）
  Private
    Character(20) :: Name
    Integer :: Age
  Public
    Procedure :: SetName
    Procedure :: SetAge
    Procedure :: PrintInfo
End Type Person

Program Main
  Use Person
  Type(Person):: John
  
  John = New(Person)
  call John%SetName('John')
  call John%SetAge(30)
  call John%PrintInfo()
End Program Main  
```

```
class(urbantv_type) :: this 
!class指定父类型; this可以是一个urbantv_type或者扩展它的派生类型，允许多态性

type(bounds_type) , intent(in) :: bounds 
!type定义派生类型; bounds使用特定声明type，具有具体的bounds_type派生类型
```



### this

- This 允许对象访问它自己的字段和方法
- this%对象字段
- this%方法
- this不是变量，而是编译器解释的特殊关键字

```
type, public :: urbantv_type
   real(r8), public, pointer :: t_building_max(:)
   type(shr_strdata_type)    :: sdat_urbantv
...
end type urbantv_type

subroutine Init(this, bounds, NLFilename)
! !ARGUMENTS:
    class(urbantv_type) :: this !在class定义中，this指的是类实例本身
    ...
    allocate(this%t_building_max(begl:endl)); 
    this%t_building_max(:) = nan
    ...
end subroutine Init
```



# 指针 Pointers

在大多数编程语言中，指针变量存储对象的内存地址。 但是，在Fortran中，指针是一种数据对象，它具有比仅存储内存地址更多的功能。 它包含有关特定对象的更多信息，如类型，等级，范围和内存地址。

## 声明指针变量

```
integer, pointer :: p1 ! pointer to integer  整数型指针
real, pointer, dimension (:) :: pra ! pointer to 1-dim real array  一维数组型指针
real, pointer, dimension (:,:) :: pra2 ! pointer to 2-dim real array  二维数组行指针
```

指针可以指向：

- 动态分配内存的区域
- 与指针具有相同类型的数据对象，具有**target**属性；



## Allocate()为指针分配空间

```
program pointerExample
implicit none
   integer, pointer :: p1
   allocate(p1)
   p1 = 1
   Print *, p1
   p1 = p1 + 4
   Print *, p1
end program pointerExample
```

- 当不再需要时，应该通过**deallocate**语句清空分配的存储空间，并避免累积未使用和不可用的内存空间。

### 目标target和协会

```
program pointerExample
implicit none
   integer, pointer :: p1
   integer, target :: t1 
   p1=>t1 !"=>"关联运算符，将指针变量p1与目标变量t1相关联
   p1 = 1
   Print *, p1
   Print *, t1
   p1 = p1 + 4
   Print *, p1
   Print *, t1
   t1 = 8
   Print *, p1
   Print *, t1
   nullify(p1) !nullify()语句将指针与目标解除关联
end program pointerExample
```



# Fortran 错误汇总

**Program received signal SIGSEGV: Segmentation fault - invalid memory reference.**

- 会把很多内存的问题都归结为这个错误

### 程序之间的链接错误

**Undefined symbols for architecture arm64** 工程某些地方不支持arm64指令集 [ref](https://stackoverflow.com/questions/66855252/what-is-an-undefined-reference-unresolved-external-symbol-error-and-how-do-i-fix)

- 当main program 主程序调用subroutine子例程时

  ```
  gfortran -c module.f90
  gfortran program.f90 -o program 
  ./program !这样子会报错
  ```

```
!方法一
gfortran -c module.f90
gfortran module.o program.f90 -o program
./program
```

```
!方法二
gfortran -c module.f90
gfortran -c program.f90
gfortran module.o program.o -o program
./program
```

### debug tool

[Linaro Forge](https://www.linaroforge.com/downloadForge/)



# Fortan 内置函数检索

- fortran 函数有严格的参数类型和返回值；

- forran内部函数是逐元函数（elemental）, 可以接受任意维度的矩阵或者数组

  ```
  Do i = 1, N
     y(i) = sin(x(i))
  End Do !无需通过循环来使用y=sin(x)
  
  Real :: x(3,5), y(3,5) ! 3行5列
  y = sin (x)
  y(1:2,:) = sin(x(1:2,:)) !取数组片对
  ```

## abs()

Abs() 绝对值函数



## adjustl()

str = adjustl ( str)  !左对齐字符串（返回字符串长度不变）

str  = ajustr( str ) !右对齐字符串（返回字符串长度不变）



## All()

b = all( array [, dim]) 判断逻辑数组元素是否全是真

```
integer :: a(5) = [1,2,3,-5,3]
if (all (a>0)) then
    a
    a>0 
    all(a>0) !结果.false.
```



## allocate()

Allocate()用于动态分配内存：

```
allocate(array(dimensions)) !array是目标数组的名称，dimensions是一个整数数组，用于指定数组的维度

real, allocatable :: a(:) !声明一个实数数组，数组名为a，是一维数组； allocatable表示数组a是可分配的；
allocate(a(10)) !分配给数组a 10个元素的内存空间

real, allocatable :: b(: , : ) !声明一个二维数组
allocate(b(10,20)) !给二维数组分配了一个10*20个元素的内存空间
```



## any()

b = any(array[ , dim]) 判断逻辑数组元素是否至少有一个是真

All()和any()可以相互转换

- if ( all (满足) ) 等效于 if( .not.any( .not.满足 ) )



## associate()

Associate() 将复杂的语句构造为简单的缩写，用于在一个代码段执行过程中，临时用一个别名代替某变量或者表达式的原名，当变量或者表达式很长时，这种结构能够简化代码

```
associate(
    x => active_tracks(i)%x
    y => active_tracks(i)%y)
    dist = sqrt((rada%x - x) ** 2 + (rada%y - y) ** 2)
end associate
```



```
program associateTest

  implicit none
  integer :: a=1,b=1
  associate( x => a*b )
    print *, x                ! yields: 1
    a=10
    print *, x                ! yields: 1
  end associate

  associate( x => a )
    print *, x                ! yields: 10
    a=100
    print *, x                ! yields: 100
  end associate

end program associateTest
```

```
！associate()适用于大型项目中
program associateTest
  implicit none
  integer :: a=1,b=1
  someName: associate( x => a*b )
    print *, x                ! yields: 1
    a=10
    print *, x                ! yields: 1
  end associate someName
end program associateTest
```



## Attr()

- to access attributes of derived data types;
- The first argument to Attr() is the attribute number, starting from 1. This selects which attribute to access.
- The second argument is the array index if the attribute is an array. Omitted for scalar attributes.
- Attr() returns the value of the specified attribute.

```
type my_type
  real :: attr1
  integer, dimension(10) :: attr2
end type

type(my_type) :: var

! Access scalar attribute 
print *, var%Attr(1) 

! Access array attribute element
print *, var%Attr(2,5)
```



## Avs()

The avs() function in Fortran is used to access elements of an allocatable array that is a component of a derived type.

Fortran中的avs()函数用于访问可分配数组的元素，该数组是派生类型的组件。

```
type my_type
  real, allocatable :: var1(:)
  real, allocatable :: var2(:,:) 
end type

type(my_type) :: obj
allocate(obj%var1(5)) 
allocate(obj%var2(10,2))

! Access first allocatable array 
obj%avs(1) = some_value  

! Access second allocatable array
obj%avs(2)(3,2) = some_other_value

# avs(1) returns var1, so we can assign to it；
# avs(2) returns var2, so we can assign to it
```



## ceiling()

Ceiling(r) ！天花板函数，返回大于等于x的最小整数



## dot_product(a, b)

Dot_product (a, b) 内积函数



## Index()

i = index( c , cf [, back])

- 在字符串c中查找cf第一次出现的位置，可指定back = .true（表示从后面开始查询）

```
c = "My Name is fcode"
i =  index(c, "is")
write(*,*)c(i+3) !输出”fcode“
```

```
filename = "D:\mydata\2015\20150314.txt"
i = index(filename, "\", back = .true.)
write(*,*)filename(i+1) !输出”20150314“
```



## Int()

Int(x)把x转换为integer类型

- r=int(x, kind = 8) 把x转换为kind = 8的integer 类型（长整型）



## len()

i = len( str ) !返回字符串str的长度

i = len_trim(str) !返回字符串的长度（trim之后的长度）

## Matmul()

Matmul (a, b) ！m * n矩阵和n * l矩阵相乘，得到 m * l 矩阵



## maxval()

n = maxval( array [ , dim] [ , mask]) 获取数组中最大值

- Maxval()可以附带条件，也可以指定dim维度

```
integer :: a(5) = [1,2,3,-5,3]
maxvel( a, mask = (a<3)) !求a中所有小于3的最大值
```



## mod()

Mod(a, b) ！取余数

## nint()

Nint(x) 把x四舍五入并转换

## read()

Read(字符串, *) 其他数值 ！将字符串转为其他类型

- read(str, *) i ! i = 9.0

## real()

Real(x) 把x转换为real 类型

- r = real (x, kind = 8) ! 把x转换成kind=8的real类型（双精度）
- real()函数代替了float()和dble()的功能



## reshape()

arrayB = reshape (arrayA, shape) 重新设置数组的外形

```
integer :: a(6) = [1,2,3,-5,3,0], b(3,2) !定义数组a和b
b = reshape (a, shape (b))
b = reshape (a, [3,2]) ！shape(b) == [3,2]
!输出结果 b = [1, 2, 3
             -5, 3, 0]
```



## scan()

i = scan(c, cf[ , back]) 在字符串c中查找cf中的每一个字符，返回任意目标字符第一次出现的位置，可指定back = .true 从后面查询

```
c = "3*4=12"
i = scan(c, "=*") ! 查找=或*第一次出现的位置，返回值i=2
```



## Selected_int_kind(r)

Selected_int_kind(r) 获得所需证书精度

Selected_real_kind([p,r]) 返回一整个函数，该整数是给定的十进制精度p和十进制指数范围r所需的类型参数值.p：是精度的缩写;r是范围的缩写。十进制精度是有效数字的个数，十进制指数范围指定最小和最大的可表示数字。The range is thus from 10-r to 10+r.

- 例如，selected_real_kind (p = 10, r = 99)返回精度为10位小数、范围至少为10-99到10+99所需的kind值。



## Transpose(a)

Transpose(a) 把m * n矩阵转置为n * m矩阵



## Trim()

str = trim( str )  ！去掉字符串尾部的空格（返回短的字符串）

## write()

Write(字符串, *) 数值  ！将其他类型转为字符串

- Write(str, *) 9.0  ! Str = '9.000000' 



# Stream

## field

- A field refers to a single data item or value in a record. For example, a record representing a person might have fields for name, age, height, etc.

## field list

- A field list specifies the fields that are present in a record and their order.

```
CHARACTER*20 NAME
INTEGER AGE
REAL HEIGHT

# This defines a field list with 3 fields - a 20 character string NAME, an integer AGE, and a real HEIGHT.
```



# nc文件读取

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

    status = NF90_INQ_VARID(fidA,"lat",LAT_ID)
    call erreur(status,.TRUE.,"inq_LAT_ID")

    status = NF90_INQ_VARID(fidA,"lon",LON_ID)
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
```

```
# 编译&执行：
NETCDF=xxx/netcdf/4.4.1  
NC_INC="-I ${NETCDF}/include"    
NC_LIB="-L ${NETCDF}/lib -lnetcdf -lnetcdff"  
ifort -c $NC_INC io_netcdf.f90  
ifort -o run_example io_netcdf.o $NC_LIB  
./run_example
```

