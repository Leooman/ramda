# List

## length(数组长度)

`[a] → Number `

```javascript
R.length([]); //=> 0
R.length([1, 2, 3]); //=> 3
```

## adjust(对指定索引的元素执行操作)

`(a → a) → Number → [a] → [a] `

```javascript
//第一个参数为元素要执行的函数
//第二个参数为索引index
//第三个参数为array-like列表对象
R.adjust(R.add(10), 1, [1, 2, 3]);     //=> [1, 12, 3]
R.adjust(R.add(10))(1)([1, 2, 3]);     //=> [1, 12, 3]
```

## all(所有成员是否满足判定规则)

`(a → Boolean) → [a] → Boolean `

```javascript
var equals3 = R.equals(3);
R.all(equals3)([3, 3, 3, 3]); //=> true
R.all(equals3)([3, 3, 1, 3]); //=> false
```

## any(是否有成员满足判定规则)

`(a → Boolean) → [a] → Boolean `

```javascript
var lessThan0 = R.flip(R.lt)(0);
var lessThan2 = R.flip(R.lt)(2);
R.any(lessThan0)([1, 2]); //=> false
R.any(lessThan2)([1, 2]); //=> true
```

## none(是否所有成员都不满足判定规则)

`(a → Boolean) → [a] → Boolean `

```javascript
var isEven = n => n % 2 === 0;
var isOdd = n => n % 2 === 1;

R.none(isEven, [1, 3, 5, 7, 9, 11]); //=> true
R.none(isOdd, [1, 3, 5, 7, 8, 11]); //=> false
```

## aperture(指定数量的数组分割重组)

`Number → [a] → [[a]] `

```javascript
//第一个参数为n指定数量
//第二个参数为目标数组
//每个数组成员与其后的n-1个成员分为一组，生成一个新数组
R.aperture(2, [1, 2, 3, 4, 5]); //=> [[1, 2], [2, 3], [3, 4], [4, 5]]
R.aperture(3, [1, 2, 3, 4, 5]); //=> [[1, 2, 3], [2, 3, 4], [3, 4, 5]]
R.aperture(7, [1, 2, 3, 4, 5]); //=> []
```

## append(数组后置添加Array.prototype.push)

`a → [a] → [a] `

```javascript
R.append('tests', ['write', 'more']); //=> ['write', 'more', 'tests']
R.append('tests', []); //=> ['tests']
R.append(['tests'], ['write', 'more']); //=> ['write', 'more', ['tests']]
```

## prepend(数组前置添加Array.prototype.unshift)

`a → [a] → [a] `

```javascript
R.prepend('fee', ['fi', 'fo', 'fum']); //=> ['fee', 'fi', 'fo', 'fum']
```

## chain(每个成员执行指定链式操作)

`Chain m => (a → m b) → m a → m b `

```javascript
var duplicate = n => [n, n];
R.chain(duplicate, [1, 2, 3]); //=> [1, 1, 2, 2, 3, 3]

R.chain(R.append, R.head)([1, 2, 3]); //=> [1, 2, 3, 1]
```

## concat(合并)

`[a] → [a] → [a] ` `String → String → String `

```javascript
R.concat('ABC', 'DEF'); // 'ABCDEF'
R.concat([4, 5, 6], [1, 2, 3]); //=> [4, 5, 6, 1, 2, 3]
R.concat([], []); //=> []
```

## contains(列表是否包含指定元素)

`a → [a] → Boolean `

```javascript
R.contains(3, [1, 2, 3]); //=> true
R.contains(4, [1, 2, 3]); //=> false
R.contains({ name: 'Fred' }, [{ name: 'Fred' }]); //=> true
R.contains([42], [[42]]); //=> true
```

## drop(删除前n个元素)

`Number → [a] → [a] ` `Number → String → String `

