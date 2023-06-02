# 基础

## 最短路径

首先入门，只精学能够就业的。

## 记忆方法

苹果、金枪鱼、衣架、小鸟、望远镜、胡萝卜、猩猩、戒指、手、马桶、乌龟、旗杆、拳套、钥匙、鳄鱼、麦克风、桌子、电脑、茶杯、鸡蛋。

链表记忆：强化图片使得前后进行联系。

## 英语学习

Apple - 苹果

data - data

文字的记忆比声音或图片记忆更难，将文字升维。

语言学习不需要文字，通过概念直接关联声音。

## List容器

|   关键字   |        存储方式        | 存储类型 |
| :--------: | :--------------------: | :------: |
|    List    |        线性存储        |   泛型   |
| ArrayList  |        线性存储        |  object  |
| Dictionary | 非线性存储（顺序存储） |   泛型   |
| HashTable  | 非线性存储（无序存储） |  object  |

## action和func

System.Func<int,string> 表示定义一个int参数 string返回值的委托，最后一个类型就是返回值。

system.Action<int>表示定义一个int参数无返回值的委托。





# C#中级

## 属性

``` csharp
public int Experience
{
    get{return experience};
    set
    {
        experience = value;
    }
}
```



## 扩展方法

```csharp
public static class ExtensionMethods
{
    public static void ResetTransformation(this Transform trans) 
    //此处的this关键字将该方法加入Transform类中，此方法没有参数，参数隐形变为Transform的实例
    {
        trans.localScale = new Vector3(1,1,1);
    }
}
```

## 协程

> 基本等同与中断

```csharp
 IEnumerator  method(Transform target)
 {
     ...
 }
//use
StartCoroutine(method(target));  

```



## 四元数

### Quaternions

拥有x,y,z和w四个组件，不受万向节锁影响。

它是处理旋转的最佳方式，但是不能单独使用其中任何一个组件。



