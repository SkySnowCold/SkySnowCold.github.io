---
title: C# 学习笔记 —— 类和接口
author: SkySnowCold
date: 2025-04-05
categories: [.NET]
tags: [.NET, C#]
pin: false
---

自从上次跟着刘铁猛老师的视频学习 C# 之后，已经隔了很长很长时间没有练习编程了，主打一个三天打鱼两天晒网 (￣ε (#￣)，现在抽空整理复习一下。

## 类的声明

类可以声明在命名空间中或者作为成员类声明在类体里，不推荐声明在全局命名空间。声明类有三个必要的元素：`class` 关键字、类名和类体，如下是最基本的类声明格式。

```csharp
namespace Practice {
    class Student {
    }
}
```

## 类与对象的关系

对象也叫实例，是类经过实例化得到的内存中的实体，可以使用 `new` 操作符创建类的实例。类是引用类型，创建一个类的实例时，会在堆上分配内存，变量保存的是实例的引用。

```csharp
Student stu = new Student();
```

## 类和类成员的访问级别控制

- 类的访问级别默认为 `internal`，将类限制为只在同程序集内可以访问
- 被 `public` 修饰的类可以在其他程序集中访问

---

- 类成员的访问级别默认为 `private`，只能在类体里访问
- 被 `public` 修饰的类成员可以被外部的类访问，但是以所属的类的访问级别为上限
- 被 `internal` 修饰的类成员可以在同程序集内访问
- 被 `protected` 修饰的类成员在该类的内部和继承类中可以访问

## 继承

继承指派生类在接收基类所有成员（除了构造和析构函数之外）的基础上进行**类成员数量的扩充**和**类成员版本的更新**。

被 `sealed` 修饰的类不能被继承。一个类只能继承一个基类，但可以实现多个基接口。派生类的访问级别不能超过基类。

实例被创建时，先调用基类构造函数，再调用派生类构造函数；实例被销毁时，先调用派生类析构函数，再调用基类析构函数。

## 虚方法、重写和多态

在基类中声明一个虚方法，派生类可以重写该方法来实现自己的版本，在基类中使用关键字 `virtual` 声明虚方法，同时在继承类中用关键字 `override` 声明重写虚方法，示例如下：

```csharp
class Program {
    static void Main(string[] args) {
        Vehicle v1 = new Vehicle();
        v1.Run(); // 输出 I'm running.

        Vehicle v2 = new Car();
        v2.Run(); // 输出 Car is running.
    }
}

class Vehicle {
    public virtual void Run() {
        Console.WriteLine("I'm running.");
    }
}

class Car : Vehicle {
    public override void Run() {
        Console.WriteLine("Car is running.");
    }
}
```

多态主要通过虚方法、抽象类和接口来实现，使得同一个方法根据对象类型的不同，具有不同的表现形式。多态机制使具有不同内部结构的对象可以共享相同的外部接口。

## 抽象类和接口

抽象类专门作为基类使用（复用，解耦），需要使用关键字 `abstract` 声明抽象类和抽象方法。抽象类的逻辑未完全实现，既可以提供抽象方法，也可以提供非抽象方法。它的派生类实现抽象方法时在方法前要用 `override` 修饰。接口通过关键字 `interface` 声明，完全未实现逻辑，接口成员的默认访问级别为 `public`，逻辑由它的派生类实现。抽象类和接口都不能被实例化。

```csharp
interface IVehicle {
    void Run();
    void Stop();
}

abstract class Vehicle : IVehicle {
    public abstract void Run();

    public void Stop() {
        Console.WriteLine("Stopped");
    }
}

class Car : Vehicle {
    public override void Run() {
        Console.WriteLine("Car is running.");
    }
}

class Truck : Vehicle {
    public override void Run() {
        Console.WriteLine("Truck is running.");
    }
}
```