## 1、C语言内存

五个存储区：代码区，全局变量与静态变量区，堆，栈，常量区

* 代码区：存放二进制代码
* 全局变量与静态变量区：分为data段和bss段，其中data段存放已初始化的变量，bss段存放未初始化的变量
* 堆：由程序员自由分配的存储区
* 栈：存放一些局部变量，函数传参，返回值等
* 常量区：存放字符串和const修饰的常量等



堆的分配：

使用`malloc()`申请任意大小字节内存，使用`free()`释放内存

注意：堆区**不会**在分配内存时进行初始化(包括清0)，需要程序员初始化

例如：

```c
char *p;
p=(char *)malloc(10);//分配十个连续字节空间，需要进行强制类型转换

free(p);//释放内存
p=NULL;//释放内存后需要进行初始化，避免野指针
```

> 若`malloc()`返回指针丢失，所分配内存无法回收，称为内存泄露，必须妥善保存返回指针





## 2、面向对象思想

头文件:

```c
//父类
typedef struct{
    /* 属性 */
    int age;
    int weight;
    
    /* 方法 */
    struct AnimalTable *vptr;
    
}pig;

//父类方法实现表
struct AnimalTable {
    void (*eat) (pig *who);
}

void pig_eat(pig *who); //父类虚函数实现
void pig_init(pig *who,int age,int weight);//父类构造函数


//子类
typedef struct {
    pig parent; //继承父类属性
    
    /* 添加自己的属性 */
    int lenght;
}Dag;



```

.c文件:

```c
#include "animal.h"

void pig_eat(animal *this)
{
    //具体函数实现
}

/* 构造函数 */
void pig_init(pig *who,int age,int weight)
{
    /* 1.创建虚函数表 */
    struct AnimalTable table1;
    table1.eat=pig_eat;
    
    /* 2.初始化结构体 */
    who->vptr=&table1;
    who->age=age;
    who->weight=weight;
}

void Dag_eat(Dag *who)
{
    //具体实现
}

void Dag_init(Dag *who,int age,int weight,int length)
{
    /* 1.调用父类构造函数 */
    pig_init(&who->parent,age,weight);
    
    /* 2.定义自己的虚函数表 */
    struct AnimalTable table2;
    table2.eat=Dag_eat;
    
    /* 3.初始化结构体 */
    who->parent.vptr=&table2;
    who->length=length;
}


//根据传入对象不同，实现多态
void animal_eat(pig *who)
{
    who->vptr->eat(who);
}


int main()
{
    Dag d;
    
   	Dag_init(&d,1,10,60);
    
    pig *p=&d;
    
    animal_eat(d);//多态
}
```

