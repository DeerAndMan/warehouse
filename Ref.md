# ref 高阶组件
***创建时间 2021-06-29***

## reference：引用
+ 使用场景：希望直接使用dom元素中的某个方法，或者希望直接使用自定义组件中的某个方法
```
    export default class Comp extends React.component {
        handleClick = () => {
            console.log(this)
            this.refs.txt.focus()
        }
    }
    render() {
        return(
            <input ref="txt" type="text>
            <button onClick="handleClick">聚焦</button>
        )
    }

```
1. ref作用于内置的html组件，得到的将是真实的dom对象
2. ref作用于类组件，得到的将是类的实例
3. ref不能作用于函数组件（函数组件中的html组件或类组件依旧能够使用ref）


ref不再推荐使用字符串赋值，字符串赋值的方式将来可能会被移除

目前，ref推荐使用对象或者函数

**对象**
通过 React.createRef 函数创建
```
    export default class Comp extends React.component {
        constructor(props){
            super(props)
            this.txt = React.createRef()
        }
        // 获取
        console.log( this.txt.current )
        render(){
            returu{
                <input ref={this.txt} />
            }
        }
    }
```

**函数**
函数的调用时间：
1. 在 componentDidMount 的时候会调用该函数
    + 在 componentDidMount 事件中可以使用ref
2. 如果ref的值发生了变动（旧的函数被新的函数替代），分别调用旧的函数以及新的函数，时间点出现在 componentDidUpdate 之前
    + 旧的函数被调用时，传递null
    + 新的函数被调用时，传递对象或Dom对象
3. 如果ref所在的组件被卸载，会调用该函数
```
    export default class Comp extends React.component {

        componentDidMount() {
            // 获取
            console.log( this.txt )
            // 在组件挂载的时候就能够使用ref了
        }

        componentDidUpdate() {
            // 组件卸载的时候，也会再次调用 ref
            console.log( this.txt )
        }

        // 获取
        console.log( this.txt )

        getRef = el => {
            this.txt = el
        }

        render() {
            return(
                <input ref=>{this.getRef} />
            )
        }
        <!-- render(){
            returu{ // 存在性能问题
                <input ref={ el => { this.txt = el } } />
            }
        } -->
    }

```

## 谨慎使用 ref
能够使用属性和状态进行控制，就不要使用ref

1. 调用真实的DOM对象中方法
2. 某个时候需要调用类组件的方法




