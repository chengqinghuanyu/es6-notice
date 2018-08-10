1、reflect出现的原因
1）是为了规范Object中方法和属性
将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上
2）修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false
3）让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
4）Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。
总结：完善Object{
    1、让对象变得合理
    2、让对象便于管理
    3、让对象操作更规范
},关联Proxy。

reflect一共13个静态方法

Reflect.apply()
Reflect.construt()
Reflect.get()
Reflect.set()
Reflect.defineProperty()
Reflect.deleteProperty()
Reflect.has()
Reflect.ownKey()
Reflect.isExtensible()
Reflect.preventExtensions()
Reflect.getOwnProperty()
Reflect.getPrototypeOf()
Reflect.setPrototypeOf()

>Reflect.get()
有三个参数
target:
name
receiver

如果name属性部署了读取函数（getter），则读取函数的this绑定receiver。

var myObj= {
    foo:1,
    bar:2,
    get zar(){
        return this.foo+this.bar
    }
}

var myReflectObj = {
    foo:4,
    bar:6
}

Reflect.get(myObj,'zar',myReflectObj);

>set属性设置target的name属性为value,如果name属性设置了赋值函数this绑定recevier
var myobj = {
    foo:1,
    set bar(value){
        return this.foo=value
    }
}
myobj.foo
Reflect.set(myobj,'foo',2);
myobj.foo
Reflect.set(myobj,'bar',4);
myobj.foo//不能访问bar函数只能通过间接设置bar函数然后获取foo的值

可以使用Proxy的拦截然后在拦截中对实例进行设置，完成自己的需求
>Reflect.has()
第一个参数是Obj;
第二个参数是name;
has对应了in运算符
var reHas = {
    book:true
}
Reflect.has(resHas,'name');
如果第一个参数不是对象就会报错
>Reflect.deleteProperty();等同于delete obj[name];删除属性

返回一个布尔值
成功就true,否则就false
const myBook = {
    name:'前端教程',
    year:'2018。10.15',
    au:'尹鹏孝'
}

Reflect.deleteProperty(myBook,'name');
myBook;
>Reflect.construct()等同于new target(...args);但是reflect。construct没有使用new来创建构造函数
function mynames(name,age,adds){
    this.name= name;
    this.age=age;
    this.address=adds
}
var args = ['李白',30,'西安']
const innerMan = Reflect.construct(mynames,args);

>Reflect.getPrototypeOf()
一个参数obj
用于读取对象的原型链

>Reflect.getPrototypeOf()和Object.getPrototypeOf()的区别是
如果参数是对象则没事没如果参数不是对象Object会转成对象，而Reflect会报错

>Reflect.setPrototypeOf()用于设置目标对象的原型法用于设置目标对象的原型（prototype）返回一个布尔值来确认是否设置成功
和Object.setPrototypeOf()的区别在于第一个参数是否是对象如果是对象，没关系。如果不是Object.setPrototypeOf()会返回对象本身，而Reflect.setPrototypeOf()会报错
第二个参数是要设置的对象属性
>Reflect.apply()用于替换Function.prototype.apply.call(fn,obj,args);用于this对象执行给定函数
var args = [18,33,45,99,120]
Reflect.apply(Math.min,Math,args);
>Reflect.defineProperty()用于设定对象的属性
Reflect.defineProperty(target, propertyKey, attributes) ；
这个方法和Proxy.defineProperty()一起使用用于拦截和设定属性

const pdf = new Proxy({},{
    defineProperty(target,props,descript){
        console.log(descript);
        return Reflect.defineProperty(target,props,descript)
    }
})
pdf.oo="ooof";
pdf.oo;

>Reflect.getOwnPropertyDescriptor()会得到对象的指定属性描述
与Object.getOwnPropertyDescriptor()的区别还是第一个参数是否是对象
>Reflect.isExtensible()
表示当前对象是否可拓展

var mk = {}
Reflect.isExtensible(my)
>Reflect.preventExtensions()让一个对象变为不可拓展

var mkol = {

}
Reflect.preventExtensions(mkol);
Reflect.isExtensible();

>Reflect.ownKeys()用于返回对象所有属性，等同于Object.getOwnPropretyNames()和Objec.getOwn.propertySymbols之和

>>>>><<<<<<<<<<实例>>>>>>>>>>
使用Reflect写一个观察者模式
const nik = new Set();
const observe =fn=>nik.add(fn);
const observerble = obj=>new Proxy(obj,{set});
function set(target,key,value,receiver){
    const result = Reflect.set(target,key,value,receiver);
    nik.forEach(observe => observe());
    return result;
}











