#Map数据结构

>出现的原因和定义：：：：：
ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。
##>>定义

let ak = new Map([['name':'张三'],['title':'love']]);
map.size;
map.has('name');
map.get('name');
map.set('name','沥青');

map可以用来处理库的同名碰撞问题，不用担心两个属性会同名

map的属性和操作方法

>size属性
返回map的总结构数
>set用来设置键值对应的键名，如果键名存在就更新键值，如果不存在就新增
set可以采用链式写法、get用于读取键值如果找不到就返回undefined
has用于返回一个布尔值表示某个键是否存在当前map中
delete用于删除某个键返回true就是删除成功，false就是删除失败
clear会清除所有成员没有返回
遍历方法
keys()方法是返回键名、
values()返回键值遍历
entries()返回所有成员的遍历器
forEach()遍历map的所有成员，map遍历的顺序就是插入的顺序

map结构转换成数组使用展运算符
cost map = new Map([

    [1,'one'],
    [2,'two'],
    [3,'tree']
]);
[...map.keys()];
[...map.values()];
[...map.entries()];
[...map]

forEach方法和数组的forEach方法相同不返回

map转换成数组就是使用拓展运算符

数组转化成map是将数组传入map的构造函数中，
map转为对象>
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)

对象转map使用set方法
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})

JSON转成map
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')

function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

map转成JSON

function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);


function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}
let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);

>WeakMap:
WeakMap只接受对象作为键名（null除外）

其次，WeakMap的键名所指向的对象，不计入垃圾回收机制。

主要功能是防止内存泄漏
WeakMap只有四个方法可用：has,get,delete,set




