---
title: Selector组件开发
date: 2018-08-27 15:13:18
tags: [React, 组件]
categories: [React]
---

# React基础组件Selector 开发

磕磕绊绊写了个Selector组件...

和原生JS开发不同的，React开发UI天生就自带组件，JSX的写法刚开始有点不习惯，后面就好了，写起来还是很舒服的

简单的列下配置

`React + Webpack + Sass `

webpack用的是4.x版本，用起来坑还是蛮多的，比如说不指定mode为	`development`会有warning，

目前的目录结构为：

![1535354919979](D:\MyRepository\personal-notebook\React\Select组件开发.assets\1535354919979.png)

Selected.jsx 

``` jsx
import React from 'react';
import S from './Selected.scss';

export default class Selected extends React.Component {
    constructor(props){
        super(props);
        this.state = {
            selectText: [],  // 选择的内容
            selectValue: [],  // 选择的值
        };
        this.selectHandler = this.selectHandler.bind(this);
    }

    selectHandler(ev){
        let target = ev.currentTarget;
        let { selectText, selectValue}  = this.state;
        let { onChange } = this.props;
        let value = target.getAttribute("data-value"),
            text = target.getAttribute("data-text");

        selectText.indexOf(text) === -1 
            ? selectText.push(text) 
        	: selectText.splice(selectText.indexOf(text), 1);
        selectValue.indexOf(value) === -1 
            ? selectValue.push(value) 
        	: selectValue.splice(selectValue.indexOf(value), 1);

        let obj = selectText.reduce((previousValue, currentValue, currentIndex) => {
            previousValue.push({
                text: currentValue,
                value: selectValue[currentIndex]
            });
            return previousValue;
        }, []);
        this.setState({
            selectText,
            selectValue
        });
        onChange(obj);
    }

    render(){
        let { selectHandler } = this;
        let { selectText, selectValue } = this.state;
        let { list } = this.props;
        let contentList = [];
        let selectorList = [];
        list.map( (elt, i)=>{
            selectorList.push(
                <li
                    className={S["selector-content-li"]}
                    data-value={elt.value}
                    data-text={elt.text}
                    key = {i}
                    onClick={selectHandler}
                >
                    <span className={S.span}>{ elt.text }</span>
                </li>
            )
        });
        selectValue.map((value,index)=>{
            contentList.push(
                <li
                    className={S["selector-content-li"]}
                    data-value={value}
                    data-text={selectText[index]}
                    key={index}
                >{selectText[index]}</li>
            )
        });

        return (
            <div>
                <div
                    className={S["multi-selector"]}
                >
                    <ul className={S["selector-content"]}>
                        { contentList }
                    </ul>
                </div>
                <ul className={S["selector-list"]}>
                    {selectorList}
                </ul>
            </div>
        )
    }
}
```

在index.js中调用组件

``` jsx
import React from 'react';
import ReactDOM from 'react-dom';
import Selected from './Selected/Selected.jsx';

let list = [
    {
        text: "text1",
        value: "value1"
    },
    {
        text: "text2",
        value: "value2"
    },
    {
        text: "text3",
        value: "value3"
    },
    {
        text: "text4",
        value: "value4"
    },
    {
        text: "text5",
        value: "value5"
    }
];

function changeValue(obj){
    console.log(obj);
}

ReactDOM.render(
    <div>
        <Selected
            list = {list}
            onChange = {changeValue}
        />
        <Selected
            list = {list}
            onChange = {changeValue}
        />
    </div>,
    document.getElementById('root')
);
```

## 二、有几个需要注意的地方

### 1. 暴露出去所选择的值

这里是在props中传入一个父级组件的方法onChange，每次值改变时触发onChange好了

## 三、需要继续完善的地方

### 1. 选择项控制

- [ ] 选择项高亮显示
- [ ] 可以自定义禁用选择项
- [ ] 单选/多选模式可以切换
- [ ] 支持最大项控制

### 2. 支持搜索

- [ ] 支持输入搜索项自动检索选择项
- [ ] 支持输入搜索项时高亮所选择内容

### 3. 动态加载

- [ ] 支持动态加载数据
- [ ] 支持大量数据逐步加载

