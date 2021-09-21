---
layout: center
class: 'text-white'
---
# Java 101

---
layout: center
class: 'text-white'
---

## whoami ? 
<style type="text/css">
    img {
        width: 250px;
    }
</style>
![Me](index.jpeg)

1. Java/Linux/DB/whatever business needs dev
2. Open source enthusiast
3. <logos-Blogger/> **Blog** - https://strogiyotec.github.io/
4. <logos-git/> **Github** - https://github.com/strogiyotec
4. <logos-telegram/> **Telegram** - @Lollipopster

---
layout: center
class: 'text-white'
---
# What will be covered ? 
1. What is Java ?
2. What is JVM ? 
3. You said Gc ?
4. Threading model
5. JIT compilation
6. Future of Java
   
---
layout: center
class: 'text-white'
---
# What won't be covered
1. Syntax (cmon just google it)
2. Spring(it's complicated)
3. How to become Java developer?


---
layout: center
class: 'text-white'
---
![Java is slow](2021-09-18_17-08.png)

---
layout: center
class: 'text-white'
---
# Why ? 
1. It's interpreted
2. It's a GC language
3. Because NodeJs is faster
4. Because other people say so


---
layout: center
class: 'text-white'
---
# Compiled vs Interpreted


---
layout: center
class: 'text-white'
---
# Depends

##  Compiled language is a programming language whose implementations are typically compilers (translators that generate machine code from source code)

---
layout: center
class: 'text-white'
---
# Meet **javac**

#### .java -> .class

---


```java
public class Main {

    public static void main(String[] args) {
        int a = 5;
        int b = 10;
        int c = a + b;
        System.out.println(c);
    }
}

```
--- 

1. `javac Main.java` - compile it
2. `javap -v Main ` - show me the bytecode
```java {7,9,12,13}
  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=4, args_size=1
         0: iconst_5
         1: istore_1
         2: bipush        10
         4: istore_2
         5: iload_1
         6: iload_2
         7: iadd
         8: istore_3

```

<a href="https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-6.html">Checkout other bytecode instructions</a>

---
layout: center
class: 'text-white'
---
# What is a bytecode?

1. Ease interpretation
2. Reduce OS dependence

---
layout: center
class: 'text-white'
---
# What about python ? 


```python
print("Let's print Full name")
full_name = first_name+" "+"Abdrazak"
print(full_name)
```
## Output
```python
Let's print Full name
Traceback (most recent call last):
  File "test.py", line 2, in <module>
    full_name = first_name+" "+"Abdrazak"
NameError: name 'first_name' is not defined
```

---
layout: center
class: 'text-white'
---
# Let's write an interpeter

---
layout: center
class: 'text-white'
---

```java
public class Interpreter {

    public static void main(String[] args) {
        var bytecode = "2*3";
        var numbers = new Stack<Integer>();
        var operands = new Stack<Character>();
        for (int i = 0; i < bytecode.length(); i++) {
            char c = bytecode.charAt(i);
            if (Character.isDigit(c)) {
                if (numbers.isEmpty()) {
                    numbers.push(Character.getNumericValue(c));
                } else {
                    numbers.push(Character.getNumericValue(c));
                    char operand = operands.pop();
                    switch (operand) {
                        case '+':
                            System.out.println(numbers.pop() + numbers.pop());
                            break;
                        case '*':
                            System.out.println(numbers.pop() * numbers.pop());
                            break;
                    }
                }
            } else {
                operands.push(c);
            }
        }

    }
}
```

---
layout: center
class: 'text-white'
---

# JVM - Java virtual Machine
1. Memory Management
2. Interpreter
3. Gc
4. JIT

---
layout: center
class: 'text-white'
---

# Memory Management
1. Stack
2. Heap

---
layout: center
class: 'text-white'
---

## Stack

1. Unique per thread
2. Has a limited size(8 Mb for AdoptOpenJDK, checkout doc for your distribution)
3. Store local primitives and references
4. StackOverFlowException

---
layout: center
class: 'text-white'
---

```java 
 void add(int first,int second){
        int c = first+second;
        return c;
}
```
![Img](stack.png)

#### Total size - around 16 bytes(`ref to **this** 4 bytes,3 ints *4`)
#### (Not really -_-)

---
layout: center
class: 'text-white'
---

# Heap

1. Stores all objects
2. Same for all threads
3. Subject for Garbage Collection
4. OutOfMemoryException
5. By default `java -XX:+UnlockDiagnosticVMOptions -XX:+PrintFlagsFinal -version`

---
layout: center
class: 'text-white'
---

# Question - how much memory ? 
```java

class Integer extends Number{
    int number;//int takes 4 bytes
}

Integer number = new Integer(42);
```
1. 4 byte
2. 8 byte
3. 16 byte

---
layout: center
class: 'text-white'
---
![Integer](Object.drawio.png)

---
layout: center
class: 'text-white'
---

# Question 2

```java

class User {
    boolean enabled;//boolean takes 1 byte
}

User user = new User(false);
```

Metadata(8 byte) + Ref to class(4 byte) + enabled(1 byte) = 13 bytes ? 

---
layout: center
class: 'text-white'
---
# Right Answer is 16 bytes
## All objects are alligned by 8 bytes

![User](User.drawio.png)

Same as 

```java
class User{
    //2 bytes for padding
    boolean enabled;
    boolean isAdmin;
}
```

---
layout: center
class: 'text-white'
---


```java

ArrayList<Integer> list = array(100);
```
400*4 = 1600 bytes

---
layout: center
class: 'text-white'
---

# Any solution ? 
1. Use primitives as much as you can(`int[] instead of ArrayList<Integer>`)
2. [Project Valhala `List<int>`](https://openjdk.java.net/projects/valhalla/) 
   
   
---
layout: center
class: 'text-white'
---

# Max Heap Size(not only Java)
1. x32 - 2^32 ~ 4 Gygabytes
2. x64 - 2^64 ~ 2000 Petabytes

But.... big but there are **OOPS**.
For heaps < 32G Java references take 4 bytes, as soon as
your heap is bigger than 32G then all references are changed to take 8 bytes

> Heap 32G is better than Heap 44G
