# Object

## keys(对象自身属性的键集合)

`{k: v} → [k] `

```javascript
R.keys({a: 1, b: 2, c: 3}); //=> ['a', 'b', 'c']
```

## keysIn(对象自身和继承属性的键集合 )

`{k: v} → [k] `

```javascript
var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.keysIn(f); //=> ['x', 'y']
```

## values(对象自身属性值集合)

`{k: v} → [v] `

```javascript
R.values({a: 1, b: 2, c: 3}); //=> [1, 2, 3]
```

## valuesIn(对象自身和继承属性的值集合)

`{k: v} → [v] `

```javascript
var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.valuesIn(f); //=> ['X', 'Y']
```

## assoc(添加或重写指定属性)

`String → a → {k: v} → {k: v} `

```javascript
//第一个参数prop属性名
//第二个参数val属性值
//第三个参数target对象
//得到一个新的对象，原对象不变
R.assoc('c', 3, {a: 1, b: 2}); //=> {a: 1, b: 2, c: 3}
```

## assocPath(添加或重写指定路径的值)

`[Idx] → a → {a} → {a} ` `Idx = String | Int `

```javascript
//第一个参数path路径
//第二个参数val属性值
//第三个参数target对象
//得到一个新的对象，原对象不变
R.assocPath(['a', 'b', 'c'], 42, {a: {b: {c: 0}}}); //=> {a: {b: {c: 42}}}

// Any missing or non-object keys in path will be overridden
R.assocPath(['a', 'b', 'c'], 42, {a: 5}); //=> {a: {b: {c: 42}}}
```

## clone(深层克隆)

`{*} → {*} `

```javascript
var objects = [{}, {}, {}];
var objectsClone = R.clone(objects);
objects === objectsClone; //=> false
objects[0] === objectsClone[0]; //=> false
```

## dissoc(删除指定属性)

`String → {k: v} → {k: v} `

```javascript
//第一个参数prop属性名
//第三个参数target对象
//得到一个新的对象，原对象不变
R.dissoc('b', {a: 1, b: 2, c: 3}); //=> {a: 1, c: 3}
```

## dissocPath(删除指定路径的属性)

`[Idx] → {k: v} → {k: v} ` `Idx = String | Int `

```javascript
//第一个参数path路径
//第三个参数target对象
//得到一个新的对象，原对象不变
R.dissocPath(['a', 'b', 'c'], {a: {b: {c: 42}}}); //=> {a: {b: {}}}
```

## eqProps(属性值是否相等)

`k → {k: v} → {k: v} → Boolean `

```javascript
var o1 = { a: 1, b: 2, c: 3, d: 4 };
var o2 = { a: 10, b: 20, c: 3, d: 40 };
R.eqProps('a', o1, o2); //=> false
R.eqProps('c', o1, o2); //=> true
```

## evolve(自定义对象转换)

`{k: (v → v)} → {k: v} → {k: v} `

```javascript
//第一个参数transformations转换规则
//第三个参数target对象
//得到一个新的对象，原对象不变
var tomato  = {firstName: '  Tomato ', data: {elapsed: 100, remaining: 1400}, id:123};
var transformations = {
  firstName: R.trim,
  lastName: R.trim, // Will not get invoked.
  data: {elapsed: R.add(1), remaining: R.add(-1)}
};
R.evolve(transformations, tomato); //=> {firstName: 'Tomato', data: {elapsed: 101, remaining: 1399}, id:123}
```

## forEachObjIndexed(遍历对象)

`((a, String, StrMap a) → Any) → StrMap a → StrMap a `

```javascript
//第一个参数fn,接受三个参数value,key,obj
//第三个参数obj对象
//返回原对象
var printKeyConcatValue = (value, key) => console.log(key + ':' + value);
R.forEachObjIndexed(printKeyConcatValue, {x: 1, y: 2}); //=> {x: 1, y: 2}
// x:1
// y:2
```

## mapObjIndexed(遍历对象)

`((*, String, Object) → *) → Object → Object `

```javascript
//返回新对象
var values = { x: 1, y: 2, z: 3 };
var prependKeyAndDouble = (num, key, obj) => key + (num * 2);

R.mapObjIndexed(prependKeyAndDouble, values); //=> { x: 'x2', y: 'y4', z: 'z6' }
```

## has(对象自身是否具有指定属性)

`s → {s: x} → Boolean `