```javascript
R.drop(1, ['foo', 'bar', 'baz']); //=> ['bar', 'baz']
R.drop(2, ['foo', 'bar', 'baz']); //=> ['baz']
R.drop(3, ['foo', 'bar', 'baz']); //=> []
R.drop(4, ['foo', 'bar', 'baz']); //=> []
R.drop(3, 'ramda');               //=> 'da'
```

## dropWhile(删除符合规则的前n个元素)

`(a → Boolean) → [a] → [a] ` `(a → Boolean) → String → String `

```javascript
var lteTwo = x => x <= 2;

R.dropWhile(lteTwo, [1, 2, 3, 4, 3, 2, 1]); //=> [3, 4, 3, 2, 1]

R.dropWhile(x => x !== 'd' , 'Ramda'); //=> 'da'
```

## dropLast(删除后n个元素)

`Number → [a] → [a] ` `Number → String → String `

```javascript
R.dropLast(1, ['foo', 'bar', 'baz']); //=> ['foo', 'bar']
R.dropLast(2, ['foo', 'bar', 'baz']); //=> ['foo']
R.dropLast(3, ['foo', 'bar', 'baz']); //=> []
R.dropLast(4, ['foo', 'bar', 'baz']); //=> []
R.dropLast(3, 'ramda');               //=> 'ra'
```

## dropLastWhile(删除符合规则的后n个元素)

`(a → Boolean) → [a] → [a] ` `(a → Boolean) → String → String `

```javascript
var lteThree = x => x <= 3;

R.dropLastWhile(lteThree, [1, 2, 3, 4, 3, 2, 1]); //=> [1, 2, 3, 4]

R.dropLastWhile(x => x !== 'd' , 'Ramda'); //=> 'Ramd'
```

## dropRepeats(去除连续重复元素)

`[a] → [a] `

```javascript
//内置R.equals判断是否重复
R.dropRepeats([1, 1, 1, 2, 3, 4, 4, 2, 2]); //=> [1, 2, 3, 4, 2]
```

## dropRepeatsWith(指定规则去除连续重复元素)

`((a, a) → Boolean) → [a] → [a] `

```javascript
var l = [1, -1, 1, 3, 4, -4, -4, -5, 5, 3, 3];
R.dropRepeatsWith(R.eqBy(Math.abs), l); //=> [1, 3, 4, -5, 3]
```

## endsWith(判断是否以指定元素结尾)

`[a] → Boolean ` `String → Boolean `

```javascript
R.endsWith('c', 'abc')                //=> true
R.endsWith('b', 'abc')                //=> false
R.endsWith(['c'], ['a', 'b', 'c'])    //=> true
R.endsWith(['b'], ['a', 'b', 'c'])    //=> false
```

## startsWith(判断是否以指定元素开始)

`[a] → Boolean ` `String → Boolean `

```javascript
R.startsWith('a', 'abc')                //=> true
R.startsWith('b', 'abc')                //=> false
R.startsWith(['a'], ['a', 'b', 'c'])    //=> true
R.startsWith(['b'], ['a', 'b', 'c'])    //=> false
```

## filter(符合规则的成员集合)

`Filterable f => (a → Boolean) → f a → f a `

```javascript
var isEven = n => n % 2 === 0;

R.filter(isEven, [1, 2, 3, 4]); //=> [2, 4]

R.filter(isEven, {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, d: 4}
```

## reject(不符合规则的成员集合)

`Filterable f => (a → Boolean) → f a → f a `

```javascript
var isOdd = (n) => n % 2 === 1;

R.reject(isOdd, [1, 2, 3, 4]); //=> [2, 4]

R.reject(isOdd, {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, d: 4}
```

## find(正序查找第一个符合规则的元素)

`(a → Boolean) → [a] → a | undefined `

```javascript
var xs = [{a: 1}, {a: 2}, {a: 3}, {a: 2}];
R.find(R.propEq('a', 2))(xs); //=> {a: 2}
R.find(R.propEq('a', 4))(xs); //=> undefined
```

## findIndex(正序查找第一个符合规则的元素索引)

`(a → Boolean) → [a] → Number `

