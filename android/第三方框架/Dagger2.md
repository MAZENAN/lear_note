- [1.为什么要使用Dagger2](#why)
- [2.Dagger2的基本使用](#base)

# <a id="why">1.为什么要使用Dagger2</a>
# <a id="base">2.Dagger2的基本使用</a>

    添加依赖
    api 'com.google.dagger:dagger:2.26'  
    implementation 'com.google.dagger:dagger-android-support:2.26'  
    annotationProcessor 'com.google.dagger:dagger-compiler:2.26'  
必不可少的元素有三种
：Module,Component,Container  

![Dagger2](https://github.com/MAZENAN/lear_note/blob/master/android/第三方框架/img/dagger2.png)  

## 使用
__@inject__  

    通常在需要依赖的地方使用这个注解。换句话说，你用它告诉Dagger这个类或者字段需要依赖注入，这样，Dagger就会构造一个这个类的实例并满足他们的依赖。  
__@Module：__  

    Modules类里面的方法专门提供依赖,所以我们定义一个类，用@Module注解，这样Dagger在构造类的实例的时候，就知道从哪里去找需要的依赖。moudules的一个重要特征是他们设计为分区并组合在一起（比如说，我们的app中可以有多个组成在一起的moudules）  

__@Provide:__  

    在modules中，我们定义的方法是用这个注解，以此来告诉Dagger我们想要构造并提供这些依赖。  

__@Component：__  

    Component从根本上来说就是一个注入器，也可以说是@inject和@Module的桥梁，它的主要作用就是连接这两个部分。Component可以提供所有定义了的类型的实例，比如：我们必须用@Component注解一个接口然后列出所有的

# 3.模块化实现
# 4.创建和区分不同实例
# 5.Singleton单例讲解
# 6.自定义Scope
# 7.SubCompent和Laze与Provider