```javascript
var hasName = R.has('name');
hasName({name: 'alice'});   //=> true
hasName({name: 'bob'});     //=> true
hasName({});                //=> false

var point = {x: 0, y: 0};
var pointHas = R.has(R.__, point);
pointHas('x');  //=> true
pointHas('y');  //=> true
pointHas('z');  //=> false
```

## hasIn(对象自身或原型链 是否具有指定属性)

`s → {s: x} → Boolean `

```javascript
function Rectangle(width, height) {
  this.width = width;
  this.height = height;
}
Rectangle.prototype.area = function() {
  return this.width * this.height;
};

var square = new Rectangle(2, 2);
R.hasIn('width', square);  //=> true
R.hasIn('area', square);  //=> true
```

## invertObj(键值互换)

`{s: x} → {x: s} `

```javascript
var raceResults = {
  first: 'alice',
  second: 'jake'
};
R.invertObj(raceResults);
//=> { 'alice': 'first', 'jake':'second' }

// Alternatively:
var raceResults = ['alice', 'jake'];
R.invertObj(raceResults);
//=> { 'alice': '0', 'jake':'1' }

//如果多个属性的属性值相同，会发生值覆盖，只返回最后一个属性
var raceResultsByFirstName = {
  first: 'alice',
  second: 'jake',
  third: 'alice',
};
R.invertObj(raceResultsByFirstName)
//=> {"alice": "third", "jake": "second"}
```

## invert(键值互换)

`{s: x} → {x: [ s, … ]} `

```javascript
//每个属性值对应一个数组
var raceResultsByFirstName = {
  first: 'alice',
  second: 'jake',
  third: 'alice',
};
R.invert(raceResultsByFirstName);
//=> { 'alice': ['first', 'third'], 'jake':['second'] }

var raceResults = ['alice', 'jake', 'alice'];
R.invertObj(raceResults);
//=> { 'alice': ['0', '2'], 'jake': ['1'] }
```

## lens(指定属性的lens特殊对象)

`(s → a) → ((a, s) → s) → Lens s a ` `Lens s a = Functor f => (a → f a) → s → f s `

```javascript
//根据给定的getter和setter方法返回一个lens
//getter获取属性值
//setter修改属性值，但不应该改变数据结构

//创建一个lenses来聚焦到我们感兴趣的属性上，而不是使用getter和setter创建一个新的包裹对象
//prop是一个标准的getter方法，assoc是一个setter方法
var xLens = R.lens(R.prop('x'), R.assoc('x'));

R.view(xLens, {x: 1, y: 2});            //=> 1
R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
```

## lensIndex(指定索引)

`Number → Lens s a ` `Lens s a = Functor f => (a → f a) → s → f s `

```javascript
var headLens = R.lensIndex(0);

R.view(headLens, ['a', 'b', 'c']);            //=> 'a'
R.set(headLens, 'x', ['a', 'b', 'c']);        //=> ['x', 'b', 'c']
R.over(headLens, R.toUpper, ['a', 'b', 'c']); //=> ['A', 'b', 'c']
```

## lensPath(指定路径)

`[Idx] → Lens s a ` `Idx = String | Int ` `Lens s a = Functor f => (a → f a) → s → f s `

```javascript
var xHeadYLens = R.lensPath(['x', 0, 'y']);

R.view(xHeadYLens, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> 2
R.set(xHeadYLens, 1, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> {x: [{y: 1, z: 3}, {y: 4, z: 5}]}
R.over(xHeadYLens, R.negate, {x: [{y: 2, z: 3}, {y: 4, z: 5}]});
//=> {x: [{y: -2, z: 3}, {y: 4, z: 5}]}
```

## lensProp(指定属性)

`String → Lens s a ` `Lens s a = Functor f => (a → f a) → s → f s `

```javascript
var xLens = R.lensProp('x');

R.view(xLens, {x: 1, y: 2});            //=> 1
R.set(xLens, 4, {x: 1, y: 2});          //=> {x: 4, y: 2}
R.over(xLens, R.negate, {x: 1, y: 2});  //=> {x: -1, y: 2}
```

## view(获取指定属性值)

`Lens s a → s → a ` `Lens s a = Functor f => (a → f a) → s → f s `

```javascript
var xLens = R.lensProp('x');

R.view(xLens, {x: 1, y: 2});  //=> 1
R.view(xLens, {x: 4, y: 2});  //=> 4
```

## set(指定值修改属性)

`Lens s a → a → s → s ` `Lens s a = Functor f => (a → f a) → s → f s `

```javascript
var xLens = R.lensProp('x');

R.set(xLens, 4, {x: 1, y: 2});  //=> {x: 4, y: 2}
R.set(xLens, 8, {x: 1, y: 2});  //=> {x: 8, y: 2}
```

