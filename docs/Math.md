# Math

## add(两数相加)

`Number → Number → Number`

```javascript
R.add(2, 3);       //=>  5
R.add(7)(10);      //=> 17
```

## subtract(两数相减)

`Number → Number → Number `

```javascript
R.subtract(10, 8); //=> 2

var minus5 = R.subtract(R.__, 5);
minus5(17); //=> 12

var complementaryAngle = R.subtract(90);
complementaryAngle(30); //=> 60
complementaryAngle(72); //=> 18
```

## mutiply(两数相乘)

`Number → Number → Number `

```javascript
var double = R.multiply(2);
var triple = R.multiply(3);
double(3);       //=>  6
triple(4);       //=> 12
R.multiply(2, 5);  //=> 10
```

## divide(两数相除)

`Number → Number → Number `

```javascript
R.divide(71, 100); //=> 0.71

var half = R.divide(R.__, 2);
half(42); //=> 21

var reciprocal = R.divide(1);
reciprocal(4);   //=> 0.25
```

## dec(自减)

`Number → Number `

```javascript
R.dec(42); //=> 41
```

## inc(自增)

`Number → Number `

```javascript
R.inc(42); //=> 43
```

## mean(平均数)

`[Number] → Number `

```javascript
R.mean([2, 7, 9]); //=> 6
R.mean([]); //=> NaN
```

## median(中位数)

`[Number] → Number `

```javascript
R.median([2, 9, 7]); //=> 7
R.median([7, 2, 10, 9]); //=> 8
R.median([]); //=> NaN
```

## negate(负数)

`Number → Number `

```javascript
R.negate(42); //=> -42
```

## product(乘积)

`[Number] → Number `

```javascript
R.product([2,4,6,8,100,1]); //=> 38400
```

## sum(求和)

`[Number] → Number `

```javascript
R.sum([2,4,6,8,100,1]); //=> 121
```

## modulo(取余)

`Number → Number → Number `

```javascript
R.modulo(17, 3); //=> 2
// JS behavior:
R.modulo(-17, 3); //=> -2
R.modulo(-17, -3); //=> -2
R.modulo(17, -3); //=> 2
R.modulo(-17.5, 3); //=> -2.5

var isOdd = R.modulo(R.__, 2);
isOdd(42); //=> 0
isOdd(21); //=> 1
```

## mathMod(取余)

`Number → Number → Number `

```javascript
// 要求参数必须为整数，否则返回NaN
// 除数为0或者负数，返回NaN
R.mathMod(-17, 5);  //=> 3
R.mathMod(17, 5);   //=> 2
R.mathMod(17, -5);  //=> NaN
R.mathMod(17, 0);   //=> NaN
R.mathMod(17.2, 5); //=> NaN
R.mathMod(17, 5.3); //=> NaN

var clock = R.mathMod(R.__, 12);
clock(15); //=> 3
clock(24); //=> 0

var seventeenMod = R.mathMod(17);
seventeenMod(3);  //=> 2
seventeenMod(4);  //=> 1
seventeenMod(10); //=> 7
```

