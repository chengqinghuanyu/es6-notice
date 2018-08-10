>generator
和普通函数没什么两样，
只是在函数的括号和function中间有一个*
function* md(x,y){
    函数体内用yield表达式
}

Generator函数返回一个遍历器对象
代表generatro的内部指针以后每次调用next方法就会返回一个value和done的对象
value是对象值也就是yield的值，dene是一遍历器是否执行完毕的布尔值，

>yield表达式
generator中的yield表达式在当调用 next方法时才会执行
yield和return的区别和相同点
都能返回一个表达式的值
区别是yield会暂停,下一次调用时紧接着继续向下执行好像是暂停了函数一样，yield可以执行多次，且生效，yield只能用于generator函数里面其他地方会报错，如果yield表达式放在另一个表达式中需要放在括号里面，yield用作参数或者等号右边可以不加括号
return不会暂停，但是return一次，后面的不会在生效


>generator与Iterator的关系
任何对象的Symbol.iterator函数会返回一个遍历对象，Generator就是遍历器生成器函数，可以吧Generator赋值给对象的Symbol.iterator属性，使得该对象就有iterator接口

        var myiterator = {

        };
        myiterator[Symbol.iterator] = function* mg(){
            yield 1;
            yield* [2,2,3,'big'];
            yield '好媳妇'

        };
        [...myiterator]

        function* gm(){};
        var g = gm();
        g[Symbol.iterator]()===g()


>next方法的参数
next参数会被当成上一个yield的返回值
        function* f(){
            for(var i=0;true;i++){
                var mh = yield i;
                if(mh){
                    i = -1;
                }
            }
        }

        var m = f();
        m.next();
        m.next();
        m.next();
        m.next(true);
        next启动的第一个调用不必带参数
        从第二个带的参数开始生效
                            function* foo(x){
                                var y = 2 * (yield ( x+1 ));
                                var z= yield (y/3);
                                return (x+y+z);
                            }
                            var a = foo(5);
        a.next();
        a.next();
        a.next();
        var b = foo(5);
        b.next();
        b.next(6);
        b.next(7);
        b.next(8);


            function* getNm(){
                console.log('string');
                console.log(`1.${yield}`);
                console.log(`2.${yield}`);
                return 'result...';
            }
            let     gojb = getNm();

>for of循环可以自动遍历generator函数时生成的iterator对象，此时不在需要调用next方法

function* ml(){
    yield 1;
    yield 3;
    yield 4;
    yield 5;
    yield 6;
    return 6;
}

for(let v of ml()){
    console.log(v);
}
//斐波那契数列
        function* bgnq(){
            let [pre,cur]=[0,1];
            for(;;){
                yield cur;
                [pre,cur] = [cur,cur+pre];
            }
        }
        for(let n of bgnq()){
                if(n>1000){
                    break;
                }
                console.log(n);
        }

        genrerator为原生对象遍历
        function* org(obj){
            let poks = Reflect.ownKeys(obj);
            for(let pro of poks){
                yield [pro,obj[pro]]
            }
        }

        let jen = {
            first:'Joen',
            last:'Does'
        }
        for(let [keys,bre] of org(jen)){
            console.log(`${keys}--${bre}`);
        }


>Generator.prototype.throw()
>Generator.prototype.next()
>Generator.prototype.return()

throw()是将yield表达式替换成一个throw语句;
next()是将yield表达式替换成一个值
return()是将yield表达式替换成一个return语句

>yield表达式
在gererator中无法调用另一个generator这时候需要yield 表达式yield*
任何一个数据只要有遍历器就可被yield访问：
任何数据结构只要有 Iterator 接口，就可以被yield*遍历

>对象的generator属性可以写成 mgk={*mg(){}}

>generator函数的this
this不能正常指向定义的对象
不能使用new来定义generator遍历器函数

要想使用generator函数中的this
需要做以下几步：
1）定义空对象，
在当前对象定义后使用当前对象的call方法来讲空对象作为一个参数传递通过原型链方式连访问
2）对呀不能正确使用new的问题可以call(generator.prototype)属性

比如：
    function* Mygener(){
        this.a =1;
        yield this.b = 2;
        yield this.c = 3;

    }
    function F(){
        return Mygener.call(Mygener.prototype);
    }

    var pdg = new F();
    pdg.a;
    pdg.b;
    pdg.c;
>generator的用途
异步操作的同步表达
>流程控制管理
>generator可以在任意对象上部署iterator接口
>作为数据结构->作为数组的数据结构