```javascript
var xs = [{a: 1}, {a: 2}, {a: 3}, {a: 2}];
R.findIndex(R.propEq('a', 2))(xs); //=> 1
R.findIndex(R.propEq('a', 4))(xs); //=> -1
```

## findLast(倒序查找第一个符合规则的元素)

`(a → Boolean) → [a] → a | undefined `

```javascript
var xs = [{a: 1, b: 0}, {a:1, b: 1}];
R.findLast(R.propEq('a', 1))(xs); //=> {a: 1, b: 1}
R.findLast(R.propEq('a', 4))(xs); //=> undefined
```

## findLastIndex(倒序查找第一个符合规则的元素索引)

`(a → Boolean) → [a] → Number `

```javascript
var xs = [{a: 1, b: 0}, {a:1, b: 1}];
R.findLastIndex(R.propEq('a', 1))(xs); //=> 1
R.findLastIndex(R.propEq('a', 4))(xs); //=> -1
```

## flatten(多维转一维)

`[a] → [b] `

```javascript
R.flatten([1, 2, [3, 4], 5, [6, [7, 8, [9, [10, 11], 12]]]]);
//=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```

## forEach(遍历数组)

`(a → *) → [a] → [a] `

```javascript
var printXPlusFive = x => console.log(x + 5);
R.forEach(printXPlusFive, [1, 2, 3]); //=> [1, 2, 3]
// logs 6
// logs 7
// logs 8
```

## fromPairs(数组转对象)

`[[k,v]] → {k: v} `

```javascript
R.fromPairs([['a', 1], ['b', 2], ['c', 3]]); //=> {a: 1, b: 2, c: 3}
```

## groupBy(所有成员指定规则分组)

`(a → String) → [a] → {String: [a]} `

```javascript
var byGrade = R.groupBy(function(student) {
  var score = student.score;
  return score < 65 ? 'F' :
         score < 70 ? 'D' :
         score < 80 ? 'C' :
         score < 90 ? 'B' : 'A';
});
var students = [{name: 'Abby', score: 84},
                {name: 'Eddy', score: 58},
                // ...
                {name: 'Jack', score: 69}];
byGrade(students);
// {
//   'A': [{name: 'Dianne', score: 99}],
//   'B': [{name: 'Abby', score: 84}]
//   // ...,
//   'F': [{name: 'Eddy', score: 58}]
// }
```

## groupWith(相邻元素指定规则分组)

`((a, a) → Boolean) → [a] → [[a]] `

```javascript
R.groupWith(R.equals, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0], [1, 1], [2], [3], [5], [8], [13], [21]]

R.groupWith((a, b) => a + 1 === b, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0, 1], [1, 2, 3], [5], [8], [13], [21]]

R.groupWith((a, b) => a % 2 === b % 2, [0, 1, 1, 2, 3, 5, 8, 13, 21])
//=> [[0], [1, 1], [2], [3, 5], [8], [13, 21]]
```

## head(第一个元素)

`[a] → a | Undefined ` `String → String `

```javascript
R.head(['fi', 'fo', 'fum']); //=> 'fi'
R.head([]); //=> undefined

R.head('abc'); //=> 'a'
R.head(''); //=> ''
```

## last(最后一个元素)

`[a] → a | Undefined ` `String → String `

```javascript
R.last(['fi', 'fo', 'fum']); //=> 'fum'
R.last([]); //=> undefined

R.last('abc'); //=> 'c'
R.last(''); //=> ''
```

## indexBy(指定属性生成新对象)

`(a → String) → [{k: v}] → {k: {k: v}} `

```javascript
var list = [{id: 'xyz', title: 'A'}, {id: 'abc', title: 'B'}];
R.indexBy(R.prop('id'), list);
//=> {abc: {id: 'abc', title: 'B'}, xyz: {id: 'xyz', title: 'A'}}
```

## indexOf(正序查找元素在数组中第一次出现的位置)

`a → [a] → Number `

