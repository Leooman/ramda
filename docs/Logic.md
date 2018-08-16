# Logic

## allPass(判断是否所有规则都通过)

`[(*… → Boolean)] → (*… → Boolean) `

```javascript
var isQueen = R.propEq('rank', 'Q');
var isSpade = R.propEq('suit', '♠︎');
var isQueenOfSpades = R.allPass([isQueen, isSpade]);

isQueenOfSpades({rank: 'Q', suit: '♣︎'}); //=> false
isQueenOfSpades({rank: 'Q', suit: '♠︎'}); //=> true
```

## anyPass(判断是否有一个规则通过)

`[(*… → Boolean)] → (*… → Boolean) `

```javascript
var isClub = R.propEq('suit', '♣');
var isSpade = R.propEq('suit', '♠');
var isBlackCard = R.anyPass([isClub, isSpade]);

isBlackCard({rank: '10', suit: '♣'}); //=> true
isBlackCard({rank: 'Q', suit: '♠'}); //=> true
isBlackCard({rank: 'Q', suit: '♦'}); //=> false
```

## and(判断两个值是否都为真)

`a → b → a | b `

```javascript
R.and(true, true); //=> true
R.and(true, false); //=> false
R.and(false, true); //=> false
R.and(false, false); //=> false
```

## or(判断两个值是否有一个为真)

`a → b → a | b `

```javascript
R.or(true, true); //=> true
R.or(true, false); //=> true
R.or(false, true); //=> true
R.or(false, false); //=> false
```

## both(判断两个规则是否都通过)

`(*… → Boolean) → (*… → Boolean) → (*… → Boolean) `

```javascript
var gt10 = R.gt(R.__, 10)
var lt20 = R.lt(R.__, 20)
var f = R.both(gt10, lt20);
f(15); //=> true
f(30); //=> false
```

## complement(真假取反)

`(*… → *) → (*… → Boolean) `

```javascript
//传入一个函数f，返回一个函数g
//如果f返回true，g返回false，反则反之
var isNotNil = R.complement(R.isNil);
isNil(null); //=> true
isNotNil(null); //=> false
isNil(7); //=> false
isNotNil(7); //=> true
```

## not(真假取反)

`* → Boolean `

```javascript
R.not(true); //=> false
R.not(false); //=> true
R.not(0); //=> true
R.not(1); //=> false
```

## cond(不同情况,类似switch/case)

`[[(*… → Boolean),(*… → *)]] → (*… → *) `

```javascript
var fn = R.cond([
  [R.equals(0),   R.always('water freezes at 0°C')],
  [R.equals(100), R.always('water boils at 100°C')],
  [R.T,           temp => 'nothing special happens at ' + temp + '°C']
]);
fn(0); //=> 'water freezes at 0°C'
fn(50); //=> 'nothing special happens at 50°C'
fn(100); //=> 'water boils at 100°C'
```

## defaultTo(默认取值)

`a → b → a | b `

```javascript
//第一个参数为默认值
//第二个参数val会替代默认值，除非val是null, undefined or NaN
var defaultTo42 = R.defaultTo(42);

defaultTo42(null);  //=> 42
defaultTo42(undefined);  //=> 42
defaultTo42('Ramda');  //=> 'Ramda'
// parseInt('string') results in NaN
defaultTo42(parseInt('string')); //=> 42
```

## either(判断两个规则是否有一个通过)

`(*… → Boolean) → (*… → Boolean) → (*… → Boolean) `

```javascript
var gt10 = x => x > 10;
var even = x => x % 2 === 0;
var f = R.either(gt10, even);
f(101); //=> true
f(8); //=> true
```

## ifElse(条件判断)

`(*… → Boolean) → (*… → *) → (*… → *) → (*… → *) `

```javascript
var incCount = R.ifElse(
  R.has('count'),
  R.over(R.lensProp('count'), R.inc),
  R.assoc('count', 1)
);
incCount({});           //=> { count: 1 }
incCount({ count: 1 }); //=> { count: 2 
```

## isEmpty(判断是否为空)

`a → Boolean `

```javascript
R.isEmpty([1, 2, 3]);   //=> false
R.isEmpty([]);          //=> true
R.isEmpty('');          //=> true
R.isEmpty(null);        //=> false
R.isEmpty({});          //=> true
R.isEmpty({length: 0}); //=> false
```

## pathSatisfies(路径取值是否通过规则)

`(a → Boolean) → [Idx] → {a} → Boolean ` `Idx = String | Int `

```javascript
//第一个参数为判断规则
//第二个参数为路径
//第三个参数为目标对象
R.pathSatisfies(y => y > 0, ['x', 'y'], {x: {y: 2}}); //=> true
```

## propSatisfies(属性取值是否通过规则)

`(a → Boolean) → String → {String: a} → Boolean `

```javascript
//第一个参数为判断规则
//第二个参数为属性名称
//第三个参数为目标对象
R.propSatisfies(x => x > 0, 'x', {x: 1, y: 2}); //=> true
```

## unless(条件判断,类似if/else)

`(a → Boolean) → (a → a) → a → a `

```javascript
//第一个参数为判定函数，为true时返回值
//第二个参数为判定函数为false时执行的函数
//第三个参数为测试值
let safeInc = R.unless(R.isNil, R.inc);
safeInc(null); //=> null
safeInc(1); //=> 2
```

## until(条件判断,类似while)

`(a → Boolean) → (a → a) → a → a `

```javascript
//第一个参数为循环停止的条件
//第二个参数为循环执行函数
//第三个参数为初始值
R.until(R.gt(R.__, 100), R.multiply(2))(1) // => 128
```

## when(条件判断,类似if/else)

`(a → Boolean) → (a → a) → a → a `

```javascript
//第一个参数为判定函数
//第二个参数为判定函数为true时执行的函数
//第三个参数为测试值
var truncate = R.when(
  R.propSatisfies(R.gt(R.__, 10), 'length'),
  R.pipe(R.take(10), R.append('…'), R.join(''))
);
truncate('12345');         //=> '12345'
truncate('0123456789ABC'); //=> '0123456789…'
```

