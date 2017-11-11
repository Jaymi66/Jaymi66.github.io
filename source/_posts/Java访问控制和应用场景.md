---
title: Java访问控制和应用场景
date: 2017-11-10 10:59:12
tags:
---

Java 中的访问控制关键字是负责类内部成员变量和方法安全性的, 简单的说关键字是为设置外部是否能直接访问到该类内部的成员变量和方法而生的.

<!--more-->

Java 中有三个访问控制符, 分别是: public、protected、private

有四种访问权限, 分别是: default、public、protected、private

在类中声明属性和方法, 包括声明内部类时, 都可以关键字声明对应的访问权限.

1. default: 默认包访问权限, 声明属性和方法时, 如果没有使用任何的权限声明则使用`default`权限. 默认权限是本包的其他类都可以访问, 但无法被其它包的类访问.
2. public: 公共访问权限, 使用`public`进行权限控制, public 权限可以被所有的类访问.
3. protected: 继承访问权限, 使用`protected`进行权限控制, protected权限可以被本包中的其他类和其它包中的子类访问到.
4. private: 私有访问权限, 使用关键字`private`进行权限控制, 使用private声明的成员变量和方法只能在本类中被访问, 其它所有类都无法访问.

<table>
  <tr>
    <th>范围</th>
    <th>private</th>
    <th>default</th>
    <th>protected</th>
    <th>public</th>
  </tr>
  <tr>
    <td>同一个类中</td>
    <td>√</td>
    <td>√</td>
    <td>√</td>
    <td>√</td>
  </tr>
  <tr>
    <td>同一个包中(子类和非子类)</td>
    <td>×</td>
    <td>√</td>
    <td>√</td>
    <td>√</td>
  </tr>
  <tr>
    <td>不同包中的子类</td>
    <td>×</td>
    <td>×</td>
    <td>√</td>
    <td>√</td>
  </tr>
  <tr>
    <td>不同包中的非子类</td>
    <td>×</td>
    <td>×</td>
    <td>×</td>
    <td>√</td>
  </tr>
</table>


### 应用场景

1. default: 如果不写访问权限关键字则是`default`, 比如在定义接口是可以不写内部方法的权限控制, 该接口可以被本包的类实现.
2. public: `main`方法必须用`public`, 因为该方法要被JVM外部访问到. public的应用场景很广泛, 如POJO类中的`getter``setter`方法都需要设置public.
3. protected: `protected`比较特殊, 也少有对应的应用场景, 如果你想该成员变量和方法只能被外部的子类访问到, 则使用该关键字.
4. private: 私有访问权限的意思是外部完全访问不到, 如POJO的成员变量不能被外部直接访问到, 所以会设置为`private`.
