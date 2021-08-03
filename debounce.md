##防抖函数的使用 
1. 在定义方法的js文件中导出防抖函数
```
 export function debounce(fn, wait) {
    let timer, timeStamp = 0;
    let context, args;

    //定义定时器
    let run = () => {
        timer = setTimeout(() => {
            fn.apply(context, args);
        }, wait);
    }
    let clean = () => {
        clearTimeout(timer);
    }

    return function() {
        //接收fn函数的this，和实参
        context = this;
        args = arguments;
        let now = (new Date()).getTime();

        //判断经过时间是否超过设定的时间
        if (now - timeStamp < wait) {
            clean(); //清除上一个定时器
            run(); // 从当前时间开启新定时器
        } else {
            run(); // 这里返回第一次和最后一个开启的定时器，最后一个定时器被执行
        }
        timeStamp = now;
    }
} 
``` 
2. 在需要用使到的组件中导入防抖函数
```
import {  debounce } from "@/methods";
``` 
3. 使用
```
    //事件名：debounce(事件执行函数,防抖时间)
    //事件执行函数不能写箭头函数，不然会拿不到this
    handleChange: debounce(function (e) {
      seqSwipe(e.id, { seq: e.seq }).then((res) => {
        this.$message({
          showClose: true,
          message: "排序修改成功",
          type: "success",
        });
        this.getList();
      });
    }, 500),
```