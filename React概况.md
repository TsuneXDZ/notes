## QUICK SATRT

> Use `npx create-react-app my-app` create react project.

## 组件
> 组件允许将UI划分为独立的模块。在React中，一个组件大多数情况为一个函数

```js
function App() {
  return null;
}
```


**React 对组件的要求**

1. 组件名必须大写开头
2. React组件必须返回可以渲染的React Respond Element
    - null
    - React元素
    - 组件
    - 可以迭代的对象[数组，set,map...]，对象只需要追加迭代接口可以被渲染


React 的组件具有可重用性


### 组件状态
> 即组件内的数据，组件状态的更新是异步的。

在单击`increase`或者`decrease`后`{count}`的值在界面中没有发生改变。但是在log中可以发现已经增加或减少了。
这是因为最终执行的代码是通过babel编译以后的代码
即`React.createElement('span',{},count);`
我们需要重新执行组件。

**组件什么时候会重新执行？**
1. 组件状态发生变化。
2. 组件属性发生变化。

使用官方给定的API `import { useState } from "react"`
```js
const [count, setCount] = useState(0);  //count: prop, setCount:update function, useState(0):init
const increase = ()=>{
    setCount(prev => prev + 1);
}
```


如果想让页面与数据同步，我们必须使用useState。因为useState会在数据变更时重新渲染组件

#### useState的使用

1. 在调用时可以传递函数或者具体的值，会将函数返回值或具体的值作为初始化的状态。
尽量不要传递函数，因为初始化只会进行一次。
2. useState返回一个数组，数组有两个成员：
      - 以初始化为值的变量。
      - 修改该变量的函数。
        - 该函数可以传递值或者函数，如果传递的是函数，会将函数放入一个队列等待执行。
3. 状态更新是批量进行的。

### 组件属性
> 就是函数属性

React 中的属性分为：
  - 标签属性
  - 组件属性

## 事件
> 原生事件大差不差

同样的，事件和属性一样，也需要与界面同步。传递事件属性。

### 事件机制
在jsx中，所有的事件都不是原生的事件，如e.stopPropagetion，
React会将所有的属性集中处理。

React事件池 React17以上放弃
{
  React会尝试重用事件，即将引用放入事件池， 只是改变对应的属性值。
  因此不要再17版本以下使用异步访问事件源对象的属性。
  不过使用`e.persist()`会关闭事件池重用。
}

## 受控和非受控
在React中，只有表单属性需要讨论这个。因为只有表单组件涉及交互。
- 非受控元素只能通过defaultValue设置初始值，通过事件监控。
- 受控元素 通过变量代码控制，例如input中的value，checkbox中的checked...
```js
//受控
    ...
    <input value={data} onChange={(event)=>setData(event.target.value)}/>
    <input type="checked" checked = {checked} onChange={()=>setChecked(event.target.checked)}>
    ...

```
受控组件可以在特殊情况下操作DOM的值。

## hooks
> React16.8推出的新特性，允许开发者在不写类组件的情况下生成状态以及类组件专有的东西。

**再谈useState，它就是一个hooks。**
const [state,setState] = useState(func) 这个func必须是纯函数，并且不能有参数。
useState 必须在顶层或自定义hooks中定义。
setFunction是异步的，并且是批量更新的。

**useEffect,专门用来处理副作用的**
什么是副作用？不依赖React功能的外部操作。例如http请求、dom操作、或者异步请求。
尽量使用useEffect处理副作用。

Reference
useEffect(setup,dependencies)
- setup `函数` 每次挂载后执行
  - 通过return 一个函数移除事件
- dependencies `依赖项` 当依赖项发生变更时重新执行setup函数。
```js
  useEffect(()=>{
    console.log("hello");
  },[bool])
```

在改变bool值后，成功输出hello；


**实际应用场景**
1. 做HTTP请求
```js
    const [StudentList,setStudentList] = useState([]);
    const getStudentListFromServer = ()=>{
        return new Promise((resolve,reject)=>{
            setTimeout(()=>{
                setStudentList([{name:"jone"},{name:"chen"}])
            },2000)
        })
    }
    useEffect(()=>{
        getStudentListFromServer();
    },[]);
    return (
        <div>
            {
                StudentList.map(student => (
                    <div>{student.name}</div>
                ))
            }
        </div>
    )
```

2. **访问真实DOM**
```js
  //在useEffect中可以直接获取
  const studentDom = document.getElementsByClassName("getDom")[0];
  console.log("Dom=>" ,studentDom);
```


**清理函数**
通过在setup中设置return一个函数，例如return ()=>{document.removeEventListener("event",eventHandler);}清理事件。
或者

```js
    const [tickTime,setTickTime] = useState(100);

    useEffect(()=>{
        let timer = setInterval(()=>{
            setTickTime(prev => prev-1);
            console.log("计时器工作");
        },1000)
        return ()=>{
            clearInterval(timer);
            timer = null;
        }
    },[])

    return (
        <div>剩余抢购时间：{tickTime}</div>
    )
```

​      
