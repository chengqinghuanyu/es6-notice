interator适用于处理不同的数据的统一接口。Object,Array,Set,Map。
任何数据只要部署遍历器就会完成遍历操作。
任何数据结构只要部署 Iterator 接口，就可以完成遍历操作。
1）一是为各种数据结构，提供一个统一的、简便的访问接口；
2）二是使得数据结构的成员能够按某种次序排列
3）三是 ES6 创造了一种新的遍历命令for...of循环，Iterator 接口主要供for...of消费

iterator接口的遍历过程是这样的：
创建一个指针对象指向当前数据的起始位置，遍历器的本质就是一个指针对象。
第一次调用指针对象的next方法可以将指针指向数据结构的第一个成员。
第二次调用指针对象的next方法指针就指向数据结构的第二个成员。
不断的调用next方法直到它指向数据结构的结束位置。


每次遍历都会返回一个当前的value和done，value是当前对象的值，done是一个布尔值表示遍历是否结束。

const obj={
    [Symbol.iterator]:function(){
        return {
            next:function(){
                return {
                    value:1,
                    done:true
                }
            }
        }
    }
}

原生具备iterator属性的数据结构如下：
Array,
Map,
Set,
String,
arguments,
NodeList
TypedArray

>数组的interator
let arr = ['a','b','c'];
let inter = arr[Symbol.iterator]();
inter.next();
inter.next();
inter.next();

>对象的iterator需要自己在Symbol.iterator属性上部署才能被for...of循环遍历。
对唱没有遍历器是因为对象是无序列表。iterator是一种线性的顺序
下面是通过遍历器实现指针结构的例子。
function Obj(value){
    this.value = value;
    this.next= null;
}
Obj.prototype[Symbol.iterator] = function(){
    var iterator = {
        next:next
    };
    var current = this;
    function next(){
        if(current){
            var value = current.value;
            current = current.next;
            return {
                done:false,
                value:value
            }
        }else{
            return {
                done:true
            }
        }
    }
    return iterator;
}

var one = new Obj(1);
var two = new Obj(2);
var three = new Obj(3);

one.next = two;
two.next = three;
for (var i of one){
    console.log(i);
}

let myobjs = {
    data:['hello','world'],
    [Symbol.iterator](){
        const self = this;
        const index = 0;
        return {
            next(){
                if(index<self.data.length){
                    return {
                        value:self.data[index++],
                        done:false
                    }
                }else{
                   return {
                       value:undefined,
                       done:true
                   }
                }
            }
        };
    }
}


let iteratorble = {
    0:'1',
    1:'1',
    2:'2',
    3:'3',
    4:'4',
    length:5,
    [Symobol.iterator]:Array.prototype[Symbol.iterator]
};
for(let item of iteratorble){
    console.log(item)
}


>调用iterator的场合
1）结构赋值会默认调用Symbol.iterator

let set = new Set().add('1').add('2').add('3');
let [x,y] = set;
let [first,...se] = set;
2)拓运算符

var str = 'hellow';//不加分号会报错
[...str]
3）yield

let generator = function*(){
    yield 1;
    yield* [2,3,4,5];//yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口
    yield 3;
}
var intera = generator();
intera.next();
4)其他场合
for ... of
Array.form()
Map()
Set()
MapWeak();
setWeak();
Promise.all()
Promise.race();

>字符串的iterator

let mgi = 'iiiii';
typeof mgi[Symbol.iterator]//>'function';
let bki = mgi[Symbol.iterator]()

>iterator与generator函数
let generI = {
    * [Symbol.iterator](){
        yield 'hello';
        yield * ['dshello','world','ypx'];
        yield 'bigd';
    }
};
for(var i of generI){
    console.log(i);
}
>遍历器对象的retrun()和throw()
遍历器对象除了具有next方法还有return方法和throw方法，return方法和throw方法是可选的。
return方法一般返回一个对象，用于处理异常情况的退出
function readListSync(file){
    return {
        [Symbol.iterator](){
            return{
                next(){
                    return {
                        done:false
                    }
                },
                return(){
                    file.close();
                    return {
                        done:true
                    }
                }
            }
        }
    }
};
for (var lie of readListSync(filename)){
    console.log(lie)
}




