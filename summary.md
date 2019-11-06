# 一.目录

[一、目录](#一、目录)

[二、创建和销毁对象](#二、创建和销毁对象)

- [1.考虑用静态工厂方法代替构造器](#1.考虑用静态工厂方法代替构造器)
- [2.遇到多个构造器参数时要考虑用构建器](#2.遇到多个构造器参数时要考虑用构建器)

[三、对于所有对象都通用的方法](#三、对于所有对象都通用的方法)

[四、类和接口](#四、类和接口)

[五、泛型](#五、泛型)

[六、枚举和注解](六、枚举和注解)

[七、方法](#七、方法)

[八、通用程序设计](#八、通用程序设计)

[九、异常](#九、异常)

[十、并发](#十、并发)

[十一、序列化](#十一、序列化)

<br/>

# 二、创建和销毁对象

## 1.考虑用静态工厂方法代替构造器

***优点***

静态工厂方法与构造器不同的优势在于：

- 它们有名称
- 不必在每次调用它们的时候都创建一个新对象
- 它们可以返回原返回类型的任何子类型的对象

<br/>

***缺点***

但是, 静态工厂方法也有缺点, 主要在于:
* 类如果不含公有的或者受保护的构造器, 就不能被子类化.(鼓励使用复合, 而不是继承)
* 它们与其它的静态方法实际上没有任何区别.  
(静态工厂方法的一些惯用名称: valueOf, of, getInstance, newInstance, getType 以及 newType)

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

<br/>

## 2.遇到多个构造器参数时要考虑用构建器

如果类的构造器或者静态工厂方法中具有多个参数，设计这种类时，Builder模式就是种不错的选择。

Builder模式模拟了具名的可选参数，就像Ada和Python中的一样

```java
public static NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        // Required paramters
        private final int servingSize;
        private final int servings;

        // Optional paramters - initialized to default values
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(int val) {
            calories = val;
            return this;
        }

        public Builder fat(int val) {
            fat = val;
            return this;
        }

        public Builder sodium(int val) {
            sodium = val;
            return this;
        }

        public Builder carbohydrate(int val) {
            carbohydrate = val;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }
}
```

***调用builder***

```java
NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
    .calories(100).sodium(35).carbohydrate(27).build();
```

<br/>