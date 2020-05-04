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

使用步骤：  

1.新建`Module`类  

     @Module
     public class MainMoudle {  
     Context context;  

     public MainMoudle (Context context) {  
        this.context = context;  
     }   

     @Provides  
     public Person getPerson(Leg name) {  
         return new Person(name);  
       }  
     }  
    
注意：当对象有参数时：  
方式1：在Moudule中提供`@Provides`   
方式2：在对象的构造方法中使用`@Inject`注解


2.新建`Component`接口


    @Component(modules = MainMoudle.class)
    public interface MainComponent {
        void inject(MainActivity activity);
    }


3.注入到Container中  

 注意：注入方式1：module构造方法无参数时：  

    DaggerMainComponent.create().inject(this);  
    
 注入方式2：module构造方法有参数时：  
  
    DaggerMainComponent.builder().mainMoudle(new MainMoudle(this)).build().inject(this);
4.使用   
 
    @Inject  
    Person


# 3.模块化实现
多module的使用  
使用方式：  
1.在module中使用includes  

    @Module(includes = {HttpModule.class})  
2.在compoent中modules为多个    

     @Component(modules = {UserModule.class, HttpModule.class})    

3.定义单独的`compoent`再在compoent中使用`dependencies`  
    
    第一步：
    @Module
    public class HttpModule {
        @Provides
        public Http provideHttp() {
            Log.e("user", "http======");
            return new Http();
        }
    }   
    第二步：  
	@Component(modules = {HttpModule.class})
	public interface HttpCompoent {
	    Http getHttp();
	}
    第三步：
	
	@Component(modules = UserModule.class, dependencies = HttpCompoent.class)
	public interface UserCompoent {
	
	    void inject(LoginActivity activity);
	}  

    第四步
    注入写法
    DaggerUserCompoent.builder().httpCompoent(DaggerHttpCompoent.create()).userModule(new UserModule()).build().inject(this);



# 4.创建和区分不同实例  

使用注解  

方式1：使用`@Named("")`  

 第一步：  

	@Module
	public class UserModule {
	    @Named("dev")
	    @Provides
	    public User provideUserDev() {
	        Log.e("user", "user======dev");
	        return new User();
	    }
	
	    @Named("re")
	    @Provides
	    public User provideUserRe() {
	        Log.e("user", "user======release");
	        return new User();
	    }
	}


  第二步：  

    @Named("dev")
    @Inject
    User userDev;
    @Named("re")
    @Inject
    User userRe;  

方式二：自定义注解  

第一步：定义注解  

	@Qualifier
	@Documented
	@Retention(RUNTIME)
	public @interface Dev {
	}  

	@Qualifier
	@Documented
	@Retention(RUNTIME)
	public @interface Release {
	}

第二步：在module中使用    

	@Module
	public class UserModule {
	    @Dev
	    @Provides
	    public User provideUserDev() {
	        Log.e("user", "user======dev");
	        return new User();
	    }
	
	    @Release
	    @Provides
	    public User provideUserRe() {
	        Log.e("user", "user======release");
	        return new User();
	    }
	}


第三步：在inject时指定  

    @Dev
    @Inject
    User userDev;
    @Release
    @Inject
    User userRe;

__总结:__  

- @Qualifier:要作用是用来区分不同对象实例  
- @Named其实是@Qualifier的一种实现  



# 5.Singleton单例讲解
注解：`@Singleton`  

1.使用方法  
在module的方法中使用注解，在compoent中使用注解。  

	...  

	 @Singleton
	    @Provides
	    public Leg provideLeg() {
	        return new Leg();
	    }
	...

....  

	@Singleton
	@Component(modules = UserModule.class, dependencies = HttpCompoent.class)
	public interface UserCompoent {
	
	    void inject(LoginActivity activity);
	}  


2.注意事项  
这里的singleton不是单例设计模式中的单例，实在compoent作用范围中的单例。  


# 6.自定义Scope  

注意事项  

![注意事项](https://github.com/MAZENAN/lear_note/blob/master/android/第三方框架/img/zhuyi.png)   


![scop](https://github.com/MAZENAN/lear_note/blob/master/android/第三方框架/img/scop.png)   

自定义scop注解  

	@Scope
	@Documented
	@Retention(RUNTIME)
	public @interface ActivityScop {
	}


# 7.SubCompent和Laze与Provider  


