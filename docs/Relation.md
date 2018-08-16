# Relation

## clamp(限定取值范围)

`Ord a => a → a → a → a `

```javascript
//第一个参数为最小限定值minimum
//第二个参数为最大限定值maximum
//第三个参数为value
//返回值 ：value < minimum 时返回 minimum ,value > maximum 时返回 maximum , 否则返回value
//也适用于其他具有排序属性的值，如字符串和日期
R.clamp(1, 10, -5) // => 1
R.clamp(1, 10, 15) // => 10
R.clamp(1, 10, 4)  // => 4
```

## countBy(列表元素计数)

`(a → String) → [a] → {*} `

```javascript
var numbers = [1.0, 1.1, 1.2, 2.0, 3.0, 2.2];
R.countBy(Math.floor)(numbers);    //=> {'1': 3, '2': 2, '3': 1}

var letters = ['a', 'b', 'A', 'a', 'B', 'c'];
R.countBy(R.toLower)(letters);   //=> {'a': 3, 'b': 2, 'c': 1}
```

## difference(列表差集)

`[*] → [*] → [*] `

```javascript
//返回第一个列表存在而且第二个列表不存在的值集合
//对象和数组对比时只判断值相等而不是引用相等
R.difference([1,2,3,4], [7,6,5,4,3]); //=> [1,2]
R.difference([7,6,5,4,3], [1,2,3,4]); //=> [7,6,5]
R.difference([{a: 1}, {b: 2}], [{a: 1}, {c: 3}]) //=> [{b: 2}]
```

## differenceWith(自定义规则获取列表差集)

`((a, a) → Boolean) → [a] → [a] → [a] `

```javascript
//返回第一个列表存在而且第二个列表不存在的值集合
var cmp = (x, y) => x.a === y.a;
var l1 = [{a: 1}, {a: 2}, {a: 3}];
var l2 = [{a: 3}, {a: 4}];
R.differenceWith(cmp, l1, l2); //=> [{a: 1}, {a: 2}]
```

## eqBy(自定义值处理判断是否相等)

`(a → b) → a → a → Boolean `

```javascript
R.eqBy(Math.abs, 5, -5); //=> true
```

## equals(判断两个值是否值相等)

`a → b → Boolean `

```javascript
//对象和数组对比时只判断值相等而不是引用相等
R.equals(1, 1); //=> true
R.equals(1, '1'); //=> false
R.equals([1, 2, 3], [1, 2, 3]); //=> true

var a = {}; a.v = a;
var b = {}; b.v = b;
R.equals(a, b); //=> true
```

## identical(判断两个值是否相等)

`a → a → Boolean `

```javascript
//如果引用相同内存地址，则两个值相等
var o = {};
R.identical(o, o); //=> true
R.identical(1, 1); //=> true
R.identical(1, '1'); //=> false
R.identical([], []); //=> false
//0和-0不相等，NaN和NaN相等
R.identical(0, -0); //=> false
R.identical(NaN, NaN); //=> true
```

## gt(大于判断)

`Ord a => a → a → Boolean `

```javascript
R.gt(2, 1); //=> true
R.gt(2, 2); //=> false
R.gt(2, 3); //=> false
R.gt('a', 'z'); //=> false
R.gt('z', 'a'); //=> true
```

## gte(大于等于判断)

`Ord a => a → a → Boolean `

```javascript
R.gte(2, 1); //=> true
R.gte(2, 2); //=> true
R.gte(2, 3); //=> false
R.gte('a', 'z'); //=> false
R.gte('z', 'a'); //=> true
```

## lt(小于判断)

`Ord a => a → a → Boolean `

```javascript
R.lt(2, 1); //=> false
R.lt(2, 2); //=> false
R.lt(2, 3); //=> true
R.lt('a', 'z'); //=> true
R.lt('z', 'a'); //=> false
```

## lte(小于等于判断)

`Ord a => a → a → Boolean `

```javascript
R.lte(2, 1); //=> false
R.lte(2, 2); //=> true
R.lte(2, 3); //=> true
R.lte('a', 'z'); //=> true
R.lte('z', 'a'); //=> false
```

## innerJoin(内联匹配)

`((a, b) → Boolean) → [a] → [b] → [a] `

```javascript
//根据自定义函数获取第一个列表中符合条件的值集合
//按第一个列表中顺序返回，不删除重复值
R.innerJoin(
  (record, id) => record.id === id,
  [{id: 824, name: 'Richie Furay'},
   {id: 956, name: 'Dewey Martin'},
   {id: 313, name: 'Bruce Palmer'},
   {id: 456, name: 'Stephen Stills'},
   {id: 456, name: 'Stephen Stills'},
   {id: 177, name: 'Neil Young'}],
  [177, 456, 999]
);
//=> [{id: 456, name: 'Stephen Stills'}, {id: 456, name: 'Stephen Stills'}, {id: 177, name: 'Neil Young'}]
```

## intersection(列表交集)

`[*] → [*] → [*] `

```javascript
R.intersection([1,2,3,4], [7,6,5,4,3]); //=> [3,4]
```