```javascript
R.indexOf(3, [1,2,3,4]); //=> 2
R.indexOf(10, [1,2,3,4]); //=> -1
```

## lastIndexOf(倒序查找元素在数组中第一次出现的位置)

`a → [a] → Number `

```javascript
R.lastIndexOf(3, [-1,3,3,0,1,2,3,4]); //=> 6
R.lastIndexOf(10, [1,2,3,4]); //=> -1
```

## init(去尾)

`[a] → [a] ` `String → String `

```javascript
R.init([1, 2, 3]);  //=> [1, 2]
R.init([1, 2]);     //=> [1]
R.init([1]);        //=> []
R.init([]);         //=> []

R.init('abc');  //=> 'ab'
R.init('ab');   //=> 'a'
R.init('a');    //=> ''
R.init('');     //=> ''
```

## tail(去头)

`[a] → [a] ` `String → String `

```javascript
R.tail([1, 2, 3]);  //=> [2, 3]
R.tail([1, 2]);     //=> [2]
R.tail([1]);        //=> []
R.tail([]);         //=> []

R.tail('abc');  //=> 'bc'
R.tail('ab');   //=> 'b'
R.tail('a');    //=> ''
R.tail('');     //=> ''
```

## insert(指定索引处插入一个值)

`Number → a → [a] → [a] `

```javascript
R.insert(2, 'x', [1,2,3,4]); //=> [1,2,'x',3,4]
```

## insertAll(指定索引处插入一个数组的所有值)

`Number → [a] → [a] → [a] `

```javascript
R.insertAll(2, ['x','y','z'], [1,2,3,4]); //=> [1,2,'x','y','z',3,4]
```

## intersperse(成员间插入间隔值)

`a → [a] → [a] `

```javascript
R.intersperse('n', ['ba', 'a', 'a']); //=> ['ba', 'n', 'a', 'n', 'a']
```

## into(原数组处理后结果与目标对象合并)

`a → (b → b) → [c] → a `

```javascript
var numbers = [1, 2, 3, 4];
var transducer = R.compose(R.map(R.add(1)), R.take(2));

R.into([], transducer, numbers); //=> [2, 3]

var intoArray = R.into("jsfkd");
intoArray(transducer, numbers); //=> "jsfkd23"
```

## join(数组转字符串)

`String → [a] → String `

```javascript
var spacer = R.join(' ');
spacer(['a', 2, 3.4]);   //=> 'a 2 3.4'
R.join('|', [1, 2, 3]);    //=> '1|2|3'
```

## map(每个数组成员执行指定操作)

`Functor f => (a → b) → f a → f b `

```javascript
var double = x => x * 2;

R.map(double, [1, 2, 3]); //=> [2, 4, 6]

R.map(double, {x: 1, y: 2, z: 3}); //=> {x: 2, y: 4, z: 6}
```

## mapAccum(元素左侧累加)

`((acc, x) → (acc, y)) → acc → [x] → (acc, [y]) `

```javascript
//第一个参数为迭代函数，接受两个参数acc和当前x,返回[当前累加值acc，当前迭代结果y]
//第二个参数为累加初始值acc
//第三个参数为目标数组，从左侧开始传入执行
//该方法类似map和reduce的结合，返回最终acc累加值与迭代期间产生的结果集合生成的一个数组 [累加值acc,[...迭代结果y]]
var digits = ['1', '2', '3', '4'];
var appender = (a, b) => [a + b, a + b];

R.mapAccum(appender, 0, digits); //=> ['01234', ['01', '012', '0123', '01234']]

var digits = ['1', '2', '3', '4'];
var appender = (a, b) => [a+1, a + b];

R.mapAccum(appender, 0, digits); //=> [4, ['01', '12', '23', '34']]
```

## mapAccumRight(元素右侧累加)

`((x, acc) → (acc, y)) → acc → [x] → ([y], acc) `

