# HashMap常用操作笔记

## 遍历

#### for each遍历

```java
Map<Character, Integer> map = new HashMap<Character, Integer>();
```

- 遍历<key, value>

  - 使用Map.entrySet方法获取map中所有键值对。

    ```java
    //Returns a Set view of the mappings contained in this map.
    Set<Map.Entry<K, V>> entrySet();
    
    //获取map中所有键值对并将其存储在set中
    Set<Entry<Character, Integer>> entries = map.entrySet();
    ```

  - for each 遍历

    ```java
    for(Map.Entry : map.entrySet()){
        System.out.println("key = " + entry.getKey() +"," + "value = " + entry.getValue());
    }
    
    class Student {
        Character name;
        int age;
    }
    int i =0;
    Student[] studentsArrays = new Student[map.size()];
    //如果需要将HashMap中所有键值对保存到对象数组中则需要指定Entry<K, V>的类型
    for(Map.Entry : map.entrySet()){
        studentsArrays[i] = new Student(entry.getKey(), entry.getValue());
        i++;
    }
    ```

- 遍历key

  ```java
  //用keySet()获取所有的key
  for(Character key : map.keySet()){
      System.out.println("key = " + key);
  }
  ```

- 遍历value

  ```java
  //用values()获取所有的value
  for(Integer value : map.values()){
      System.out.println("value = " + value);
  }
  ```

#### 迭代器遍历

这是集合类的通用遍历方法。

```java
//迭代entry
//使用泛型
Iterator iterator = map.entrySet().iterator();
while (iterator.hasNext()){
    Map.Entry entry = (Map.Entry) iterator.next();
    System.out.println("key = " + entry.getKey() +"," + "value = " + entry.getValue());
}

//迭代key
Iterator<Character> keyIterator = map.keySet().iterator();
while (keyIterator.hasNext()){
    Character key = keyIterator.next();
    System.out.println("key = " + key);
}

Iterator<Integer> valueIterator = map.values().iterator();
while (valueIterator.hasNext()){
    Integer value = valueIterator.next();
    System.out.println("value = " + value);
}

```

## 排序

HashMap中按照key的hashcode来存储数据，其中元素是无序的。

### 按key排序

按照key排序，首先用keySet()得到所有的key，然后将`keys`转为数组，最后用Arrays.sort()来对数组排序。

```java
Set<Character> keys = map.keySet();
Object[] keyArrays = keys.toArray();
Arrays.sort(keyArrays);
for (Object key: keyArrays) {
    System.out.println("key = " + key + ",value = " + value);
}
```

- 如果key是自定义的对象，可以让key  implement Comparable接口，来实现自定义排序规则。

### 按value排序

按照value进行排序，必须要用到comparable接口/comparator接口。

#### 使用Comparable接口

1. 用entrySet()把所有映射关系取出来

2. 创建一个map<k, v>对应的Student类并实现comparable接口

3. 把entrySet()中所有元素存入Student对象数组中。

4. Arrays.sort()根据comparable接口定义的规则进行排序。

```java
class Student implements Comparable<Student>{
    Character name;
    int age;
    
    //按照年龄降序排序,如果年龄一样,按照名字升序排序
    @Override
    public int compareTo(Student o) {
        if (age != o.age){
            return o.getAge() - age;
        }else {
            return name - o.getName();
        }
    }
}

Set<Entry<Character,Integer>> entrySet = map.entrySet();
Student[] studentArrays = new Student[entrySet.size()];
int j = 0;
for (Entry<Character, Integer> entry: entrySet) {
    studentArrays[j++] = new Student(entry.getKey(),entry.getValue());
}
Arrays.sort(students1);
```

#### 使用Comparator接口

1. 用entrySet()把所有映射关系取出来。
2. 自定义一个student类来作为存储映射关系的类，建立一个studentList来存储entrySet()的所有映射关系。
3. 自定义studentComparator类
4. list.sort(Comparator)或者Collection.sort(list, Comparator)

```java
class Student {
    Character name;
    int age;
}

class StudentComparator implements Comparator<Student>{

    //按年龄降序排序，如果年龄一样，按照姓名升序排序
    @Override
    public int compare(Student o1, Student o2) {
        if (o1.getAge() != o2.getAge()){
            return o2.getAge() - o1.getAge();
        }else {
            return o1.getName() - o2.getName();
        }
    }
}

Set<Entry<Character,Integer>> entrySet = map.entrySet();
List<Student> studentList = new ArrayList<>(entrySet.size());

Iterator<Map.Entry<Character, Integer>> entryIterator = entrySet.iterator();
while (entryIterator.hasNext()){
    Map.Entry<Character, Integer> entry = entryIterator.next();
    Student student = new Student(entry.getKey(),entry.getValue());
    studentList.add(student);
}
StudentComparator studentComparator = new StudentComparator();
studentList.sort(studentComparator);
```

