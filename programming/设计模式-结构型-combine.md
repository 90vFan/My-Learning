## 门面模式

> Provide a unified interface to a set of interfaces in a subsystem. Facade Pattern defines a higher-level interface that makes the subsystem easier to use.
>
> 门面模式为子系统提供一组统一的接口，定义一组高层接口让子系统更易用。

从隐藏实现复杂性，提供更易用接口这个意图来看，门面模式有点类似之前讲到的**迪米特法则（最少知识原则）**和**接口隔离原则**：两个有交互的系统，只暴露有限的必要的接口。除此之外，门面模式还有点类似之前提到**封装、抽象**的设计思想，提供更抽象的接口，封装底层实现细节。

### 应用场景

#### 解决易用性问题

Linux 系统调用函数就可以看作一种“门面”。它是 Linux 操作系统暴露给开发者的一组“特殊”的编程接口，它封装了底层更基础的 Linux 内核调用。再比如，Linux 的 Shell 命令，实际上也可以看作一种门面模式的应用。它继续封装系统调用，提供更加友好、简单的命令，让我们可以直接通过执行命令来跟操作系统交互。

#### 解决性能问题

假设，完成某个业务功能（比如显示某个页面信息）需要“依次”调用 a、b、d 三个接口，因自身业务的特点，不支持并发调用这三个接口。我们通过将多个接口调用替换为一个门面接口调用，减少网络通信成本，提高 App 客户端的响应速度。

如果门面接口不多，我们完全可以将它跟非门面接口放到一块，也不需要特殊标记，当作普通接口来用即可。如果门面接口很多，我们可以在已有的接口之上，再重新抽象出一层，专门放置门面接口，从类、包的命名上跟原来的接口层做区分。如果门面接口特别多，并且很多都是跨多个子系统的，我们可以将门面接口放到一个新的子系统中。

#### 解决分布式事务问题

用户注册需要支持事务，也就是说，创建用户和钱包的两个操作，要么都成功，要么都失败，不能一个成功、一个失败。

在一个事务中，执行创建用户和创建钱包这两个 SQL 操作。这就要求两个 SQL 操作要在一个接口中完成，所以，我们可以借鉴门面模式的思想，再设计一个包裹这两个操作的新接口，让新接口在一个事务中执行两个 SQL 操作。

### 接口设计原则

类、模块、系统之间的“通信”，一般都是通过接口调用来完成的。接口设计的好坏，直接影响到类、模块、系统是否好用。所以，我们要多花点心思在接口设计上。我经常说，完成接口设计，就相当于完成了一半的开发任务。只要接口设计得好，那代码就差不到哪里去。接口粒度设计得太大，太小都不好。太大会导致接口不可复用，太小会导致接口不易用。

在实际的开发中，接口的可复用性和易用性需要“微妙”的权衡。针对这个问题，我的一个基本的处理原则是，尽量保持接口的可复用性，但针对特殊情况，允许提供冗余的门面接口，来提供更易用的接口。

## 组合模式

主要是用来处理树形结构数据

正因为其应用场景的特殊性，数据必须能表示成树形结构，这也导致了这种模式在实际的项目开发中并不那么常用。但是，一旦数据满足树形结构，应用这种模式就能发挥很大的作用，能让代码变得非常简洁。

> Compose objects into tree structure to represent part-whole hierarchies.Composite lets client treat individual objects and compositions of objects uniformly.
>
> 将一组对象组织（Compose）成树形结构，以表示一种“部分 - 整体”的层次结构。组合让客户端（在很多设计模式书籍中，“客户端”代指代码的使用者。）可以统一单个对象和组合对象的处理逻辑。

### 实例

需求：设计一个类来表示文件系统中的目录，能方便地实现下面这些功能：

- 动态地添加、删除某个目录下的子目录或文件；
- 统计指定目录下的文件个数；
- 统计指定目录下的文件总大小

``` java

public abstract class FileSystemNode {
  protected String path;

  public FileSystemNode(String path) {
    this.path = path;
  }

  public abstract int countNumOfFiles();
  public abstract long countSizeOfFiles();

  public String getPath() {
    return path;
  }
}

public class File extends FileSystemNode {
  public File(String path) {
    super(path);
  }

  @Override
  public int countNumOfFiles() {
    return 1;
  }

  @Override
  public long countSizeOfFiles() {
    java.io.File file = new java.io.File(path);
    if (!file.exists()) return 0;
    return file.length();
  }
}

public class Directory extends FileSystemNode {
  private List<FileSystemNode> subNodes = new ArrayList<>();

  public Directory(String path) {
    super(path);
  }

  @Override
  public int countNumOfFiles() {
    int numOfFiles = 0;
    for (FileSystemNode fileOrDir : subNodes) {
      numOfFiles += fileOrDir.countNumOfFiles();
    }
    return numOfFiles;
  }

  @Override
  public long countSizeOfFiles() {
    long sizeofFiles = 0;
    for (FileSystemNode fileOrDir : subNodes) {
      sizeofFiles += fileOrDir.countSizeOfFiles();
    }
    return sizeofFiles;
  }

  public void addSubNode(FileSystemNode fileOrDir) {
    subNodes.add(fileOrDir);
  }

  public void removeSubNode(FileSystemNode fileOrDir) {
    int size = subNodes.size();
    int i = 0;
    for (; i < size; ++i) {
      if (subNodes.get(i).getPath().equalsIgnoreCase(fileOrDir.getPath())) {
        break;
      }
    }
    if (i < size) {
      subNodes.remove(i);
    }
  }
}
```

我们对照着这个例子，再重新看一下组合模式的定义：“将一组对象（文件和目录）组织成树形结构，以表示一种‘部分 - 整体’的层次结构（目录与子目录的嵌套结构）。**组合模式让客户端可以统一单个对象（文件）和组合对象（目录）的处理逻辑（递归遍历）**。”

实际上，刚才讲的这种组合模式的设计思路，与其说是一种设计模式，倒不如说是对业务场景的一种数据结构和算法的抽象。其中，数据可以表示成树这种数据结构，业务需求可以通过在树上的递归遍历算法来实现。

## 享元模式

Flyweight Design Pattern

享元模式的意图是复用对象，节省内存，前提是享元对象是不可变对象。

具体来讲，当一个系统中存在大量重复对象的时候，如果这些重复的对象是不可变对象，我们就可以利用享元模式将对象设计成享元，在内存中只保留一份实例，供多处代码引用。这样可以减少内存中对象的数量，起到节省内存的目的。