```javascript
//第一个参数为迭代函数，接受两个参数当前x和acc,返回[当前累加值acc，当前迭代结果y]
//第二个参数为累加初始值acc
//第三个参数为目标数组,从右侧开始传入执行
//该方法类似map和reduce的结合，返回最终acc累加值与迭代期间产生的结果集合生成的一个数组 [[...迭代结果y],累加值acc]
var digits = ['1', '2', '3', '4'];
var append = (a, b) => [a + b, a + b];

R.mapAccumRight(append, 5, digits); //=> [['12345', '2345', '345', '45'], '12345']

var digits = ['1', '2', '3', '4'];
var append = (a, b) => [b+1, a + b];

R.mapAccumRight(append, 5, digits); //=> [['18', '27', '36', '45'], 9]
```

## mergeAll(对象数组成员合并为一个对象)

`[{k: v}] → {k: v} `

```javascript
R.mergeAll([{foo:1},{bar:2},{baz:3}]); //=> {foo:1,bar:2,baz:3}
R.mergeAll([{foo:1},{foo:2},{bar:2}]); //=> {foo:2,bar:2}
```

## nth(指定索引处的元素)

`Number → [a] → a | Undefined ` `Number → String → String `

```javascript
var list = ['foo', 'bar', 'baz', 'quux'];
R.nth(1, list); //=> 'bar'
R.nth(-1, list); //=> 'quux'
R.nth(-99, list); //=> undefined

R.nth(2, 'abc'); //=> 'c'
R.nth(3, 'abc'); //=> ''
```

## pair(生成数组)

`a → b → [a,b] `

```javascript
R.pair('foo', 'bar'); //=> ['foo', 'bar']
```

## partition(规则过滤)

`Filterable f => (a → Boolean) → f a → [f a, f a] `

```javascript
//符合规则的作为数组第一个元素，不符合的作为第二个元素

R.partition(R.contains('s'), ['sss', 'ttt', 'foo', 'bars']);
// => [ [ 'sss', 'bars' ],  [ 'ttt', 'foo' ] ]

R.partition(R.contains('s'), { a: 'sss', b: 'ttt', foo: 'bars' });
// => [ { a: 'sss', foo: 'bars' }, { b: 'ttt' }  ]
```

## pluck(获取指定属性名的属性值集合)

`Functor f => k → f {k: v} → f v `

```javascript
//第一个参数为key键名或者索引
R.pluck('a')([{a: 1}, {a: 2}]); //=> [1, 2]
R.pluck(0)([[1, 2], [3, 4]]);   //=> [1, 3]
R.pluck('val', {a: {val: 3}, b: {val: 5}}); //=> {a: 3, b: 5}
```

## range(范围内数值集合)

`Number → Number → [Number] `

```javascript
R.range(1, 5);    //=> [1, 2, 3, 4]
R.range(50, 53);  //=> [50, 51, 52]
```

## scan(递归操作返回数组)

`((a, b) → a) → a → [b] → [a] `

```javascript
var numbers = [1, 2, 3, 4];
var factorials = R.scan(R.multiply, 1, numbers); //=> [1, 1, 2, 6, 24]
```

## reduce(递归操作返回最终值)

`((a, b) → a) → a → [b] → a `

```javascript
//第一个参数为执行函数，接收两个参数，初始值和当前数组成员
//第二个参数为累加初始值
//第三个参数为目标数组
//数组成员依次执行指定函数，每一次的运算结果都会进入一个累积变量
R.reduce(R.subtract, 0, [1, 2, 3, 4]) // => ((((0 - 1) - 2) - 3) - 4) = -10
//          -               -10
//         / \              / \
//        -   4           -6   4
//       / \              / \
//      -   3   ==>     -3   3
//     / \              / \
//    -   2           -1   2
//   / \              / \
//  0   1            0   1
```

## reduceBy(分组递归操作)

`((a, b) → a) → a → (b → String) → [b] → {String: a} `

