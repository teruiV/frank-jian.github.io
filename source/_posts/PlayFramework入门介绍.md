---
title: PlayFramework入门介绍
date: 2016-05-16 17:44:10
tags: [Play]
categories: Play
---
###### 什么是PlayFramework?
Play Framework 是一个轻量级，无状态，网络友好的框架，使用Java和Scala来写的开源Web应用程序框架，采用了MVC(Model View  Controller)的体系结构，它意在通过使用协议由于配置，以及在浏览器显示错误来提高开发者的工作效率。
###### PlayFramework和传统的SpringMVC框架相比优势在哪？
- 实现了热加载，就是无需重启JVM就可以加载修改过的类，更新运行时的class行为;
- 内嵌Jetty容器
- 采用MVC结构
- 强调协议优于配置的设计,通过Http路由得到具体的接口，不需要通过XML来配置Action接口；
- 优秀的错误报告功能，当错误出现时，Play Framework会在浏览器上将错误的代码位置，甚至模板给显示出来。

###### Java类的加载
Java类是通过Java虚拟机加载的，某个类的class文件被classloader加载后，会生成对应的Class对象，之后就可以创建该类的实例了。默认的虚拟机行为只会在启动时加载类，如果后期有一个类需要更新的话，单纯替换编译的class文件，Java虚拟机是不会更新正在运行的class。

###### 如何实现热加载？
1. 最直接的方式是修改虚拟机的源代码，改变classloader的加载行为，使虚拟机能监听class文件的更新，重新加载class文件，这样的行为破坏性极大，为后续的JVM升级埋下一个大坑。
2. 实现自己的classLoader，并且创建对象的行为，指定为用自定义的classLoader加载class，playFramework就是这么干的。以下是其源码实现，从源码上看play会遍历所有发生改变的class然后重新加载，因为是放在用户请求的过程中，所以，我们的直观感觉就是刷新页面就热加载了一切，这就是play的hotswap.

###### 热加载具体源码
```
    // Init the call (especially usefull in DEV mode to detect changes)
    public boolean init() {
        Thread.currentThread().setContextClassLoader(Play.classloader);
        Play.detectChanges();
        if (!Play.started) {
            if (Play.mode == Mode.PROD) {
                throw new UnexpectedException("Application is not started");
            }
            Play.start();
        }
        InvocationContext.current.set(getInvocationContext());
        return true;
    }
```
Play.detectChanges()方法实现
```
/**
     * Detect Java changes
     */
    public void detectChanges() {
        // Now check for file modification
        List<ApplicationClass> modifieds = new ArrayList<ApplicationClass>();
        for (ApplicationClass applicationClass : Play.classes.all()) {
            if (applicationClass.timestamp < applicationClass.javaFile.lastModified()) {
                applicationClass.refresh();
                modifieds.add(applicationClass);
            }
        }
        Set<ApplicationClass> modifiedWithDependencies = new HashSet<ApplicationClass>();
        modifiedWithDependencies.addAll(modifieds);
        if (modifieds.size() > 0) {
            modifiedWithDependencies.addAll(Play.pluginCollection.onClassesChange(modifieds));
        }
        List<ClassDefinition> newDefinitions = new ArrayList<ClassDefinition>();
        boolean dirtySig = false;
        for (ApplicationClass applicationClass : modifiedWithDependencies) {
            if (applicationClass.compile() == null) {
                Play.classes.classes.remove(applicationClass.name);
                currentState = new ApplicationClassloaderState();//show others that we have changed..
            } else {
                int sigChecksum = applicationClass.sigChecksum;
                applicationClass.enhance();
                if (sigChecksum != applicationClass.sigChecksum) {
                    dirtySig = true;
                }
                BytecodeCache.cacheBytecode(applicationClass.enhancedByteCode, applicationClass.name, applicationClass.javaSource);
                newDefinitions.add(new ClassDefinition(applicationClass.javaClass, applicationClass.enhancedByteCode));
                currentState = new ApplicationClassloaderState();//show others that we have changed..
            }
        }
        if (newDefinitions.size() > 0) {
            Cache.clear();
            if (HotswapAgent.enabled) {
                try {
                    HotswapAgent.reload(newDefinitions.toArray(new          ClassDefinition[newDefinitions.size()]));
                } catch (Throwable e) {
                    throw new RuntimeException("Need reload");
                }
            } else {
                throw new RuntimeException("Need reload");
            }
        }
        // Check signature (variable name & annotations aware !)
        if (dirtySig) {
            throw new RuntimeException("Signature change !");
        }

        // Now check if there is new classes or removed classes
        int hash = computePathHash();
        if (hash != this.pathHash) {
            // Remove class for deleted files !!
            for (ApplicationClass applicationClass : Play.classes.all()) {
                if (!applicationClass.javaFile.exists()) {
                    Play.classes.classes.remove(applicationClass.name);
                    currentState = new ApplicationClassloaderState();//show others that we have changed..
                }
                if (applicationClass.name.contains("$")) {
                    Play.classes.classes.remove(applicationClass.name);
                    currentState = new ApplicationClassloaderState();//show others that we have changed..
                    // Ok we have to remove all classes from the same file ...
                    VirtualFile vf = applicationClass.javaFile;
                    for (ApplicationClass ac : Play.classes.all()) {
                        if (ac.javaFile.equals(vf)) {
                            Play.classes.classes.remove(ac.name);
                        }
                    }
                }
            }
            throw new RuntimeException("Path has changed");
        }
    }
```

参考文章：
1. [playframework拦截器和热加载 源码浅析](http://itindex.net/detail/47223-playframework-%E6%BA%90%E7%A0%81)
2. [深入探索 Java 热部署](http://www.ibm.com/developerworks/cn/java/j-lo-hotdeploy/index.html?ca=drs-)
3. [打破传统的JAVA框架－PLAY FRAMEWORK](http://www.adaplay.org/index.php/break-the-traditional-java-frameworks-play-framework/)










