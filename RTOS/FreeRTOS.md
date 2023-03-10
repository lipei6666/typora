# FreeRTOS

### 1.创建任务

```c
#include “FreeRTOS.h”
#include “task.h”
BaseType_t xTaskCreate(TaskFunction_t pvTaskCode,
                       const char *const pcName,
                       unsigned short usStackDepth,
                       void *pvParameters,
                       UBaseType_t uxPriority,
                       TaskHandle_t *pxCreatedTask);
```
参数说明：
**pvTaskCode**---任务实现的函数指针
**pcName**---任务名字
**usStackDepth**---任务堆栈大小，单位为字
**pvParameters**---任务传参
**uxPriority**---任务优先级
**pxCreatedTask**---返回的任务句柄---TCB控制块



TCB控制块结构体
```c
typedef struct tskTaskControlBlock{
    volatile StackType_t	*pxTopOfStack;  //栈顶的指针
    ListItem_t			xStateListItem;
    ListItem_t			xEventListItem;		
	UBaseType_t			uxPriority;			//优先级
	StackType_t			*pxStack;	        //栈的起始位置

}
```
函数任务实现的实现地址和传参为什么不在TCB结构体里？
因为在该函数栈里会保存好相应的寄存器，例如PC的值保存的是函数地址，R0保存函数参数

为何有两个栈地址？
因为操作系统会在一开始使用一个大型数组分配一个大的空间，提供给每个任务的栈使用，所以需要有一个栈的起始位置



### 2.删除任务

```c
void vTaskDelete( TaskHandle_t xTaskToDelete );
```
参数说明：
xTaskToDelete---要删除函数的句柄，传入NULL时，删除自己

注意：对于自杀的任务，他的内存由空闲任务回收，对于删除别人的函数，内存由删除方进行回收。

----------

### 3.任务优先级

数值越大，优先级越高

```c
UBaseType_t uxTaskPriorityGet( const TaskHandle_t xTask ); //获取优先级，传入NULL表示获取自己

void vTaskPrioritySet( TaskHandle_t xTask,UBaseType_t uxNewPriority );//改变优先级
```

####4.任务状态

* 阻塞态：
   等待两种类型的事件：时间相关、同步事件
* 暂停态：
```c
    void vTaskSuspend( TaskHandle_t xTaskToSuspend ); //传入NULL表示暂停自己
```
要退出暂停需要其他任务调用`vTaskResume()`

* 就绪态：等待调度

状态转换图：

![265783417220845](https://liepinote.oss-cn-beijing.aliyuncs.com/test/202303101305748.png)

----------

### 5.空闲任务及钩子函数

* 空闲任务是由调度器初始化时创建的
* 空闲任务优先级为0：它不能阻碍用户任务运行
* 空闲任务要么处于就绪态，要么处于运行态，永远不会阻塞
* 空闲任务每次调用都会调用钩子函数，我们可以在钩子函数中来实现一些代码

例如：执行一些低优先级的、后台的、需要连续执行的函数
测量系统的空闲时间：空闲任务能被执行就意味着所有的高优先级任务都停止了，所以测量空闲任
务占据的时间，就可以算出处理器占用率。
让系统进入省电模式：空闲任务能被执行就意味着没有重要的事情要做，当然可以进入省电模式
了。
注意：钩子函数中不能进入阻塞、暂停状态

使用钩子函数前提：
    把这个宏定义为1：`configUSE_IDLE_HOOK`
    实现 `vApplicationIdleHook()` 函数
    

----------

### 6.让出CPU资源的方法

①使用`vTaskDdelay()`，进入阻塞状态
②使用`taskTIELD()`,主动发起一次任务切换
③可以设置不同的优先级来实现抢占


volatile功能：
任务A写变量，任务B读变量，不加volatile的话，任务B读到的值可能不是任务A写的值
在while循环中判断一个变量时，不加volatile的话，可能把变量的值读进来后就循环判断，不会多次读变量


----------

### 7.队列和信号量

* 使用队列可以传递数据，数据的保存需要空间
* 使用信号量时不需要传递数据，更节省空间
* 使用信号量时不需要复制数据，效率更高

