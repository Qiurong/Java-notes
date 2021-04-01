# Arrays工具类阅读笔记

## 介绍

该类位于`java.util`包下，封装了一些操作数组的常用方法，如**sort**，**search**等等

## 常用方法

在介绍排序方法之前，先介绍一下Java中可以自定义排序规则的两个接口。

#### Comparable 接口

Comparable接口是集合中排序时默认的排序规则实现方法，但其功能比较单一固定。只能根据定义规则来进行**升序排序**。

```java
public interface Comparable<T> {
    //返回一个负数，0，正数用于表示>，==，<关系
    public int compareTo(T o);
}
```

**使用方法：**

```java
object.compareTo(Object anotherObjcet)
```

让需要进行排序的对象/类实现Comparable接口，并且重写其中`CompareTo()`方法。那么就可以直接调用**Collections.sort**或**Arrays.sort**来进行自动排序。

注意一般而言，如果重写了compareTo方法，那么相对得也要重写hashcode()和equals()方法。

下面我们以team class来举例。

```java
class Team implements Comparable<Team>{
    private Character teamName;
    private int teamScore;

    public team() {
    }

    public team(char teamName, int teamScore) {
        this.teamName = teamName;
        this.teamScore = teamScore;
    }

    //先根据分数进行升序排序，如果分数相同的情况下根据名字进行升序排序。
    @Override
    public int compareTo(Team o) {
        if (teamScore != o.teamScore){
            return teamScore - o.teamScore;
        }else {
            //这里使用compareTo需要把teamName改为Character类
            return teamName.compareTo(o.teamName);
        }
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        team team = (team) o;
        return teamScore == team.teamScore && Objects.equals(teamName, team.teamName);
    }

    @Override
    public int hashCode() {
        return Objects.hash(teamName, teamScore);
    }

    @Override
    public String toString() {
        return "team{" +
            "teamName=" + teamName +
            ", teamScore=" + teamScore +
            '}';
    }
}
```

下面我们写一个进行测试。

```java
public class compare_interface_test {

    public static void main(String[] args) {
        Team[] teams = {new Team('c',10), new Team('b',20),new Team('d',5),new Team('a',20)};
        for (int i = 0; i < teams.length;i++) {
            System.out.println(teams[i].toString());
        }
        Arrays.sort(teams);
        for (int i = 0; i < teams.length;i++) {
            System.out.println(teams[i].toString());
        }
    }
}
```

输出结果：

```java
team{teamName=c, teamScore=10}
team{teamName=b, teamScore=20}
team{teamName=d, teamScore=5}
team{teamName=a, teamScore=20}
team{teamName=d, teamScore=5}
team{teamName=c, teamScore=10}
team{teamName=a, teamScore=20}
team{teamName=b, teamScore=20}
```

#### Comparator接口

Comparator接口相对comparable接口，功能更多，可以实现升序降序等。可以将comparator看作是一种算法的实现，将算法与数据分离。
> Comparator接口是传两个待比较的object，compare(objectOne, objectTwo)。
> Comparable接口则是object.compareTo(anotherObject)。

```java
public interface Comparator<T> {
    //Returns a negative integer, zero, or a positive integer as the first argument is less than, equal to, or greater than the second.
    int compare(T o1, T o2);
}
```

使用方法：

```java
object.compare(T o1, T o2);
```

去构造一个专门用于compare功能的comparator类，把这个comparator类作为参数传入**Collections.sort(T[], Comparator<? super T>)**或**Arrays.sort(T[], Comparator<? super T>)**中实现排序功能。

```java
//定义待比较的数据结构team
class Team{
    char teamName;
    int teamScore;

    public Team(char teamName, int teamScore) {
        this.teamName = teamName;
        this.teamScore = teamScore;
    }

    @Override
    public String toString() {
        return "team{" +
            "teamName=" + teamName +
            ", teamScore=" + teamScore +
            '}';
    }
}

//希望以分数降序排列，当遇到分数相同时以字母升序排序
class TeamComparator implements Comparator<Team>{

    @Override
    public int compare(Team t1, Team t2) {
        if (t1.teamScore != t2.teamScore){
            return t2.teamScore - t1.teamScore;
        }else {
            //这里会把char自动转义为ascii码计算
            return t1.teamName - t2.teamName;
        }
    }
}
```

下面我们写一个进行测试

```java
public class comparator_interface_test {

    public static void main(String[] args) {
        Team[] teams = {new Team('c',10), new Team('b',20),new Team('d',5),new Team('a',20)};
        for (int i = 0; i < teams.length; i++) {
            System.out.println(teams[i]);
        }
        TeamComparator teamComparator = new TeamComparator();
        Arrays.sort(teams,teamComparator);
        for (int i = 0; i < teams.length; i++) {
            System.out.println(teams[i]);
        }
    }
}
```

输出结果：

```java
team{teamName=c, teamScore=10}
team{teamName=b, teamScore=20}
team{teamName=d, teamScore=5}
team{teamName=a, teamScore=20}
team{teamName=a, teamScore=20}
team{teamName=b, teamScore=20}
team{teamName=c, teamScore=10}
team{teamName=d, teamScore=5}
```

**总结：**

- 待比较的object类 继承 Comparable接口，并实现compareTo()方法，把比较的功能放在了类中。
- new一个专门用于比较的objectComparator类继承Comparator接口，并实现compare(Object o1, Object o2)方法，把比较的功能专门抽取为一个类，做到了**比较功能**与**待比较的类**分离。

### sort方法

在Arrays类中定义了一个排序规则类NaturalOrder，如果不指定排序规则则按照NaturalOrder进行排序。

```java
static final class NaturalOrder implements Comparator<Object> {
    @SuppressWarnings("unchecked")
    public int compare(Object first, Object second) {
        return ((Comparable<Object>)first).compareTo(second);
    }
    static final NaturalOrder INSTANCE = new NaturalOrder();
}
```

- sort(int[])：用于对数组进行排序，内部排序算法用的是DualPivotQuicksort。
- sort(int[], int fromIndex, int toIndex)：用于对数组指定开始下标和结束下标进行排序。
- sort(object[], Comparator<? super T> c)：用给定的Comparator类定义的规则对object数组进行排序。

### search方法

- binarySearch(int[], int key)：寻找key在**排序后的数组**中的下标。（如果key不存在，则返回该插入的位置）

  > 数组中存在多个key值的元素，则返回的下标不定。

### fill方法

- fill(int[], int val)：给数组所有元素赋值。

### equals方法

- equals(int[], int[])：判断两个数组是否相等。