## over(指定方法修改属性)

`Lens s a → (a → a) → s → s ` `Lens s a = Functor f => (a → f a) → s → f s `

```javascript
var headLens = R.lensIndex(0);

R.over(headLens, R.toUpper, ['foo', 'bar', 'baz']); //=> ['FOO', 'bar', 'baz']
```

## merge(对象合并)

`{k: v} → {k: v} → {k: v} `

```javascript
//如果有同名属性，后面的值会覆盖掉前面的值
R.merge({ 'name': 'fred', 'age': 10 }, { 'age': 40 });
//=> { 'name': 'fred', 'age': 40 }

var resetToDefault = R.merge(R.__, {x: 0});
resetToDefault({x: 5, y: 2}); //=> {x: 0, y: 2}
```

## mergeDeepLeft(对象左合并)

`{a} → {a} → {a} `

```javascript
//两个值都是对象，这两个值将被递归合并
//如果有同名属性，使用第一个对象(左)属性值
R.mergeDeepLeft({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                { age: 40, sex: 'M', contact: { email: 'baa@example.com' }});
//=> { name: 'fred', age: 10, sex: 'M', contact: { email: 'moo@example.com' }}
```

## mergeDeepRight(对象右合并)

`{a} → {a} → {a} `

```javascript
//两个值都是对象，这两个值将被递归合并
//如果有同名属性，使用第二个对象(右)属性值
R.mergeDeepRight({ name: 'fred', age: 10, contact: { email: 'moo@example.com' }},
                 { age: 40, contact: { email: 'baa@example.com' }});
//=> { name: 'fred', age: 40, contact: { email: 'baa@example.com' }}
```

## mergeDeepWith(指定规则的对象合并)

`((a, a) → a) → {a} → {a} → {a} `

```javascript
//如果有同名属性，会使用指定的函数处理
R.mergeDeepWith(R.concat,
                { a: true, c: { values: [10, 20] }},
                { b: true, c: { values: [15, 35] }});
//=> { a: true, b: true, c: { values: [10, 20, 15, 35] }}
```

## mergeWith(指定规则的对象合并)

`((a, a) → a) → {a} → {a} → {a} `

```javascript
R.mergeWith(R.concat,
            { a: true, values: [10, 20] },
            { b: true, values: [15, 35] });
//=> { a: true, b: true, values: [10, 20, 15, 35] }
```

## mergeDeepWithKey(指定键的对象合并)

`((String, a, a) → a) → {a} → {a} → {a} `

```javascript
//如果是指定键名，按指定函数处理
let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
R.mergeDeepWithKey(concatValues,
                   { a: true, c: { thing: 'foo', values: [10, 20] }},
                   { b: true, c: { thing: 'bar', values: [15, 35] }});
//=> { a: true, b: true, c: { thing: 'bar', values: [10, 20, 15, 35] }}
```

## mergeWithKey(指定键的对象合并)

`((String, a, a) → a) → {a} → {a} → {a} `

```javascript
//如果是指定键名，按指定函数处理
let concatValues = (k, l, r) => k == 'values' ? R.concat(l, r) : r
R.mergeDeepWithKey(concatValues,
                   { a: true, c: { thing: 'foo', values: [10, 20] }},
                   { b: true, c: { thing: 'bar', values: [15, 35] }});
//=> { a: true, b: true, c: { thing: 'bar', values: [10, 20, 15, 35] }}
```

## objOf(创建单键对象)

`String → a → {String:a}`

```javascript
var matchPhrases = R.compose(
  R.objOf('must'),
  R.map(R.objOf('match_phrase'))
);
matchPhrases(['foo', 'bar', 'baz']); //=> {must: [{match_phrase: 'foo'}, {match_phrase: 'bar'}, {match_phrase: 'baz'}]}
```

## omit(对象部分拷贝[去除指定键])

`[String] → {String: *} → {String: *} `

```javascript
R.omit(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {b: 2, c: 3}
```

## pick(对象拷贝[包含指定键]）

`[k] → {k: v} → {k: v} `

```javascript
//指定键不存在会被忽略掉
R.pick(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, d: 4}
R.pick(['a', 'e', 'f'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1}
```

## pickAll(对象拷贝[包含指定键]）

`[k] → {k: v} → {k: v} `

```javascript
//指定键不存在返回undefined
R.pickAll(['a', 'd'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, d: 4}
R.pickAll(['a', 'e', 'f'], {a: 1, b: 2, c: 3, d: 4}); //=> {a: 1, e: undefined, f: undefined}
```

