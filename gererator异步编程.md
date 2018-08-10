Generator函数：
>1协成的步骤
第一步、协成A开始执行
第二步、协成A执行了一半进入暂停，进入协成B
第三步、协成B交还执行权限
第四步、协成A继续执行
Generator函数的精妙之处在于yield命令来隔断同步执行的，让程序进入暂停阶段
>2Generator的协成写法
function* pk(x){
    var y = yield x*2;
    return y; 
}
var mkg = pk(2);
mkg.next()
>3Generator函数的数据交换和错误处理
1）next()参数中对数据的交换
2）Generator函数中错误处理
function* mgs(x){
    try{
        var y = yield x*2;

    }catch(e){
        console.log(e)
    }
    return y;
}

var uio = mgs(1);
uio.next();
uio.throw('错错了');//uio.throw()实现了错误的异步处理这很有用
>4异步任务封装

>5Thunk 函数是自动执行 Generator 函数的一种方法。
function run(fn){
    var gen = fn();
    function next(err,data){
        var result = gen.next(data);
        if(result.done){
            return 
        }
        result.value(next);
    }
    next();
}

function* gun(){
    var f1 = yield readFileThunk('fileA');
    var f2 = yield readFileThunk('fileB');
    // ...
    var fn = yield readFileThunk('fileN');
};
run(gun)

>6co模块
前面说过，Generator 就是一个异步操作的容器。它的自动执行需要一种机制，当异步操作有了结果，能够自动交回执行权。
两种方法可以做到：
（1）回调函数。将异步操作包装成 Thunk 函数，在回调函数里面交回执行权。
（2）Promise 对象。将异步操作包装成 Promise 对象，用then方法交回执行权。
