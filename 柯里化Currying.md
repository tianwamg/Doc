#### 柯里化(Currying)

      在计算机科学中，柯里化（Currying）是把接受多个参数的函数变换成接受一个单一参数(最初函数的第一个参数)的函数，并且返回接受余下的参数且返回结果的新函数的技术。这个技术由 Christopher Strachey 以逻辑学家 Haskell Curry 命名的，尽管它是 Moses Schnfinkel 和 Gottlob Frege 发明的。––摘自[百度百科](https://baike.baidu.com/item/%E6%9F%AF%E9%87%8C%E5%8C%96/10350525?fr=aladdin)

   
##### 简言之，Currying是将多个参数聚合成一个参数，并返回结果。

#        

* 举例：
```
    Before:
    function add(x,y){
        return x+y;    
    }   
    After Currying
    function add(x){
        return function(y){
            return x+y;
        }
    }
```
        数学公式：F(x) = f(g(x))

* Java8函数式编程下的柯里化



    - 方式一
```
    @Test
    public void curryingInt(){
    Function<Integer,Function<Integer,Function<Integer,Integer>>> currying= x -> y -> z ->(x+y)*z;

        Function<Integer,Function<Integer,Integer>> c = x ->y -> x+y;
```
    - 方式二
```
    @Test
    public void curryingInt(){
        IntFunction<IntFunction<IntUnaryOperator>> f = x ->y -> z->(x+y)*z;
        System.out.println(f.apply(5).apply(6).applyAsInt(7));
        IntFunction<IntUnaryOperator> f1 = x ->y ->x+y;
        System.out.println(f1.apply(5).applyAsInt(6));
    }
```

* 参考
1. [什么是柯里化](https://blog.csdn.net/Crazypokerk_/article/details/97674338)
2. [Java8柯里化](https://www.jianshu.com/p/c623b8b2aec8)

