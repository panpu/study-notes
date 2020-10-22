# study-notes
1.es6中，对象声明函数属性可以使用简写，但简写不能作为构造函数，会报错
2.深拷贝的2种数据
数据类型判断
`
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
  1.手动递归
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
    `
