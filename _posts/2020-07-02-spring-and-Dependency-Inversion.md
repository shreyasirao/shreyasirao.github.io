---
title: Spring and Dependency Inversion
date: 2020-07-02 05:33:11
---

A spring container does the following-
- Create and Manage objects (IoC)
- Inject Object's dependencies (DI)

Spring provides an Object factory that will give us an object whenever we need based on Configuration provided by us.
> IoC is also known as dependency injection (DI). It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern.

### What are Spring Beans?

When Java objects are created by the Spring Container, then Spring refers to them as "Spring Beans".

> In Spring, the objects that form the backbone of your application and that are managed by the Spring IoC container are called beans. A bean is an object that is instantiated, assembled, and managed by a Spring IoC container. Otherwise, a bean is simply one of many objects in your application. Beans, and the dependencies among them, are reflected in the configuration metadata used by a container.

1. Configure Spring beans. This can be done in 3 ways -
- XML configuration file
- Java Annotations
- Java Source code
2. Create Spring Container- generally known as an Application context

>The org.springframework.context.ApplicationContext interface represents the Spring IoC container and is responsible for instantiating, configuring, and assembling the beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. The configuration metadata is represented in XML, Java annotations, or Java code. It lets you express the objects that compose your application and the rich interdependencies between those objects.

3. Retrieve beans from Spring container


Dependency Injection can be
1. Constructor Injection
2. Setter Injection


