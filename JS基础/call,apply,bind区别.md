# call,apply,bind区别
### 相同点
都可以改变this的指向
### 区别点
1. call 和 apply 会调用函数，并且改变函数内部的this指向。
2. call 和 apply 传递的参数不一样，call传递参数以aru1，aru2这种形式，而apply必须以数组的形式[arg]。
3. bind 不会调用函数，可以改变函数内部的this指向。
### 主要应用的场景
1. call适用于经常做继承。
2. apply适用于经常和数组有关系。比如借组于数字对象实现数组最大值最小值。
3. bind适用于不调用函数，但是还想要改变this的指向，比如改变定时器内部的this指向。