```javascript
//第一个参数为valueFn，递归操作每一组数据得到一个单值。接受两个参数，每组的累加初始值和当前成员
//第二个参数为每组的累加初始值
//第三个参数为keyFn，对每个成员执行操作得到键值
//第四个参数为目标数组
var reduceToNamesBy = R.reduceBy((acc, student) => acc.concat(student.name), []);
var namesByGrade = reduceToNamesBy(function(student) {
  var score = student.score;
  return score < 65 ? 'F' :
         score < 70 ? 'D' :
         score < 80 ? 'C' :
         score < 90 ? 'B' : 'A';
});
var students = [{name: 'Lucy', score: 92},
                {name: 'Drew', score: 85},
                // ...
                {name: 'Bart', score: 62}];
namesByGrade(students);
// {
//   'A': ['Lucy'],
//   'B': ['Drew']
//   // ...,
//   'F': ['Bart']
// }
```

## reduced(递归终止)

`a → * `

```javascript
R.reduce(
 (acc, item) => item > 3 ? R.reduced(acc) : acc.concat(item),
 [],
 [1, 2, 3, 4, 5]) // [1, 2, 3]
```

## reduceRight(倒序递归操作)

`((a, b) → b) → b → [a] → b `

```javascript
R.reduceRight(R.subtract, 0, [1, 2, 3, 4]) // => (1 - (2 - (3 - (4 - 0)))) = -2
//    -               -2
//   / \              / \
//  1   -            1   3
//     / \              / \
//    2   -     ==>    2  -1
//       / \              / \
//      3   -            3   4
//         / \              / \
//        4   0            4   0
```

## reduceWhile(不符合条件终止递归操作)

`((a, b) → Boolean) → ((a, b) → a) → a → [b] → a `

```javascript
var isOdd = (acc, x) => x % 2 === 1;
var xs = [1, 3, 5, 60, 777, 800];
R.reduceWhile(isOdd, R.add, 0, xs); //=> 9

var ys = [2, 4, 6]
R.reduceWhile(isOdd, R.add, 111, ys); //=> 111
```

## remove(指定索引开始删除n个元素)

`Number → Number → [a] → [a] `

```javascript
R.remove(2, 3, [1,2,3,4,5,6,7,8]); //=> [1,2,6,7,8]
```

## repeat(重复n次值)

`a → n → [a] `

```javascript
R.repeat('hi', 5); //=> ['hi', 'hi', 'hi', 'hi', 'hi']

var obj = {};
var repeatedObjs = R.repeat(obj, 5); //=> [{}, {}, {}, {}, {}]
repeatedObjs[0] === repeatedObjs[1]; //=> true
```

## times(重复n次函数)

`(Number → a) → Number → [a] `

```javascript
//第一个参数为fn，当前参数值为val
//第二个参数为n，val值为0~n-1，每次fn执行完自增
R.times(R.identity, 5); //=> [0, 1, 2, 3, 4]
```

## reverse(翻转操作)

`[a] → [a] ` `String → String `

```javascript
R.reverse([1, 2, 3]);  //=> [3, 2, 1]
R.reverse([1, 2]);     //=> [2, 1]
R.reverse([1]);        //=> [1]
R.reverse([]);         //=> []

R.reverse('abc');      //=> 'cba'
R.reverse('ab');       //=> 'ba'
R.reverse('a');        //=> 'a'
R.reverse('');         //=> ''
```

## slice(截取操作)

`Number → Number → [a] → [a] ` `Number → Number → String → String `

```javascript
//第一个参数为开始索引
//第二个参数为结束索引
R.slice(1, 3, ['a', 'b', 'c', 'd']);        //=> ['b', 'c']
R.slice(1, Infinity, ['a', 'b', 'c', 'd']); //=> ['b', 'c', 'd']
R.slice(0, -1, ['a', 'b', 'c', 'd']);       //=> ['a', 'b', 'c']
R.slice(-3, -1, ['a', 'b', 'c', 'd']);      //=> ['b', 'c']
R.slice(0, 3, 'ramda');                     //=> 'ram'
```

