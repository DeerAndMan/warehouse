# HOC 高阶组件
***创建时间 2021-06-23***

## HOF: 高阶函数，以函数作为参数，并返回一个函数
```
    function(func){
        return function(){}
    }
```

## HOC: 高阶组件，以组件作为参数，并返回一个组件
+ 通常使用`With`开头
+ 高阶组件一般不存在其他的操作，通常情况下是用来增强功能的。
+ 通常，可以利用HOC实现横切关注点(每个组件在创建和销毁时的日志记录)
```
export default function withLog(Comp){
    return class LogWarpper extends React.Component {
        componentDidMount() {
            console.log(`日志：组件${Comp.name}被创建了！${Date.now()}`)
        }
        componentWillUnmount() {
            console.log(`日志：组件${Comp.name}被销毁了！${Date.now()}`)
        }

        retnder() {
            // 将组件传递出来的参数返给原来的组件
            return <Comp {...this.props}>
        }
    }
}
```
```
export default function withLogin(Comp) {
    return function LoginWrapper(props) {
        if(props.isLogin){
            return <Comp {...props}>
        }
    }
}
```


**注意**
1. 不要将高阶组件(HOC)放在render中使用
2. 不要在高阶组件内部更改传入的组件