## max(两数较大值)

`Ord a => a → a → a `

```javascript
R.max(789, 123); //=> 789
R.max('a', 'b'); //=> 'b'
```

## maxBy(根据自定义规则判断何值的返回值最大)

`Ord b => (a → b) → a → a → a `

```javascript
//  根据自定义函数判断何值的返回值最大
var square = n => n * n;

R.maxBy(square, -3, 2); //=> -3

R.reduce(R.maxBy(square), 0, [3, -5, 4, 1, -2]); //=> -5
R.reduce(R.maxBy(square), 0, []); //=> 0
```

## min(两数较小值)

`Ord a => a → a → a `

```javascript
R.min(789, 123); //=> 123
R.min('a', 'b'); //=> 'a'
```

## minBy(根据自定义规则判断何值的返回值最小)

`Ord b => (a → b) → a → a → a `

```javascript
//  根据自定义函数判断何值的返回值最小
var square = n => n * n;

R.minBy(square, -3, 2); //=> 2

R.reduce(R.minBy(square), Infinity, [3, -5, 4, 1, -2]); //=> 1
R.reduce(R.minBy(square), Infinity, []); //=> Infinity
```

## pathEq(路径匹配)

`[Idx] → a → {a} → Boolean ` `Idx = String | Int `

```javascript
//返回指定路径的值符合条件的成员
var user1 = { address: { zipCode: 90210 } };
var user2 = { address: { zipCode: 55555 } };
var user3 = { name: 'Bob' };
var users = [ user1, user2, user3 ];
var isFamous = R.pathEq(['address', 'zipCode'], 90210);
R.filter(isFamous, users); //=> [ user1 ]
```

## propEq(属性值匹配)

`String → a → Object → Boolean `

```javascript
//返回指定属性的值符合条件的成员
var abby = {name: 'Abby', age: 7, hair: 'blond'};
var fred = {name: 'Fred', age: 12, hair: 'brown'};
var rusty = {name: 'Rusty', age: 10, hair: 'brown'};
var alois = {name: 'Alois', age: 15, disposition: 'surly'};
var kids = [abby, fred, rusty, alois];
var hasBrownHair = R.propEq('hair', 'brown');
R.filter(hasBrownHair, kids); //=> [fred, rusty]
```

## sortBy(自定义规则排序)

`Ord b => (a → b) → [a] → [a] `

```javascript
var sortByFirstItem = R.sortBy(R.prop(0));
var sortByNameCaseInsensitive = R.sortBy(R.compose(R.toLower, R.prop('name')));
var pairs = [[-1, 1], [-2, 2], [-3, 3]];
sortByFirstItem(pairs); //=> [[-3, 3], [-2, 2], [-1, 1]]
var alice = {
  name: 'ALICE',
  age: 101
};
var bob = {
  name: 'Bob',
  age: -10
};
var clara = {
  name: 'clara',
  age: 314.159
};
var people = [clara, bob, alice];
sortByNameCaseInsensitive(people); //=> [alice, bob, clara]
```

## sortWith(自定义组规则排序)

`[(a, a) → Number] → [a] → [a] `

```javascript
var alice = {
  name: 'alice',
  age: 40
};
var bob = {
  name: 'bob',
  age: 30
};
var clara = {
  name: 'clara',
  age: 40
};
var people = [clara, bob, alice];
var ageNameSort = R.sortWith([
  R.descend(R.prop('age')),//后执行
  R.ascend(R.prop('name'))//先执行
]);
ageNameSort(people); //=> [alice, clara, bob]
```

## symmetricDifference(列表非交集)

`[*] → [*] → [*] `

```javascript
R.symmetricDifference([1,2,3,4], [7,6,5,4,3]); //=> [1,2,7,6,5]
R.symmetricDifference([7,6,5,4,3], [1,2,3,4]); //=> [7,6,5,1,2]
```

## symmetricDifferenceWith(自定义规则获取列表非交集)

`((a, a) → Boolean) → [a] → [a] → [a] `

```javascript
var eqA = R.eqBy(R.prop('a'));
var l1 = [{a: 1}, {a: 2}, {a: 3}, {a: 4}];
var l2 = [{a: 3}, {a: 4}, {a: 5}, {a: 6}];
R.symmetricDifferenceWith(eqA, l1, l2); //=> [{a: 1}, {a: 2}, {a: 5}, {a: 6}]
```

## union(列表去重合并)

`[*] → [*] → [*] `

```javascript
R.union([1, 2, 3, 2], [2, 3, 4]); //=> [1, 2, 3, 4]
R.union([1, 2, 3, 2], []); //=> [1, 2, 3]
```

## unionWith(列表自定义规则合并)

`((a, a) → Boolean) → [*] → [*] → [*] `

```javascript
var l1 = [{a: 1}, {a: 2}];
var l2 = [{a: 1}, {a: 4}];
R.unionWith(R.eqBy(R.prop('a')), l1, l2); //=> [{a: 1}, {a: 2}, {a: 4}]
```

