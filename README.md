# 面向对象的设计模式

设计模式的原则：

- 回顾 solid(单一，开闭，里氏置换，接口分离，依赖倒置)


## 创建型模式
- [工厂模式](#factory)
- [单例模式](#singleton)
- [建造者模式](#builder)
- [原型模式](#prototype-chain)

### <a name="factory"></a>工厂模式

1. 简单工厂模式

    简单工厂模式氛围三种:
   -  普通

        就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建
        示例：`factory/simple.ts`
   -  多个方法

        是对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象，而多个工厂方法模式是提供多个工厂方法，分别创建对象
        示例：`factory/simple.ts`
   -  多个静态方法

        将上面的多个工厂方法模式里的方法置为静态的，不需要创建实例，直接调用即可
        示例：`factory/simple.ts`

2. 工厂方法模式

    简单工厂模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则，所以，从设计角度考虑，有一定的问题，如何解决？就用到工厂方法模式，创建一个工厂接口和创建多个工厂实现类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码。
    示例：`factory/factory-method.ts`


3. 抽象工厂模式

    工厂方法模式引入工厂等级结构，解决了简单工厂模式中工厂类职责过重的问题，但由于工厂方法模式中每个工厂只创建一类具体类的对象，这将会导致系统当中的工厂类过多，这势必会增加系统的开销。此时，我们可以考虑将一些相关的具体类组成一个“具体类族”，由同一个工厂来统一生产，这就是“抽象工厂模式”的基本思想。


### <a name="singleton"></a>单例模式

 单例对象（Singleton）是一种常用的设计模式，在系统运行中保证只有一个实例存在，示例：`singleton/singleton.ts`
1. 饿汉式: 单例实例在类装载时就构建，急切初始化。（预先加载法）
    ```

    class SingletonTest {
        private static instance: SingletonTest = new SingletonTest();

        public static getInstance() {
            return this.instance;
        }

        public static otherMethod() {

        }
    }
    ```
    - 优点: 
        - 线程安全 
        - 在类加载的同时已经创建好一个静态对象，调用时反应速度快 
    - 缺点: 
        -   资源效率不高，可能getInstance()永远不会执行到，但执行该类的其他静态方法或者加载了该类，那么这个实例仍然初始化  

2. 懒汉式: 单例实例在第一次被使用时构建，延迟初始化
    ```
    class SingletonTest1 {
        private static instance: SingletonTest1;

        public static getInstance() {
            if (!this.instance) {
                this.instance = new SingletonTest1();
            }
            return this.instance;
        }

        public static otherMethod() {
            console.log("other operation, new instance");    
        }
    }
    ```
    - 优点:

        避免了饿汉式的那种在没有用到的情况下创建事例，资源利用率高，不执行getInstance()就不会被实例，可以执行该类的其他静态方法。




### <a name="builder"></a>建造者模式

1. 特点: 一步一步来构建一个复杂对象，可以用不同组合或顺序建造出不同意义的对象，通常使用者并不需要知道建造的细节，通常使用链式调用来构建对象
1. 用处：当对象像积木一样灵活，并且需要使用者来自己组装时可以采用此模式，好处是不需要知道细节，调用方法即可，常用来构建如Http请求、生成器等。
1. 注意：和工厂模式的区别，工厂是生产产品，谁生产，怎样生产无所谓，而建造者重在组装产品，层级不一样。

示例： `builder/builder.ts`


### <a name="prototype-chain"></a>原型模式(Prototype)

1. 特点: 不需要知道对象构建的细节，直接从对象上克隆出来。
1. 用处：当对象的构建比较复杂时或者想得到目标对象相同内容的对象时可以考虑原型模式。

示例： `prototype/prototype.ts`

## 结构型模式
- [适配器模式](#adapter)
- [代理模式](#proxy)
- [装饰器模式](#decorator)
- [外观模式](#facade)
- [桥接模式](#bridge)
- [组合模式](#composite)
- [享元模式](#flyweight)

### <a name="adapter"></a>适配器模式
1. 类的适配器模式: 

    当希望将一个类转换成满足另一个新接口的类时，可以使用类的适配器模式，创建一个新类，继承原有的类，实现新的接口即可。

    核心思想就是：有一个Source类，拥有一个方法，待适配，目标接口是Targetable，通过Adapter类，将Source的功能扩展到Targetable里。 
    
    示例 `adapter/adapter.ts`
2. 对象的适配器模式:

    当希望将一个对象转换成满足另一个新接口的对象时，可以创建一个Wrapper类，持有原类的一个实例，在Wrapper类的方法中，调用实例的方法就行

    基本思路和类的适配器模式相同，只是将Adapter类作修改，这次不继承Source类，而是持有Source类的实例，以达到解决兼容性的问题。 
    
    示例 `adapter/adapter.ts`
3. 接口的适配器模式:

    有时我们写的一个接口中有多个抽象方法，当我们写该接口的实现类时，必须实现该接口的所有方法，这明显有时比较浪费，因为并不是所有的方法都是我们需要的，有时只需要某一些，此处为了解决这个问题，我们引入了接口的适配器模式

    借助于一个抽象类，该抽象类实现了该接口，实现了所有的方法，而我们不和原始的接口打交道，只和该抽象类取得联系，所以我们写一个类，继承该抽象类，重写我们需要的方法就行

    
### <a name="proxy"></a>代理模式
代理模式就是多一个代理类出来，替原对象进行一些操作，比如我们在租房子的时候回去找中介，为什么呢？因为你对该地区房屋的信息掌握的不够全面，希望找一个更熟悉的人去帮你做，此处的代理就是这个意思。再如我们有的时候打官司，我们需要请律师，因为律师在法律方面有专长，可以替我们进行操作，表达我们的想法

示例： `proxy/proxy.ts`

### <a name="decorator"></a>装饰器模式
装饰模式就是给一个对象增加一些新的功能，而且是动态的，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例 

示例：`decorator/decorator-1.ts decorator/decorator-2.ts`

### <a name="facade"></a>外观模式

外观模式是为了解决类与类之间的依赖关系的，就是将他们的关系放在一个Facade类中，降低了类类之间的耦合度，该模式中没有涉及到接口

示例： `facade/facade.ts`

    如果没有Computer类，那么，CPU、Memory、Disk他们之间将会相互持有实例，产生关系，这样会造成严重的依赖，修改一个类，可能会带来其他类的修改，这不是我们想要看到的，有了Computer类，他们之间的关系被放在了Computer类里，这样就起到了解耦的作用，这，就是外观模式！

### <a name="bridge"></a>桥接模式
桥接模式就是把事物和其具体实现分开，使他们可以各自独立的变化。

桥接的用意是：将抽象化与实现化解耦，使得二者可以独立变化，像我们常用的JDBC桥DriverManager一样，JDBC进行连接数据库的时候，在各个数据库之间进行切换，基本不需要动太多的代码，甚至丝毫不用动，原因就是JDBC提供统一接口，每个数据库提供各自的实现，用一个叫做数据库驱动的程序来桥接就行了

示例： `bridge/bridge.ts`

### <a name="composite"></a>组合模式(Composite)
组合模式有时又叫部分-整体模式在处理类似树形结构的问题时比较方便

使用场景：将多个对象组合在一起进行操作，常用于表示树形结构中，例如二叉树等。

示例： `composite/composite.ts`
### <a name="flyweight"></a>享元模式(Flyweight)

运用共享物件来尽可能减少不变量的内存消耗。享元模式的主要目的是实现对象的共享，即共享池，当系统中对象多的时候可以减少内存的开销，通常与工厂模式一起使用

    FlyWeightFactory负责创建和管理享元单元，当一个客户端请求时，工厂需要检查当前对象池中是否有符合条件的对象，如果有，就返回已经存在的对象，如果没有，则创建一个新对象，FlyWeight是超类。
示例： `flyweight/flyweight.ts`


## 行为型模式
- [策略模式](#strategy)
- [观察者模式](#observer)
- [模板方法模式](#template-method)
- [责任链模式](#chain-of-responsibility)
- [命令模式](#command)
- [迭代器模式](#iterator)
- [状态模式](#state)
- [备忘录模式](#memento)
- [访问者模式](#visitor)
- [中介者模式](#mediator)
- [解释器模式](#interpreter)

### <a name="strategy"></a>策略模式
策略模式定义了一系列算法，并将每个算法封装起来，使他们可以相互替换，且算法的变化不会影响到使用算法的客户。需要设计一个接口，为一系列实现类提供统一的方法，多个实现类实现该接口，还可以设计一个抽象类（可有可无，属于辅助类），提供辅助函数

示例：`strategy/strategy.ts`

### <a name="observer"></a>观察者模式
观察者模式很好理解，类似于邮件订阅和RSS订阅，当我们浏览一些博客或wiki时，经常会看到RSS图标，就这的意思是，当你订阅了该文章，如果后续有更新，会及时通知你。其实，简单来讲就一句话：当一个对象变化时，其它依赖该对象的对象都会收到通知，并且随着变化！对象之间是一种一对多的关系

示例： `observer/observer.ts`

### <a name="template-method"></a>模板方法模式
模板方法模式，就是指：一个抽象类中，有一个主方法，再定义1...n个方法，可以是抽象的，也可以是实际的方法，定义一个类，继承该抽象类，重写抽象方法，通过调用抽象类，实现对子类的调用

示例：`template-method/template-method.ts`


### <a name="chain-of-responsibility"></a>责任链模式

有多个对象，每个对象持有对下一个对象的引用，这样就会形成一条链，请求在这条链上传递，直到某一对象决定处理该请求。但是发出者并不清楚到底最终那个对象会处理该请求，所以，责任链模式可以实现，在隐瞒客户端的情况下，对系统进行动态的调整

示例：`chain-of-responsibility/chain-of-responsibility.ts`

### <a name="command"></a>命令模式

命令模式是一种封装方法调用的方式，用来将行为请求者和行为实现者解除耦合。

有哪些角色:
- Invoker 调用命令的对象
- Command 具体的命令
- Receiver 真正的命令执行对象

示例：`command/command.ts`

### <a name="iterator"></a>迭代器模式

迭代器是一种行为设计模式， 让你能在不暴露复杂数据结构内部细节的情况下遍历其中所有的元素。

示例：`iterator/iterator.ts`

### <a name="state"></a>状态模式

当对象的状态改变时，同时改变其行为

示例: `state/state.ts`

### <a name="memento"></a>备忘录模式

主要目的是保存一个对象的某个状态，以便在适当的时候恢复对象，通俗的讲下：假设有原始类A，A中有各种属性，A可以决定需要备份的属性，备忘录类B是用来存储A的一些内部状态，类C呢，就是一个用来存储备忘录的，且只能存储，不能修改等操作

### <a name="visitor"></a>访问者模式

访问者模式把数据结构和作用于结构上的操作解耦合，使得操作集合可相对自由地演化。访问者模式适用于数据结构相对稳定算法又易变化的系统。因为访问者模式使得算法操作增加变得容易。若系统数据结构对象易于变化，经常有新的数据对象增加进来，则不适合使用访问者模式。访问者模式的优点是增加操作很容易，因为增加操作意味着增加新的访问者。访问者模式将有关行为集中到一个访问者对象中，其改变不影响系统数据结构。其缺点就是增加新的数据结构很困难。—— From 百科

简单来说，访问者模式就是一种分离对象数据结构与行为的方法，通过这种分离，可达到为一个被访问者动态添加新的操作而无需做其它的修改的效果

示例: `visitor/visitor.ts`

### <a name="mediator"></a>中介者模式

中介者模式也是用来降低类类之间的耦合的，因为如果类类之间有依赖关系的话，不利于功能的拓展和维护，因为只要修改一个对象，其它关联的对象都得进行修改。如果使用中介者模式，只需关心和Mediator类的关系，具体类类之间的关系及调度交给Mediator就行，这有点像spring容器的作用

示例: `mediator/mediator.ts`

### <a name="interpreter"></a>解释器模式

