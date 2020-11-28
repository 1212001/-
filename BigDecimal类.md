## BigDecimal类

### 1、BigDecimal构造方法

1. public BigDecimal(double val)   将double表示形式转换为BigDecimal ***不建议使用**

2. public BigDecimal(int val)　　将int表示形式转换成BigDecimal

3. public BigDecimal(String val)　　将String表示形式转换成BigDecimal

   例：

   ```java
   public static void main(String[] args)
       {
           BigDecimal bigDecimal = new BigDecimal(2);
           BigDecimal bDouble = new BigDecimal(2.3);
           BigDecimal bString = new BigDecimal("2.3");
           System.out.println("bigDecimal=" + bigDecimal);
           System.out.println("bDouble=" + bDouble);
           System.out.println("bString=" + bString);
       }
   ```

结果：

```
bigDecimal=2
bDouble=2.299999999994845
bString=2.3
```

为什么会出现这种情况呢？

JDK的描述：

1、参数类型为double的构造方法的结果有一定的不可预知性。有人可能认为在Java中写入newBigDecimal(0.1)所创建的BigDecimal正好等于 0.1（非标度值 1，其标度为 1），但是它实际上等于0.1000000000000000055511151231257827021181583404541015625。这是因为0.1无法准确地表示为 double（或者说对于该情况，不能表示为任何有限长度的二进制小数）。这样，传入到构造方法的值不会正好等于 0.1（虽然表面上等于该值）。

2、另一方面，String 构造方法是完全可预知的：写入 newBigDecimal("0.1") 将创建一个 BigDecimal，它正好等于预期的 0.1。因此，比较而言，**通常建议优先使用String构造方法**。

### 2、当`double`必须用作`BigDecimal`的源时

请使用`Double.toString(double)转成String，然后`使用String构造方法，或使用BigDecimal的静态方法valueOf，如下

```java
public static void main(String[] args)
    {
        BigDecimal bDouble1 = BigDecimal.valueOf(2.3);
        BigDecimal bDouble2 = new BigDecimal(Double.toString(2.3));
        System.out.println("bDouble1=" + bDouble1);
        System.out.println("bDouble2=" + bDouble2);
    }
```

### 3、BigDecimal加减乘除运算

```java
public BigDecimal add(BigDecimal value);                        //加法
public BigDecimal subtract(BigDecimal value);                   //减法 
public BigDecimal multiply(BigDecimal value);                   //乘法
public BigDecimal divide(BigDecimal value);                     //除法
```

例：

```java
public static void main(String[] args)
    {
        BigDecimal a = new BigDecimal("4.5");
        BigDecimal b = new BigDecimal("1.5");

        System.out.println("a + b =" + a.add(b));
        System.out.println("a - b =" + a.subtract(b));
        System.out.println("a * b =" + a.multiply(b));
        System.out.println("a / b =" + a.divide(b));
    }
```

### 4、注意除法运算divide.

BigDecimal除法可能出现不能整除的情况，比如 4.5/1.3，这时会报错java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result.

其实divide方法有可以传三个参数

```java
public BigDecimal divide(BigDecimal divisor, int scale, int roundingMode) 
```

第一参数表示除数， 二个参数表示小数点后保留位数，第三个参数表示舍入模式。

只有在作除法运算或四舍五入时才用到舍入模式，有下面这几种：

