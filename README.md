# 前端技术规范（草稿）
## 0. 命名规则
使用英文名，避免拼音以及简写。
英文可以使用有道、Google等翻译软件。

### (1) 避免单字母命名。命名应具备描述性。

```
// bad
function q() {
  // ...stuff...
}

// good
function query() {
  // ..stuff..
}
```

### (2) 使用驼峰式命名对象、函数和实例。

```
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

### (3) 使用帕斯卡式命名构造函数或类。

```
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```
### (4) 使用下划线 _ 开头命名私有属性。

```
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';

// good
this._firstName = 'Panda';
```

### (5) 别保存 this 的引用。使用箭头函数或 Function#bind。
```
// bad
function foo() {
  const self = this;
  return function() {
    console.log(self);
  };
}

// bad
function foo() {
  const that = this;
  return function() {
    console.log(that);
  };
}

// good
function foo() {
  return () => {
    console.log(this);
  };
}
```

### (6) 如果你的文件只输出一个类，那你的文件名必须和类名完全保持一致。

```
// file contents
class CheckBox {
  // ...
}
export default CheckBox;

// in some other file
// bad
import CheckBox from './checkBox';

// bad
import CheckBox from './check_box';

// good
import CheckBox from './CheckBox';
```

### (7) 当你导出默认的函数时使用驼峰式命名。你的文件名必须和函数名完全保持一致。

```
function makeStyleGuide() {
}

export default makeStyleGuide;
```


## 1. 使用ECMAScript 2015规范声明变量

```
//不允许
var a = 10;

//推荐
let a =10；
const b=10; 
```

## 2. console输出
 **使用 window.console**,现阶段前端代码主要运行在浏览器中，所以统一使用window.console,否则静态检查不会通过。
 
```
//bad
console.log('error');

//good
window.error('list is null');
```
 
## 3. 字符串处理
对于拼接的字符串使用ECMAScript 2015 的 **模版字符串**。

对于带参数的拼接也必须使用 **模版字符串**。

```
//bad
'This is '+ user.name + ', Welcome!';

//good
`This is ${user.name} ,Welcome`

```

## 4. 箭头函数
对于回调函数推荐用箭头函数。
使用箭头函数可以使你的代码更加简洁。

```
//bad
const f = function () { return 5 };

//good
const f = () => 5;

//bad
[1,2,3].map(function (x) {
  return x * x;
});

// good
[1,2,3].map(x => x * x);

```

## 5. 图片引用
使用import引入图片，避免使用require

```
//bad
<img src={reuqire('logo')} />

//good
import log from 'logo';

<img src={logo} />

```

## 6. 开发环境和产品代码控制
使用process.env.NODE_ENV来区分

```
//good
if(process.env.NODE_ENV === 'production'){
	return 'is production';
}else{
	return 'is test.';
}

```

## 7. router使用常量
对于router中的path，Link中的to等，只有涉及url path的地方统一使用常量，在解决非根部署时会很方便。

```
//bad
<Route exact path={'main'} component={IndexPage} />

<Link to={'index'}/>

routerRedux.push('main')

//good
export const ROUTE_PATH = {
  WEB_HOME: ROUTER_PATH,
};

<Route exact path={ROUTE_PATH.WEB_HOME}} component={IndexPage} />

<Link to={ROUTE_PATH.WEB_HOME}}/>

routerRedux.push(ROUTE_PATH.WEB_HOME})

```

## 8. 5行代码原则
5行代码原则介绍，更准确说是5句。非强制要求，组内要求function 尽量不要超过10句，最长要求不能超过对于复杂业务尽量保持在一屏幕范围内可观看，总体不超过***30行***。

通过重构，提取function等使每个function功能单一，内容简介，增加可阅读性。

## 9. 保持render整洁
在组件中render不要写多行的计算和逻辑代码，对于有计算要求的或者有业务逻辑处理的，提取为function，render尽量保持只有JSX。

***对于render中超过5行的业务逻辑代码或者计算必须要重构提取为function***

```
//bad
render() {
    const {intl, dispatch,quit} = this.props.state;

    const columns = [
      {
        title: intl.formatMessage({id:'onJob.name'}),
        dataIndex: 'name',
        key: 'name',
      },{
        title: intl.formatMessage({id:'onJob.position'}),
        dataIndex: 'position',
        key: 'position',
      }, 
      }];
      
    const rowSelection = {
      onChange: (selectedRowKeys, selectedRows) => {
        if(selectedRows.length !== 0){
          let userId = ''
          selectedRows.map((e)=> {
            userId = userId+e.id+';'
          })
          dispatch({
            type:'quit/setState',
            payload:{
              userIds:userId
            }
          })
        }else {
          dispatch({
            type:'quit/setState',
            payload:{
              userIds:''
            }
          })
        }

      },
      getCheckboxProps: record => ({
        disabled: record.name === 'Disabled User', // Column configuration not to be checked
        name: record.name,
      }),
    };
    return (...)
   }
   
   
    
    //good
     const columns =()=> [
      {
        title: intl.formatMessage({id:'onJob.name'}),
        dataIndex: 'name',
        key: 'name',
      },{
        title: intl.formatMessage({id:'onJob.position'}),
        dataIndex: 'position',
        key: 'position',
      }, 
      }];
      
      const rowSelection = {
      onChange: (selectedRowKeys, selectedRows) => {
        if(selectedRows.length !== 0){
          let userId = ''
          selectedRows.map((e)=> {
            userId = userId+e.id+';'
          })
          dispatch({
            type:'quit/setState',
            payload:{
              userIds:userId
            }
          })
        }else {
          dispatch({
            type:'quit/setState',
            payload:{
              userIds:''
            }
          })
        }

      },
      getCheckboxProps: record => ({
        disabled: record.name === 'Disabled User', // Column configuration not to be checked
        name: record.name,
      }),
    };


    render() {
    const {intl, dispatch,quit} = this.props.state;
        return (...columns, rowSelection)
   }
```


## 10.样式的使用
统一使用styles

```
//bad
 import bs from ‘example.css/less’; 

//good
 import styles from ‘example.css/less’;
```
对于多个单词使用下划线分割，不要使用横线，使用横线在引用时会报错。

```
//bad
<div className={styles['button-red']}/>

//会报错
<div className={styles.button-red}/>

//good
<div className={styles.button_red}/>

```

***每个组件使用独立的样式，禁止整个项目使用只一个。***

## 11. 打包配置
以下参数为必选

```
 devtool: 'source-map',
 hash: true,
```

## 12. 代码静态检查
Eslint为项目必备的静态检查工具，Stylelint为样式检查工具。

***Eslint推荐使用fbjs和eslint-config-airbnb。***
***Stylelint使用默认配置***

## 13. 格式化代码
prettier为项目必备格式化工具，现有配置如下：

```
# .prettierrc
singleQuote: true
trailingComma: es5

```

## 14. 包的安全
通过 ***npm audit*** 来检查包依赖的安全。

漏洞级别以及修复要求。

dependencies 里面的需要遵守下面原则。
devDependencies 可以暂时忽略。

|级别            | 修复要求
|---------------|----------------
|Low            | 可以不修复。
|Moderate       | 可以不修复。
|High           | 必须修复。
|Critical       | 必须修复。
