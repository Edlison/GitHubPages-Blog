# Java中继承与实现的思考

继承(extend): 继承传达的意思是is-a, 比如Cat is an Animal.

实现(Implement): 实现传达的意思是able一种能力, 比如Cat is swimmable.

## extend
比较灵活可以继承抽象类，也可以继承实体类。
抽象类可以定义非抽象方法与抽象方法，如果有抽象方法则子类必须继承，如果子类没有重写非抽象方法则子类的实例使用的是父类的方法。

**多态**
如果实例化一个子类赋给一个父类(例: Animal cat = new Cat())， 则只能够使用父类中的field与method，如果method被子类重写则会调用子类的method。



## implement
比较严格，若实现则必须实现所有方法。
其中的field都是static final修饰的，且只能被实现的子类获取。method不能有body，但是如果static修饰则可以有body(怎么用？)。

**多态**
如果实例化一个子类赋给一个接口(例: Swimmable cat = new Cat())，则只能够使用该接口的field并且无法获得field。
如果一个子类继承了一个实现了该接口的父类则可以重写方法，如果没有重写则(Swimmable cat = new Cat())中cat获取的方法则是父类中实现的方法。