- ROUND_UP ：向远离零的方向舍入。舍弃非零部分，并将非零舍弃部分相邻的一位数字加一。
- ROUND_DOWN ：向接近零的方向舍入。舍弃非零部分，同时不会非零舍弃部分相邻的一位数字加一，采取截取行为。
- ROUND_CEILING ：向正无穷的方向舍入。如果为正数，舍入结果同ROUND_UP一致；如果为负数，舍入结果同ROUND_DOWN一致。注意：此模式不会减少数值大小。
- ROUND_FLOOR ：向负无穷的方向舍入。如果为正数，舍入结果同ROUND_DOWN一致；如果为负数，舍入结果同ROUND_UP一致。注意：此模式不会增加数值大小。
- ROUND_HALF_UP ：向“最接近”的数字舍入，如果与两个相邻数字的距离相等，则为向上舍入的舍入模式。如果舍弃部分>= 0.5，则舍入行为与ROUND_UP相同；否则舍入行为与ROUND_DOWN相同。这种模式也就是我们常说的我们的“四舍五入”。
- ROUND_HALF_DOWN ：向“最接近”的数字舍入，如果与两个相邻数字的距离相等，则为向下舍入的舍入模式。如果舍弃部分> 0.5，则舍入行为与ROUND_UP相同；否则舍入行为与ROUND_DOWN相同。这种模式也就是我们常说的我们的“五舍六入”。
- ROUND_HALF_EVEN ：向“最接近”的数字舍入，如果与两个相邻数字的距离相等，则相邻的偶数舍入。如果舍弃部分左边的数字奇数，则舍入行为与 ROUND_HALF_UP 相同；如果为偶数，则舍入行为与 ROUND_HALF_DOWN 相同。注意：在重复进行一系列计算时，此舍入模式可以将累加错误减到最小。此舍入模式也称为“银行家舍入法”，主要在美国使用。四舍六入，五分两种情况，如果前一位为奇数，则入位，否则舍去。
- ROUND_UNNECESSARY ：断言请求的操作具有精确的结果，因此不需要舍入。如果对获得精确结果的操作指定此舍入模式，则抛出ArithmeticException。

按照各自的需要，可传入合适的第三个参数。**四舍五入采用 ROUND_HALF_UP**

### 5、四舍五入示例

需要对BigDecimal进行截断和四舍五入可用setScale方法，例：

```java
   public static void main(String[] args)
    {
        BigDecimal a = new BigDecimal("4.5635");

        a = a.setScale(3, RoundingMode.HALF_UP);    //保留3位小数，且四舍五入
        System.out.println(a);
    }
```

减乘除其实最终都返回的是一个新的BigDecimal对象，因为BigInteger与BigDecimal都是不可变的（immutable）的，在进行每一步运算时，都会产生一个新的对象

```java
   public static void main(String[] args)
    {
        BigDecimal a = new BigDecimal("4.5");
        BigDecimal b = new BigDecimal("1.5");
        a.add(b);

        System.out.println(a);  //输出4.5. 加减乘除方法会返回一个新的BigDecimal对象，原来的a不变（注意）


    }
```

### 6、BigDecimal比较大小

```java
BigDecimal a = new BigDecimal (101);
BigDecimal b = new BigDecimal (111);
 
//使用compareTo方法比较
//注意：a、b均不能为null，否则会报空指针
if(a.compareTo(b) == -1){
    System.out.println("a小于b");
}
 
if(a.compareTo(b) == 0){
    System.out.println("a等于b");
}
 
if(a.compareTo(b) == 1){
    System.out.println("a大于b");
}
 
if(a.compareTo(b) > -1){
    System.out.println("a大于等于b");
}
 
if(a.compareTo(b) < 1){
    System.out.println("a小于等于b");
}
```

### 7、BigDecimal转String

```java
public static void main(String[] args) {
        // 浮点数的打印
        System.out.println(new BigDecimal("10000000000").toString());

        // 普通的数字字符串
        System.out.println(new BigDecimal("100.000").toString());

        // 去除末尾多余的0
        System.out.println(new BigDecimal("100.000").stripTrailingZeros().toString());

        // 避免输出科学计数法
        System.out.println(new BigDecimal("100.000").stripTrailingZeros().toPlainString());

}

// output
10000000000
100.000
1E+2
100
```

### 8、总结

1. 商业计算使用BigDecimal。
2. 尽量使用参数类型为String的构造函数。
3. BigDecimal都是不可变的（immutable）的，在进行每一步运算时，都会产生一个新的对象，所以在做加减乘除运算时千万要保存操作后的值。