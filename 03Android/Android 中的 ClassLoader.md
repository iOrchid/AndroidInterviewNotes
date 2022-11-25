## 1 分类

* 系统类加载器
  * BootClassLoader：**预加载常用类**，由 Java 实现，继承 ClassLoader
  * PathClassLoader：**加载系统类和应用程序的类**，继承 BaseDexClassLoader，没有参数 optimizedDirectory（给了默认 /data/dalvik-cache）无法定义解压的 dex 文件存储路径，常用来加载已经安装 apk 的 dex 文件（/data/dalvik-cache）
  * DexClassLoader：加载 dex 文件以及包含 dex 的压缩文件（apk 和 jar 文件），继承 BaseDexClassLoader，

* 自定义加载器

## 2 其他的类

* ClassLoader：抽象类，定义一些主要功能，BootClassLoader 是它的内部类
* SecureClassLoader：拓展 CLassLoader 类加入权限方面的功能，加强了 ClassLoader 的安全性
* URLClassLoader：继承 SecureClassLoader ，通过 URL路径从 jar 文件和文件夹中加载类和资源
* InMemoryDexClassLoader：加载内存中的 dex 文件
* BaseDexClassLoader：继承自 ClassLoader，是抽象类 ClassLoader 的具体实现类

## 3 ClasLoader 加载过程

和 Java 一样都遵循双亲委托机制。

```java
protected Class<?> loadClass(String name, boolean resolve) 
    throws ClassNotFoundException {
            //检测是否已经加载过,若加载过，则直接返回
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                try {
                    if (parent != null) {
                        //调用父类的loadClass
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                   
                }

                if (c == null) {
                    //还是未找到，使用 findClass 在当前 dex 查找
                    c = findClass(name);
                }
            }
            return c;
    }

  protected Class<?> findClass(String name) throws ClassNotFoundException {
        throw new ClassNotFoundException(name);//需要子类去实现 BaseDexClassLoader
    }
```

BaseDexClassLoader#findClass

```java
  @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        List<Throwable> suppressedExceptions = new ArrayList<Throwable>();
        //pathList类型为 DexPathList 保存 dexfile 文件的句柄等 dex 的操作
        Class c = pathList.findClass(name, suppressedExceptions);
        if (c == null) {
            ...
            throw cnfe;
        }
        return c;
    }
```

DexPathList#findClass

```java
   public Class<?> findClass(String name, List<Throwable> suppressed) {
       //Element 内部封装 DexFile 用于加载 dex
       for (Element element : dexElements) {
           Class<?> clazz = element.findClass(name, definingContext, suppressed);
           if (clazz != null) {
               return clazz;
           }
       }
       ...
       return null;
   }
```

Element#findClass

```java
public Class<?> findClass(String name, ClassLoader definingContext,
        List<Throwable> suppressed) {
    //dexFile.loadClassBinaryName
    return dexFile != null ? dexFile.loadClassBinaryName(name, definingContext, suppressed) : null;
}
```

DexFile#loadClassBinaryName

```java
public Class loadClassBinaryName(String name, ClassLoader loader, List<Throwable> suppressed) {
    return defineClass(name, loader, mCookie, this, suppressed);
}

private static Class defineClass(String name, ClassLoader loader, Object cookie,
                                 DexFile dexFile, List<Throwable> suppressed) {
    Class result = null;
    try {
        //加载 dex 文件
        result = defineClassNative(name, loader, cookie, dexFile);
    } 
    ...
    return result;
}

// 调用 Native 层代码
private static native Class defineClassNative(String name, ClassLoader loader, Object cookie, DexFile dexFile)
```

