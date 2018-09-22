---
title: JS 数组乱序
date: 2018-09-22 23:40:21
tags: [JS]
categories: [JS]
---

JS 数组乱序

```javascript
function shuffle(arrPara){
    let arr = arrPara.slice(0);
    let len = arr.length;
    if(len <= 1){
        return arr;
    }
    for( let i = len - 1 ; i > 0 ; i -- ){
        let pos = Math.floor(Math.random() * (i + 1));  // 这里的随机数是左闭右开 [0, 1)
        swap(arr, i , pos);
    }
    return arr;
}

function swap(arr, i, j) {
    [ arr[i], arr[j] ] = [ arr[j], arr[i] ];
}

// 如何测试我的乱序代码

let times = 1000;
let res = {};
let arrDemo = [1,2,3];

for(let j = 0 ; j < times; j ++){
    let temp = JSON.stringify(shuffle(arrDemo));
    res[temp] = res[temp] ? res[temp] + 1 : 1;
}

// for(let key in res){
//     res[key] = res[key] / times;
// }

console.log(res);
```

