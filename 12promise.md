# Promise

举个例子：

```
//方式一
function test(resolve, reject) {
    var timeout = Math.random() * 2;
    console.log("开始尝试promise");
    setTimeout(function() {
        if (timeout < 1) {
            resolve(timeout);
        } else {
            reject(timeout);
        }
    }, 1000);
}

function resolve(time) {
    console.log("小于1的数字" + time);
}

function reject(time) {
    console.log("不小于1的数字" + time);
}
var p1 = new Promise(test);
var p2 = p1.then(function(result) {
    console.log("成功" + result);
});
var p3 = p1.catch(function(reason) {
    console.log("失败" + reason);
});

// 方式二
new Promise(test).then(function(result) {
    console.log("成功" + result);
}).catch(function(reason) {
    console.log("失败" + reason);
});


//方式三
<div id="test-promise-log">

</div>


var logging = document.getElementById("test-promise-log");
while (logging.children.length > 1) {
    logging.removeChild(logging.children[logging.children.length - 1]);
}

function log(s) {
    var p = document.createElement("p");
    p.innerHTML = s;
    logging.appendChild(p);
}


new Promise(function(resolve, reject) {
    log("start new Promise...");
    var timeout = Math.random() * 2;
    log('set timeout to: ' + timeout + 'seconds.');
    setTimeout(function() {
        if (timeout < 1) {
            log('call resolve()...');
            resolve('200 OK');
        } else {
            log('call reject()...');
            reject('timeout in ' + timeout + 'sconds.');
        }
    }, timeout * 1000);

}).then(function(result) {
    log('Done: ' + result);
}).catch(function(reason) {
    log("Failed: " + reason);
});


------------

start new Promise...

set timeout to: 1.6579586637257697seconds.

call reject()...

Failed: timeout in 1.6579586637257697sconds.

```

可见 Promise 的最大好处就是在异步执行的流程中，将执行代码和处理结果的代码清晰地分离了。

