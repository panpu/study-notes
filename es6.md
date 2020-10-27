# study-notes
1.es6中，对象声明函数属性可以使用简写，但简写不能作为构造函数，会报错
2.深拷贝的2种数据
数据类型判断

```
console.log(Object.prototype.toString.call("jerry"));//[object String]
console.log(Object.prototype.toString.call(12));//[object Number]
console.log(Object.prototype.toString.call(true));//[object Boolean]
console.log(Object.prototype.toString.call(undefined));//[object Undefined]
console.log(Object.prototype.toString.call(null));//[object Null]
console.log(Object.prototype.toString.call({name: "jerry"}));//[object Object]
console.log(Object.prototype.toString.call(function(){}));//[object Function]
console.log(Object.prototype.toString.call([]));//[object Array]
console.log(Object.prototype.toString.call(new Date));//[object Date]
console.log(Object.prototype.toString.call(/\d/));//[object RegExp]
console.log(Object.prototype.toString.call(new Person));//[object Object]
```

  1.手动递归

  1.手动递归

```
   //定义一个空对象
    let a = {
        name: 'zhangsan',
        age: 24,
        skill: {
            Structure: 'html',
            style: 'css',
            behavior: 'js'
        },
        family: ['daddy', 'mom', 'me', 'wife', 'child']
    }
    let b = [1, 2, {
        skill: {
            Structure: 'html',
            style: 'css',
            behavior: 'js'
        }
    }, [112, [123, 23]]]
    //a为需要深拷贝的对象
    function deepClone(clone) {
        var obj
        if (Object.prototype.toString.call(clone) === '[object Object]') {
            obj = {}
        } else if (Object.prototype.toString.call(clone) === '[object Array]') {
            obj = []
        } else {
            throw Error('该类型不支持深拷贝')
        }
        cb(clone,obj)
        return obj
    }
    function cb(clone,obj){
        //如果需要深拷贝的是数组
        if (Object.prototype.toString.call(clone) === '[object Object]') {
            Object.keys(clone).forEach((item, index) => {
                if (Object.prototype.toString.call(Object.values(clone)[index]) === '[object Object]') {
                    obj[item] = {}
                    cb(Object.values(clone)[index], obj[item])
                } else if (Object.prototype.toString.call(Object.values(clone)[index]) === '[object Array]') {
                    obj[item] = []
                    cb(Object.values(clone)[index], obj[item])
                } else {
                    obj[item] = Object.values(clone)[index]
                }
            })
        } else if (Object.prototype.toString.call(clone) === '[object Array]') {
            clone.forEach((item, index) => {
                if (Object.prototype.toString.call(item) === '[object Object]') {
                    obj.push({})
                    cb(item, obj[index])
                } else if (Object.prototype.toString.call(item) === '[object Array]') {
                    obj.push([])
                    cb(item, obj[index])
                } else {
                    obj.push(item)
                }
            })
        }
    }
    let obj = deepClone(a)
    let arr = deepClone(b)
    console.log(obj);
    console.log(arr);
```

3.关于babel的转译

给出一个日常编译的初始代码段，如下：

```
// let b 

// new Promise((resolve, reject) => {

//   setTimeout(() => {

//     resolve(0)

//   }, 1000)

// }).then(res=>{

//   b=res

//   let fn = async () => {

//     await setTimeout(()=>{

//       b=1

//     },1000)

//   }

//   fn()

// })
```

使用最新版babel编译后

```
function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) {
  try {
    var info = gen[key](arg);
    var value = info.value;
  } catch (error) {
    reject(error);
    return;
  }
  if (info.done) {
    resolve(value);
  } else {
    Promise.resolve(value).then(_next, _throw);
  }
}
function _asyncToGenerator(fn) {
  return function () {
    var self = this, args = arguments;
    return new Promise(function (resolve, reject) {
      var gen = fn.apply(self, args);
      function _next(value) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value);
      }
      function _throw(err) {
        asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err);
      }
      _next(undefined);
    });
  };
}
var b;
new Promise(function (resolve, reject) {
  setTimeout(function () {
    resolve(0);
  }, 1000);
}).then(function (res) {
  b = res;
  var fn = function () {
    var _ref = _asyncToGenerator(regeneratorRuntime.mark(function _callee() {
      return regeneratorRuntime.wrap(function _callee$(_context) {
        while (1) {
          switch (_context.prev = _context.next) {
            case 0:
              _context.next = 2;
              return setTimeout(function () {
                b = 1;
              }, 1000);
            case 2:
            case "end":
              return _context.stop();
          }
        }
      }, _callee);
    })
    );
    return function fn() {
      return _ref.apply(this, arguments);
    };
  }();
  fn();
});
```

事实证明new Promise不会被新版本的babel转译