## pickBy(对象拷贝[自定义规则])

`((v, k) → Boolean) → {k: v} → {k: v} `

```javascript
var isUpperCase = (val, key) => key.toUpperCase() === key;
R.pickBy(isUpperCase, {a: 1, b: 2, A: 3, B: 4}); //=> {A: 3, B: 4}
```

## path(指定路径的属性值)

`[Idx] → {a} → a | Undefined ` `Idx = String | Int `

```javascript
R.path(['a', 'b'], {a: {b: 2}}); //=> 2
R.path(['a', 'b'], {c: {b: 2}}); //=> undefined
```

## pathOr(指定路径的属性值/默认值)

`a → [Idx] → {a} → a ` `Idx = String | Int `

```javascript
//第一个参数为默认值
R.pathOr('N/A', ['a', 'b'], {a: {b: 2}}); //=> 2
R.pathOr('N/A', ['a', 'b'], {c: {b: 2}}); //=> "N/A"
```

## project(数组成员属性组选生成新数组 )

`[k] → [{k: v}] → [{k: v}] `

```javascript
var abby = {name: 'Abby', age: 7, hair: 'blond', grade: 2};
var fred = {name: 'Fred', age: 12, hair: 'brown', grade: 7};
var kids = [abby, fred];
R.project(['name', 'grade'], kids); //=> [{name: 'Abby', grade: 2}, {name: 'Fred', grade: 7}]

//数组成员中指定属性不存在返回undefined
var abby = {name: 'Abby', age: 7, hair: 'blond'};
var fred = {name: 'Fred', age: 12, hair: 'brown', grade: 7};
var kids = [abby, fred];
R.project(['name', 'grade'], kids); //=> [{name: 'Abby', grade: undefined}, {name: 'Fred', grade: 7}]
```

## prop(指定属性值)

`s → {s: a} → a | Undefined `

```javascript
R.prop('x', {x: 100}); //=> 100
R.prop('x', {}); //=> undefined
```

## propOr(指定属性值/默认值)

`a → String → Object → a `

```javascript
var alice = {
  name: 'ALICE',
  age: 101
};
var favorite = R.prop('favoriteLibrary');
var favoriteWithDefault = R.propOr('Ramda', 'favoriteLibrary');

favorite(alice);  //=> undefined
favoriteWithDefault(alice);  //=> 'Ramda'
```

## props(指定多个属性值)

`[k] → {k: v} → [v] `

```javascript
R.props(['x', 'y'], {x: 1, y: 2}); //=> [1, 2]
R.props(['c', 'a', 'b'], {b: 2, a: 1}); //=> [undefined, 1, 2]

var fullName = R.compose(R.join(' '), R.props(['first', 'last']));
fullName({last: 'Bullet-Tooth', age: 33, first: 'Tony'}); //=> 'Tony Bullet-Tooth'
```

## toPairs(对象[自身属性]转数组)

`{String: *} → [[String,*]] `

```javascript
R.toPairs({a: 1, b: 2, c: 3}); //=> [['a', 1], ['b', 2], ['c', 3]]
```

## toPairsIn(对象[自身属性或继承属性]转数组)

`{String: *} → [[String,*]] `

```javascript
var F = function() { this.x = 'X'; };
F.prototype.y = 'Y';
var f = new F();
R.toPairsIn(f); //=> [['x','X'], ['y','Y']]
```

## where(判断属性值是否符合条件)

`{String: (* → Boolean)} → {String: *} → Boolean `

```javascript
var pred = R.where({
  a: R.equals('foo'),
  b: R.complement(R.equals('bar')),
  x: R.gt(R.__, 10),
  y: R.lt(R.__, 20)
});

pred({a: 'foo', b: 'xxx', x: 11, y: 19}); //=> true
pred({a: 'xxx', b: 'xxx', x: 11, y: 19}); //=> false
pred({a: 'foo', b: 'bar', x: 11, y: 19}); //=> false
pred({a: 'foo', b: 'xxx', x: 10, y: 19}); //=> false
pred({a: 'foo', b: 'xxx', x: 11, y: 20}); //=> false
```

## whereEq(判断属性值是否等于指定值)

`{String: *} → {String: *} → Boolean `

```javascript
var pred = R.whereEq({a: 1, b: 2});

pred({a: 1});              //=> false
pred({a: 1, b: 2});        //=> true
pred({a: 1, b: 2, c: 3});  //=> true
pred({a: 1, b: 1});        //=> false
```

