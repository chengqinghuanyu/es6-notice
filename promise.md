promise是异步编程的一种方案。
1）不受外界影响
2）状态一点改变就不会在改变

resolve--fulfilled(已经成功)、
rejected(已经失败)

const promise = new Promise(function(resolve,reject){
    if(resolve){
        resove(value)
    }
    else{
        reject(error)
    }
});
promise.then(function(value){
    console.log(value);
},function(error){
    console.log(error);
});

function timeout(ms){
    return new Promise(function(reslove,reject){
            setTimeout(reslove,ms,'done')
    })
}
timeout(1000).then((value)=>{
    console.log(value)
});

promise异步加载图片
function loadImageAsync(url){
    return new Promise(function(resolve,reject){
        const img =new Image();
        img.onload=function(){
            resolve()
        };
        img.onerror = function(){
            reject(new Error('图片加载错误'))
        };
        img.src=url;
    })
};

promise实现的异步加载的ajax
const   getJSON = function(url){
    const promise = new Promise(function(resolve,reject){
        const   handler = function(){
            if(this.readyState!==4){
                return 
            }
            if(this.state==200){
                resolve(this.response);
            }else{
                reject(new Error(this.statusText));
            }
        };
        const  client = new XMLHttpRequest();
        client.open("GET",url);
        client.onreadystatechange= handler;
        client.responseType="JSON";
        client.setRequestHeader("Accept","application/json");
        client.send();
    })
    return promise
};
getJSON('/post.json').then(function(json){
    console.log('contents:'+json);
},function(error){
    console.log('出错了:'+error);
});
promise.prototype.then();
用于解决异步回调的链式调用问题，上一个执行结束后再调用后一个直到最后一个。

promise.prototype.catch()方法是promise.then()中第二个参数的优化，then方法执行中的错误，也更接近同步的写法（try/catch）。可以捕获promise内部的错误，还可以调用then方法，如果没有错误就会跳过catch

promise.prototype.finally()方法用于指定不管promise对象的最后状态如何，都会执行的操作。

promise.then(result=>{
    console.log(reslut)
}).cath(error=>{

}).finally()


promise.all()里面的参数是一个数组或者是interator的接口，且返回的成员都有promise实例
只有所有 的参数都返回了resolve的all的实例才会返回resolve数组包含了参数的resolve组合,只要有一个返回了reject，all实例就会返回第一个的reject

const promise = [1,2,3,4,5,6,7].map(function(id){
    return getJSON('/post/'+id+".json");
})
Promise.all(promise).then(function(post){

}).catch(function(response){

})


promise.rece()::Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例

const p = Promise.rece([p1,p2,p3]);
p1,p2,p3只要有一个状态率先改变就将那个状态返回给p


promise.resolve()

const jsPromise = Promise.resolve($.ajajx('./whatever.json'));
Promise.resolve共有四种情况
1、参数是promise实例，promise.resolve()不做任何修改，原封不动返回这个实例
2、参数是thenable对象指的是有then方法的对象，promise.resolve()会将这个对象转换成Promise对象，然后立即执行thenable对象的then方法
let thenable = {
    then:function(resolve,reject){
        resolve(1000);
    }
};
let p1 = Promise.resolve(thenable);
p1.then(function(value){
    console.log(value)
})
3、如果参数不是对象没有then方法则promise.resolve()返回一个新的Promise对象状态为resolved

const hello = Promise.resolve('leoo');
hello.then(function(s){
    console.log(s)
})
4、promise.resolve()不带任何参数直接返回一个resolved状态的Promise对象，需要注意的是，立即resolve的 Promise 对象，是在本轮“事件循环”（event loop）的结束时，而不是在下一轮“事件循环”的开始时

setTimeout(function(){
    console.log('three')
},0);

Promise.resolve().then(function(){
    console.log('two');
});

console.log('one');

promise.reject()方法也会返回一个新的 Promise 实例.Promise.reject()方法的参数,会原封不动地作为reject的理由，变成后续方法的参数。这一点与Promise.resolve方法不一致。

应用

promise图片加载

function loadingImg(url){
    return new Promise(function(resolve,reject){
        const img = new Image();
        img.onload = resolve;
        img.onerror = reject;
        img.src= url;
    })
}
Generator 函数与 Promise 的结合


promise.try：想知道某个函数是异步还是同步
const f = () => console.log('now');
Promise.resolve().then(f);
console.log('next');