## sort(数组排序)

`((a, a) → Number) → [a] → [a] `

```javascript
var diff = function(a, b) { return a - b; };
R.sort(diff, [4,2,7,5]); //=> [2, 4, 5, 7]
```

## splitAt(指定索引处分割数组)

`Number → [a] → [[a], [a]] ` `Number → String → [String, String] `

```javascript
R.splitAt(1, [1, 2, 3]);          //=> [[1], [2, 3]]
R.splitAt(5, 'hello world');      //=> ['hello', ' world']
R.splitAt(-1, 'foobar');          //=> ['fooba', 'r']
```

## splitEvery(指定数量分割数组)

`Number → [a] → [[a]] ` `Number → String → [String] `

```javascript
R.splitEvery(3, [1, 2, 3, 4, 5, 6, 7]); //=> [[1, 2, 3], [4, 5, 6], [7]]
R.splitEvery(3, 'foobarbaz'); //=> ['foo', 'bar', 'baz']
```

## splitWhen(规则第一次通过处分割数组)

`(a → Boolean) → [a] → [[a], [a]] `

```javascript
R.splitWhen(R.equals(2), [1, 2, 3, 1, 2, 3]);   //=> [[1], [2, 3, 1, 2, 3]]
```

## take(正序取n个值)

`Number → [a] → [a] ` `Number → String → String `

```javascript
R.take(1, ['foo', 'bar', 'baz']); //=> ['foo']
R.take(2, ['foo', 'bar', 'baz']); //=> ['foo', 'bar']
R.take(3, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.take(4, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.take(3, 'ramda');               //=> 'ram'
```

## takeWhile(正序取符合规则的值集合)

`(a → Boolean) → [a] → [a] ` `(a → Boolean) → String → String `

```javascript
//正序取值，在第一次不符合规则处停止
var isNotFour = x => x !== 4;

R.takeWhile(isNotFour, [1, 2, 3, 4, 3, 2, 1]); //=> [1, 2, 3]

R.takeWhile(x => x !== 'd' , 'Ramda'); //=> 'Ram'
```

## takeLast(倒序取n个值)

`Number → [a] → [a] ` `Number → String → String `

```javascript
R.takeLast(1, ['foo', 'bar', 'baz']); //=> ['baz']
R.takeLast(2, ['foo', 'bar', 'baz']); //=> ['bar', 'baz']
R.takeLast(3, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.takeLast(4, ['foo', 'bar', 'baz']); //=> ['foo', 'bar', 'baz']
R.takeLast(3, 'ramda');               //=> 'mda'
```

## takeLastWhile(倒序取符合规则的值集合)

`(a → Boolean) → [a] → [a] ` `(a → Boolean) → String → String `

```javascript
//倒序取值，在第一次不符合规则处停止
var isNotOne = x => x !== 1;

R.takeLastWhile(isNotOne, [1, 2, 1, 3, 4]); //=> [3, 4]

R.takeLastWhile(x => x !== 'R' , 'Ramda'); //=> 'amda'
```

## transduce(数组迭代转换)

`(c → c) → ((a, b) → a) → a → [b] → a `

```javascript
//第一个参数为转换函数
//第二个参数为迭代函数，接收两个参数，累加值acc和当前元素值value
//第三个参数为累加初始值
//第四个参数为目标数组
var numbers = [1, 2, 3, 4];
var transducer = R.compose(R.map(R.add(1)), R.take(2));
R.transduce(transducer, R.flip(R.append), [], numbers); //=> [2, 3]

var isOdd = (x) => x % 2 === 1;
var firstOddTransducer = R.compose(R.filter(isOdd), R.take(5));
R.transduce(firstOddTransducer, R.flip(R.append), [], R.range(0, 100)); //=> [1,3,5,7,9]
```

## transpose(相同索引的值集合)

`[[a]] → [[a]] `

