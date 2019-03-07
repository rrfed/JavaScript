# 干货
## 如何让Object也可以像Array一样可以使用for...of 进行遍历？
- Array、String、Set、Map数据类型早就在prototype原型链上早就挂载了Symbol.iterator 方法，如果Object也要能够通过for of 遍历，只能给Object的原型链上追加Symbol.iterator 方法 做法如下
```
Object.prototype[Symbol.iterator] = function() {
    let _this = this;
    let index = 0;    
    let keys = Object.keys(_this);
    let length = Object.keys(_this).length;

    return {
        next:() => {            
            let value = {};
			      value[keys[index]] = _this[keys[index]]; //这里将其对象内的每个键值输出了
            let done = (index >= length)
            index++
            return {value,done}
        }
    }
}

想要的效果，我有一个这样的对象:
let obj = { name:"stepday",sex:"男",work:"仁润"};
想通过for of 遍历 直接提取到每一组键值进行输出
for(let key of obj){
   console.log(key);
}

//输出如下
{name:"stepday"}
{sex:"男"}
{work:"仁润"}

通过加以改造可以达到和for in 一样的效果输出键
```
