>async
1）为了方便书写方便阅读，
2）有自己的内置自执行函数
3）有更好的实用性
4）返回的值是Promise
async function getss(name){
    const symbol = await getStrok(name);
    const stockPic = await getStockP(symbol);
    return stockPic;
}
getss('good').then(function(result){
    console.log(resuilt);
});



async function timeout(ms){
    await new Promise((resolve)=>{
        setTimeout(resolve,ms);
    })
}

async function asyncPrint(value,ms){
    await timeout(ms);
    console.log(value);
}
asyncPrint('hello ypx',5000)

async 多种形式
async function foo(){

}
const foo = async function(){

}
let obj {
    async foo(){

    }
}

obj.foo().then(...)
class Storage {
    constructor(){

    }
    async getAbs(){

    }
}
const stroage  = new Storage();
stroage.getAbs().then()

箭头函数
const  foo = async()=>{

}

>async语法简单难点在于处理错误的机制
>async 返回一个promise对象
async函数内部return语句的返回值会成为then方方达函数参数
async function bg(){
    return 'heloo world';
}

bg().then(v=>console.log(v));


async function error(){
    throw new Error('错误');
}
error().then(v=>console.log(v),e=>console.log(e));

>promise状态的变化
async function getTitle(url) {
  let response = await fetch(url);
  let html = await response.text();
  return html.match(/<title>([\s\S]+)<\/title>/i)[1];
}
getTitle('https://tc39.github.io/ecma262/').then(console.log);
>await命令
正常情况下await命令     后面是一个promise对象如果不是会被转换成一个立即resolve的promise对象
async function pv(){
    return await 123;
}
pv().then(v=>console.log(v));


async function dlp(){
    return Promise.reject('错误');
}
dlp().then(v=>console.log(v)).catch(e=>console.log(e))
>只要一个await语句后面的 Promise 变为reject，那么整个async函数都会中断执行。
有时，我们希望即使前一个异步操作失败，也不要中断后面的异步操作。这时可以将第一个await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行。

async function mmp(){
    await Promise.reject('error');
    await '123'
};
mmp().then(v=>console.log(v)).catch(e=>console.log(e));
>改造上面的函数1

async function mmpa(){
    try{
        await Promise.reject('error');
    
    }catch(e){

    }
   return await '123'
}

mmpa().then(v=>console.log(v)).catch(e=>console.log(e));
>改造上面的函数2
async function mmpb(){
    await Promise.reject('我错误了').catch(e=>console.log(e));

    return await '马云是孙'
}

mmpb().then(v=>console.log(v)).catch(e=>console.log(e))

>错误处理
如果await后面的异步是错误那么等同于是给Promise.reject

如果防止出错可以放置在try...catch中
async function ooc(){
    try{
        await new Promise(function(resolve,reject){
            throw new Error('粗');
        })
    }catch(e){}
    return await('hello world!');

}

async function konwif(){

    try{
        const vals1 = await first();
        const vals2 = await second(1);
        const vals3 = await thrids(4);
        console.log('new bug');
    }catch(e){
        console.log(e)
    }
}


const superagent   =  require('superagent');
const NUM_TIMES = 3;
async function loadssss(){
    let i =0;
    for(i;i<NUM_TIMES;i++){
        try{
            await superagent.get('http://google.com/this-throws-an-error');
            break;
        }catch(e){
            console.log(e);
        }
    }
    console.log(i)
}
loadssss();

>注意点
最好将await命令放置在try...catch中
因为运行结果可能是promise的reject
如果多个await异步操作不存在关系就最好让他们同时触发
await命令只能写在async函数中


>async 函数的实现原理

将Generator函数和自执行器包裹在一个函数里
；类似于：
function fn(args) {
  return spawn(function* () {
    // ...
  });
}

function spawn(fn){
    return new Promise(function(resolve,reject){
        const gen = fn();
        function step(nextFn){
            let next ;
            try{
                next = nextFn()
            }catch(e){
                return reject(e);
            }
            if(next.done){
                return resolve(next.value);
            }
            Promise.resolve(next.value).then(function(v){
                step(function(){
                    return gen();
                })
            },function(e){
                step(function(){
                    return gen().throw(e);
                })
            })
        }
        step(function(){
            return gen.next(undefined);
        })
    })
};

>async与其他的异步处理函数的比较
function chainAnimtionPromise(ele,animations){
    let ret = null;
    let p=Promise.resolve();
    for(let anim of animations ){
        p = p.then(function(val){
            ret = val;
            return anim(elem);
        })
    }
    return p.catch(function(e){

    }).then(function(){
        return ret;
    })
}

>Generator
function chainAnimationGeneration(ele,animtion){
    return spawn(function* (){
        let ret = null;
        try{
            for(let anim of animatio){
                ret = yield anim(ele);
            }
        }catch(e){

        }
        return ret;
    })
}


async function chainAnimtionAsync(ele,animation){
    let ret = null;
    try{
        for(let anim of animation){
            ret = await anim(ele);
        }
    }
    return ret;
}


>实例

1）顺序完成异步操作
async function logInOrder(urls){
    const textPromises = urls.map(async url=>{
        const   respromse = await fetch(url);
            return respromse.text();
    })

    for( const textprose of textPromises){
        console.log(await textprose);
    }
}


>异步遍历器
asyncIterator
.next()
.then(({value,done})=>
console.log(value)
);

const asyncIterable = createAsyncIterable(['a','b']);
const asyncIterator = asyncIterable[Symbol.iterator]();
asyncIterator.next()
.then(iter=>{
    console.log(iter);
    return asyncIterator.next();
})
.then(iter2=>{
    console.log(iter2);
    return asyncIterator.next();
})
.then(iter3=>{
    console.log(iter3);
})




> async
async function f(){
    const asyncIterable = createAsyncIterable(['a','b']);
    const asyncIterator = asyncIterable[Symbol.iterator]();
    console.log(await asyncIterator.next());
    console.log(await asyncIterator.next());
    console.log(await asyncIterator.next());
}

>for await of用于遍历异步Iterator接口
async function ff(){
    for await (const x of createAsyncIterable(['a','b'])){
        console.log(x);
    }
}


let body ="";
async function fff(){
    for await (const x  of data ){
        body+=x;
    }
    const parsed = JSON.parse(body);
    console.log('got', parsed)
}
>Generator异步函数
异步遍历器的设计目的之一，就是 Generator 函数处理同步操作和异步操作时，能够使用同一套接口。
async function* gen(){
    yield 'hello';

}
const gentObj= gen();
gentObj.next().then(x=>console.log(x))


异步 Generator 函数内部，能够同时使用await和yield命令。可以这样理解，await命令用于将外部操作产生的值输入函数内部，yield命令用于将函数内部的值输出。


(async function(){

    for await ( const line of readLines(filepath)){
        console.log(line);
    }
})


async function * prefLines(pod){
    for await (const x of pod){
        yield '>' + line;
    }
}

prefLines({yes:'111'}).next()


yield*语句
async function * gen1(){
    yield 'a';
    yield 'b';
    yield 'c';
}
async  function* gen2(){
    const result   = yiled* gen1();
}


(async function () {
  for await (const x of gen2()) {
    console.log(x);
  }
})();