# xline

### Keyword
有向无环图（DAG），代码生成器，依赖注入，JS，typescript，批量，自动化

### Features
+ 它把一个对象，拆分或者组装的所有步骤的顺序方式，递归查询出来,把一个有向无环图的数据转成多颗树
```
    比如说 A 由 B 和 C 组成，C由 D或者E组成
    它会查找出
	A => B C D
	[A,B],[A,C],[C,D]
    A => B C E
	[A,B],[A,C],[C,E]
```
+ 它可以用到一些批量程序，代码生成器的工作当中，完善功能节点，自动匹配找出对应的执行链路
```
    1，获取表结构数据
    2，根据表结构生成查询语句（from 1）
    3，根据表结构生成删除语句（from 1）
    4，根据查询语句生成查询接口（from 2）
    ...
```
+ 它有点像依赖注入的框架，它会把前面的执行节点的数据，和对象和执行后的数据，给当前的执行节点对象使用
+ 它可以记录一个东西，是由什么组成。它也可以记录一个东西能做什么，从有向无环图种搜索出相关的链路

### Development

+ 计划下一步，做一些可视化的支持
+ 计划完善功能节点，让它使用起来，修复其中的bug


### Install

yarn

```bash
yarn add xline
```

npm
```bash
npm install xline
```

### Usage

+ 它包含两个装饰器（xxxDecorator），一个加载器(Context),一个节点继承类（AnyObject），一个常量配置参数（CONST）

```javascript

import { Context,AnyObject,ObjectDecorator,MarkDecorator,CONST } from 'xline';

```

+ 加载器(Context)
```javascript

import { Context } from 'xline';//引入加载器
import objs from './objects/Index';//继承了AnyObject的对象数组

let ctx = new Context(objs);
//加载对象成功
ctx.ready(()=>{
  ctx.state("ready");//从节点种，找出符合状态的链路
});
//监听得到最终执行结果
ctx.onExec((r)=>{
  console.log(r);
})

```

+ 对象
```javascript
import { ObjectDecorator,MarkDecorator,AnyObject,CONST } from 'xline';
import Obj1 from './Obj1' ;
import Obj2 from './Obj2' ;
//对当前对象进行描述，名称，节点关系，关系类型
@ObjectDecorator({
    name:"节点1",
    type:CONST.GLOBAL_TYPE.COLLECT,
    from:[Obj1,Obj2]
})
@MarkDecorator("ready")//用于监听当前的对象，符合什么样的状态
export default class Ready  extends AnyObject {
    //当前节点的功能
    public run() {
        console.log(this.getClassName()+" exec ...",this.fromData());
        return "";//返回的结果，或者当前节点的结果，交由后面的节点使用
    }

}

```

### Contributing 

### Emaiil
```
yuelinhua@126.com
```
