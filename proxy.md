proxy
proxy可以理解为是一种对象的拦截
var proxys = new Proxy({},{
    get:function(t,p){
        return 33
    }
});
proxys.timer;
proxys.name;
proxys.age;

proxy接收两个参数一个参数是当前需要被拦截的对象的实例，另一个是设置拦截的对象配置属性
var target = {a:'10'};
var handle= {}
var proxy = new Proxy(target,handle);
proxy.a='b';
target.a;
//13种拦截操作

get,对拦截对象的属性的读取
set是对拦截对象的属性的设置
has
deleteProperty
ownKeys
getOwnpropertyDescriptor
defineProperty
preventExtensions
getPrototypeOf
isExtensible
setPrototypeOf
apply
constructor

get(参数一、参数二、参数三);
参数一、是当前对象；
参数二、目标属性
参数三、可选参数
链式操作
var pipe = (function(){
    return function(value){
        var funcStack = [];
        var oproxy =   new Proxy({},{
            get:function(pipeObj,fnName){
                if(fnName==='get'){
                    return funcStack.reduce(function(val,fn){
                        return fn(val)
                    },value)
                }
                funcStack.push(window[fnName]);
                return oproxy
            }
        })

        return oproxy;
    }
}());
var double = n=>n*2;
var pow = n=>n*n;
var reverseInt = n=>n.toString().split("").reverse().join("")|0;
pipe(3).double.pow.get
//get的第三个参数是proxy对象的实例

set有四个参数
第一个参数：目标对象
第二个参数：目标名称
第三个对象：目标值
第四个参数：返回的对象实例
const handles = {
    get(t,k){
        invariant(k,'get');
        return t[k]
    },
    set(t,k,v){
        invariant(k,'set');
        t[k]=v;
        return true
    }
}

function invariant(key,ac){
    if(key[0]==="_"){
        throw new Error('报错了')
    }
}
const targets = {};

const prox = new Proxy(targets,handles);
prox._prop


apply()拦截函数的apply,call和方法调用操作
接收三个参数
第一个参数：目标对象
第二个参数：目标对象的上下文this
第三个参数:目标对象的参数数组
var myTag = function(){
    return 'aaa bbb ccc';
}
var myHandle = {
    apply(){
        return 'bbccdd';
    }

}
var myp = new Proxy(myTag,myHandle);
myp()

>has
用来拦截hasProperty操作，判断对象是否具有某个属性时这个方法会生效，典型的就是in操作符
var handle = {
    has(target,key){
        if(key[0] === "_"){
            return false;
        }
        return key in target;
    }
}

var target = {
    _prop:'foo',
    prop:'bar'
}
var proxy = new Proxy(target,handle);
'_prop' in proxy;

has拦截的是hasProperty而不是hasOwnproperty。has只对in操作符生效，不对for of生效。

construct用于拦截new命令
参数1：目标对象
参数2：构造函数的参数对象
参数3：创建构造实例时new命令作用的构造函数

var ps = new Proxy(function(){},{
    construct(target,args):function{
        console.log('called: '+args.join(', '));
        return {value:agrs[0]*10}
    }
})

(new ps(10)).value;//报错了不知道为啥？？
deleteProperty:用于拦截删除操作
var handDelete = {
    deleteProperty(target,key){
        invariants(key,'delete');
    }
}
function invariants(m,n){
    if(m[0]==="_"){
        throw new Error('不能删除');
    }
}

var targeDele = {
    _prop:'pop'
};
var proxyDele = new Proxy(targeDele,handDelete);
delete proxyDele._prop
注意，目标对象自身的不可配置（configurable）的属性，不能被deleteProperty方法删除，否则报错。

defineProperty用于拦截对象的defineProperty方法
如果对象是不可拓展extensible则不可以增加目标对象上不存在的属性，不可以修改writeable和configurable属性


getOwnPropertyDescriptor用于拦截Object.getOwnPropertyDescriptor操作同上

getPrototypeOf主要拦截获取对象原型
>Object.prototype.__proto__
>Object.prototype.isPrototypeOf()
>Object.getPrototypeOf()
>Reflect.getPrototypeOf()
>instanceof


ownKeys方法用来拦截对象自身属性的读取操作
>Object.getOwnPropertyNames()
>Object.getOwnPropertySymbols()
>Object.keys()
>for...in循环

preventExtensions方法拦截Object.preventExtensions()。该方法必须返回一个布尔值，否则会被自动转为布尔值。


setPrototypeOf方法主要用来拦截Object.setPrototypeOf方法。


2、拦截器的revocable方法返回一个可取消的 Proxy 实例

let targetRe = {};
let handleRe = {    };
let { proxyRe,revoke} = new Proxy.revocable(targetRe,handleRe);
proxyRe.foo = "500";
revoke();
proxyRe.foo

使用场景：目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问。
3、this问题，proxy代理情况下，目标对象内部的this关键字会指向 Proxy 代理
4、proxy的应用场景：Proxy 对象可以拦截目标对象的任意属性，这使得它很合适用来写
Web服务的客户端。





