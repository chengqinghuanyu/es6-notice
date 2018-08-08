Set是es6新增的数据结构，类似数组但是元素是唯一的

set去重
let set = new Set([1,2,3,4,5,6,7,8,8,8,8]);
[...set];
console.log(set)

set的属性和方法

keys();values();entries();forEach();
let mg = new Set(['red','green','blue']);
for(let item of mg.keys()){
    console.log(item);
};
for(let item of mg.values()){
    console.log(item);
};

for(let item of mg.entries()){
    console.log(item);
};
//
Set 结构的键名就是键值（两者是同一个值），因此第一个参数与第二个参数的值永远都是一样的。forEach方法还可以有第二个参数，表示绑定处理函数内部的this对象
mg.forEach((m,i)=>m*2)

//可以直接使用for of进行遍历

for(imt of mg){
    console.log(imt)
}
>add方法||添加一个值
>delete方法||删除某个值
>has方法||返回一个布尔值
>clear方法||清除所有成员
===============
还有一个（size）属性
2、weakset，weakset的值是对象；其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
#WeakSet
=============================
使用了值是对象，其次是是弱引用
>他只能定义时对象的参数，否则会报错。
>如果是多维数组会被磨成对象
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}

================
########const b = [3, 4];
########const ws = new WeakSet(b);
=====================上面的是error

>weakset有三个方法
add
has
delete
>weakset没有size属性
