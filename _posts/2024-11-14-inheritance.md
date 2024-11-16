---
title: Inheritance in Java
date: 2024-11-14 17:00:00 -0800
categories: [Programming, Java]
tags: [AP CS]
---

| Resources                                                                    | Description                     |
| :--------------------------------------------------------------------------- | :------------------------------ |
| [W3Schools](https://www.w3schools.com/java/java_inheritance.asp)             | Brief Inheritance Intro         |
| [Oracle](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html) | More Detailed Inheritance Intro |

In Java, we can inherit fields and methods from one class to another. The parent class
(i.e. the one that is being inherited from) is called the **superclass**, while the child class
that is inheriting from the superclass is called the **subclass**.

Let's consider the following example.

```java
class Animal {
    private String name;
    private String sound;
    private int age;
    
    public Animal(String name, String sound, int age) {
        this.name = name;
        this.sound = sound;
        this.age = age;
    }

    public void hello() {
        System.out.println(sound);
    }

    public String getName() {
        return name;
    }

    public String getSound() {
        return sound;
    }

    public int getAge() {
        return age;
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal("Bessie", "Moo", 5);
        animal.hello(); // prints "Moo"
    }
}
```
{: file="Main.java"}

Say we wanted to write a new class called `Dog`. This class should 
also have fields for their name, sound, and age. However, now we want to
add a new attribute for how loud the dog barks. Because all of the attributes
we want in our `Dog` class can be found in our `Animal` class, we can use inheritance
to reuse the code we already wrote.

```java
class Animal {
    private String name;
    private String sound;
    private int age;
    
    public Animal(String name, String sound, int age) {
        this.name = name;
        this.sound = sound;
        this.age = age;
    }

    public void hello() {
        System.out.println(sound);
    }

    public String getName() {
        return name;
    }

    public String getSound() {
        return sound;
    }

    public int getAge() {
        return age;
    }
}

class Dog extends Animal {
    private int noiseLevel;

    public Dog(String name, String sound, int age, int noiseLevel) {
        super(name, sound, age);
        this.noiseLevel = noiseLevel;
    }

    public void bark() {
        if (noiseLevel >= 100) {
            System.out.println("WOOF!");
        } else {
            System.out.println("woof!");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal("Bessie", "Moo", 5);
        animal.hello(); 

        Dog dog = new Dog("Fido", "woof", 6, 100);
        dog.bark(); // prints "WOOF!"
        dog.hello(); // prints "woof"
    }
}
```
{: file="Main.java"}

There are a few important things to take note from this example.
- When we want to inherit another class, we use the keyword `extends`
  - Format is `class Subclass extends Superclass`
- Subclasses can use the methods of the parent class
  - A notable exception is if the method is declared as being `private`
  - In general, **a subclass cannot access private fields or methods from its 
superclass**
- We need to call the superclass constructor in our subclass constructor
  - If we don't, Java by default will call the no argument constructor
  
To elaborate on the third point, we must call our superclass constructor in some
form **at the top of the constructor**. If we don't, Java will call `super()` for us.
In the code above, it is necessary to explicitly call the superclass constructor because
we want to inherit all the fields from the superclass.

> If we don't write a no argument constructor, Java automatically creates one for us.
> However, **this is only if we haven't written any constructors for our class.**
{: .prompt-info }

## Quiz

<p style="margin-bottom: 20px;"> </p>

### Question 1

```java
class Vehicle {
    Vehicle(String type) {
        System.out.println("Vehicle: " + type);
    }
}

class Car extends Vehicle {
    Car() {
        super("Car");
        System.out.println("Car: Engine started");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle myVehicle = new Car();
    }
}
```
{:file="Main.java"}

What is printed by the code snippet?

1. 
        Vehicle: Car

2. 
        Vehicle: Car
        Car: Engine started
3. 
        Car: Engine started

4. 
        Car: Engine started        
        Vehicle: Car

5. This code doesn't compile.

<details>
  <summary>Answer</summary>
  Option 2 is correct. When we call the constructor for <code>Car</code>, it calls the superclass constructor, which is the constructor for 
  <code>Vehicle</code>. That method prints out "<code>Vehicle: Engine started</code>". Then, we return back to the constructor for <code>Car</code>
  and print "<code>Car: Engine started</code>".

</details>

<p style="margin-bottom: 20px;"> </p>

### Question 2

```java
class Super {
    Super() {
        System.out.println("Super");
    }
}

class Sub {
    Sub() {
        System.out.println("Sub");
    }
}

public class Main {
    public static void main(String[] args) {
        Sub sub = new Sub();
    }
}
```
{: file="Main.java"}

What is printed by the code snippet?

1. 
        Super
2. 
        Sub
3. 
        Super
        Sub

4. 
        Sub
        Super

<details>
  <summary>Answer</summary>
  Option 3 is correct. When we call the constructor for <code>Sub</code>, it first implicitly calls the constructor 
  for <code>Super</code>. Thus, it prints <code>Super</code>, then <code>Sub</code>.
</details>

<p style="margin-bottom: 20px;"> </p>

### Question 3

```java
class Super {
    public int x;

    Super(int x) {
        this.x = x;
        System.out.println("Super " + x);
    }
}

class Sub extends Super {
    private int y;
    
    Sub(int x, int y) {
        this.x = x;
        this.y = y;
        System.out.println("Sub " + y);
    }
}

public class Main {
    public static void main(String[] args) {
        Sub sub = new Sub(1, 3);
    }
}
```
{: file="Main.java"}

Why doesn't this code compile?

1. The no-args constructor is called for `Super`, but no such constructor exists.
2. The constructor for `Sub` attempts to access a field in `Super`.
3. `x` was declared as a public variable, whereas it should have been declared as private.

<details>
  <summary>Answer</summary>
  <p>Option 1 is correct. If not explicitly called, Java will call the superclass constructor.
  However, because it calls the no-args constructor and no such constructor exists, it results
  in an error.</p>

  <p>Option 2 is wrong because <code>x</code> was declared as a public field, so it was inherited.</p>

  <p>Option 3 is wrong because fields can be public, but generally should be left as private.</p>
</details>
