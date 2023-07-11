## 自定义hook
**创建服务器koa**
`npm add koa` koa是node的一个服务端框架。
```js
const koa = require("koa")

const koaApp = new koa;

koaApp.listen(8888,()=>{
    console.log("koaApp start at 8888");
})
function delay(duration = 2000){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve();
        },duration)
    })
}


koaApp.use(async(ctx)=>{
    const{ path } = ctx;
    if(path === "/student"){
        const jsonData = require("./Data.json");
        await delay();
        ctx.response.body = jsonData;
    }
})
```

**创建自定义hook**
```js
import { useState } from "react";

export default function useLoading(){
    const [loading,setLoading] = useState(false);


    const executeRequest = async(promiseFn)=>{
         setLoading(true);
         await promiseFn();
         setLoading(false);
    }

    return{
        loading,
        executeRequest
    }
}
```

上面这个useLoading的自定义hook的功能是在promiseFn函数执行前后设置loading的状态。

## 强制刷新hook
在逼不得已的时候使用，一般在操作真实dom较多的情况出现。
``` js
import { useState } from "react";

export default function useForceUpdate(){
    const [,setState] = useState({});
    const forceUpdate = ()=>{
        setState({});
        console.log("对组件进行渲染");
    }

    return forceUpdate;
}
```


## useCallback
每次组件重新渲染都会重新构建所有引用值。
useCallback能够长期维护某一个函数。它会将函数创建后的引用保存，在函数重新渲染时会返回保存的引用，不需要重新创建引用。

参数：
1. 函数，在创建或者依赖项变更时渲染组件。
2. 依赖项，监控依赖项变更。


```js
 const addCount = useCallback(()=>{
    setCount(arg => arg+1);
 },[]);

const getCount = useCallback(()=>{
    console.log("获取最新的Count",count);
 },[count]);    
```


 以上代码：
 addCount中直接调用useCallback并且没有依赖项，useCallback永远只会使用初始化的状态。
 getCount中将count设置为依赖，每一次count导致的重新渲染，都会重新刷新getCount。

 ## useMemo
 类似Vue的计算属性。

 **useCallback只能缓存函数，useMemo可以缓存any。**

参数：
1. 函数，会被React直接执行，并将其返回值进行缓存。
2. 依赖项，依赖项变化时，React会重新执行第一个参数，获得最新的返回值，并进行缓存。

```js
    useMemo(()=>student.map(stu => stu.name),[student])

    useEffect(()=>{
        const _studentName = student.map(stu => stu.name);
        setStudentName(_studentName);
    },[student])
```

以上代码：
将student设置为依赖项，当其发生改变时重新调用第一个函数，并将返回值stu.name返回。


## useRef
有些时候，状态并不需要和React产生连接，也就是说某些状态不需要重新进行页面的渲染。
`useRef`就是用来处理这种情况的。它可以创建一个状态，脱离React的控制，该状态不会因为组件重新渲染初始化，也不会造成组件重新渲染。
参数： 初始值
返回值：一个对象，包含current 形式：{current1:a,current2:b}

**应用场景**
1. 函数内传递的变量。
2. 处理真实dom
```js
const xxxRef = useRef(null);

//获取真实dom赋值给状态有两种方法，1. 使用useEffect 2. 直接在标签内使用ref
1.使用useEffect
useEffect(()=>{
    xxxRef = document.getElementsByClassName("input-example")[0];
})

2.使用标签内ref
<div>
    <input ref={xxxRef} className="input-example"/>
</div>

const func = useCallback(()=>{
    xxxRef.current.domFunction;
},[])

```

## fowardRef
高阶组件：接收一个组件，返回一个新的组件
它为传递进去的组件扩展了一个ref属性。
主要用来处理父组件需要直接使用子组件的一些功能


```js
//在子组件内————————
export default forwardRef(InputTest); // 将InputTest组件传入

function InputTest(props,ref) //第二个参数就是外部传入进来的ref
//在父组件内————————
const inputRef = useRef(null);
<InputTest ref={inputRef}>
<button onClick={ clickFunction }> click me</button> //clickFunction()就是业务处理函数。
```


## useImperativeHandle
参数：
1. ref
2. 函数： 返回值会赋值给ref.current属性。
3. 依赖项： 如果依赖项不变，ref不会重新赋值。

```js
useImperativeHandle(ref,()=>{
    return count+""+Math.random();
},[count])

```

这个代码表示，只要count没有发生改变，该引用也不会发生改变。

## useContext
主要用来解决层级高的一个全局变量。如果没有useContext需要一级一级通过参数传递。
而使用useContext可以类似订阅发布协议。哪里用哪里订阅。

1. 创建上下文： ThemeContext = createContext("initValue")
2. 在全局处使用上下文组件选择范围: <ThemeContext.Provider Value={`状态`}>Context scope</ThemeCOntext.Provider>
3. 在需要使用`状态`时使用useContext(ThemeContext)注册组件，返回`状态`

## useLayoutEffect
很少使用。
useEffect执行规则：在组件首次渲染并将真实dom生成到页面之后，将对应的回调函数推入`异步`队列等待执行。
useLayoutEffect执行规则：在组件首次渲染并将真实dom生成到页面之后，将对应的回调函数推入`同步`队列等待执行。


## useTransition

const [pending,startTransition] = new useTransition();
pending: 代表是否有transition任务在执行 `?`
startTransition: 代表开启一个transition任务。
- startTransition(()=> func())，此时func函数变成了transition工作
transition工作是低优先级的，不会阻塞用户的交互，也不会让页面失去响应。