```javascript
R.transpose([[1, 'a'], [2, 'b'], [3, 'c']]) //=> [[1, 2, 3], ['a', 'b', 'c']]
R.transpose([[1, 2, 3], ['a', 'b', 'c']]) //=> [[1, 'a'], [2, 'b'], [3, 'c']]

// If some of the rows are shorter than the following rows, their elements are skipped:
R.transpose([[10, 11], [20], [], [30, 31, 32]]) //=> [[10, 20, 30], [11, 31], [32]]
```

## unfold(指定规则的迭代操作)

`(a → [b]) → * → [b] `

```javascript
//第一个参数为fn一个迭代函数，接受一个参数，判定为false时停止迭代，判定为true时返回一个包含两个元素的数组。数组的第一个元素添加到结果数组中，第二个元素作为下一次迭代的参数值
//第二个参数为迭代函数的初始值
var f = n => n > 50 ? false : [-n, n + 10];
R.unfold(f, 10); //=> [-10, -20, -30, -40, -50]
```

## uniq(数组去重)

`[a] → [a] `

```javascript
R.uniq([1, 1, 2, 1]); //=> [1, 2]
R.uniq([1, '1']);     //=> [1, '1']
R.uniq([[42], [42]]); //=> [[42]]
```

## uniqBy(指定操作去重)

`(a → b) → [a] → [a] `

```javascript
R.uniqBy(Math.abs, [-1, -5, 2, 10, 1, 2]); //=> [-1, -5, 2, 10]
```

## uniqWith(指定规则去重)

`((a, a) → Boolean) → [a] → [a] `

```javascript
var strEq = R.eqBy(String);
R.uniqWith(strEq)([1, '1', 2, 1]); //=> [1, 2]
R.uniqWith(strEq)([{}, {}]);       //=> [{}]
R.uniqWith(strEq)([1, '1', 1]);    //=> [1]
R.uniqWith(strEq)(['1', 1, 1]);    //=> ['1']
```

## unnest(降维操作)

`Chain c => c (c a) → c a `

```javascript
R.unnest([1, [2], [[3]]]); //=> [1, 2, [3]]
R.unnest([[1, 2], [3, 4], [5, 6]]); //=> [1, 2, 3, 4, 5, 6]
```

## update(修改指定索引处的值)

`Number → a → [a] → [a] `

```javascript
R.update(1, 11, [0, 1, 2]);     //=> [0, 11, 2]
R.update(1)(11)([0, 1, 2]);     //=> [0, 11, 2]
```

## without(去除与指定数组元素相同的所有元素)

`[a] → [a] → [a] `

```javascript
//第一个参数为要去除的元素集合
//第二个参数为目标数组
R.without([1, 2], [1, 2, 1, 3, 4]); //=> [3, 4]
```

## xprod(数组混搭集合)

`[a] → [b] → [[a,b]] `

```javascript
R.xprod([1, 2], ['a', 'b']); //=> [[1, 'a'], [1, 'b'], [2, 'a'], [2, 'b']]
```

## zip(两个数组相同索引处的值匹配成新数组)

`[a] → [b] → [[a,b]] `

```javascript
R.zip([1, 2, 3], ['a', 'b', 'c']); //=> [[1, 'a'], [2, 'b'], [3, 'c']]
R.zip([1, 2, 3, 4], ['a', 'b', 'c']); //=> [[1, 'a'], [2, 'b'], [3, 'c']]
```

## zipObj(两个数组匹配成一个对象)

`[String] → [*] → {String: *} `

```javascript
//第一个参数为键名集合
//第二个参数为键值集合
R.zipObj(['a', 'b', 'c'], [1, 2, 3]); //=> {a: 1, b: 2, c: 3}
```

## zipWith(两个数组相同索引处的值传入指定函数匹配成函数数组)

`((a, b) → c) → [a] → [b] → [c] `

```javascript
var f = (x, y) => {
  // ...
};
R.zipWith(f, [1, 2, 3], ['a', 'b', 'c']);
//=> [f(1, 'a'), f(2, 'b'), f(3, 'c')]
```



