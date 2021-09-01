Inheritance is a relation between two classes. Inheritance create a parent child relationship between classes. It is a mechanism for code reuse and to allow independent extensions of the original software via public classes and interfaces.The benefit of inheritance is that classes lower down the hierarchy get the features of those higher up, but can also add specific features of their own.

In Ruby, a class can only inherit from a single other class. (i.e. a class can inherit from a class that inherits from another class which inherits from another class, but a single class can not inherit from many classes at once). The BasicObject class is the parent class of all classes in Ruby. Its methods are therefore available to all objects unless explicitly overridden.

Ruby overcome the single class inheritance at once by using the mixin.

I will try to explain with an example.

```
module Mux
 def sam
  p "I am an module"
 end
end
class A
  include Mux
end
class B < A
end
class C < B
end
class D < A
end
```

You can trace by using class_name.superclass.name and do this process unless you found BasicOject in this hierarchy. BasicObject is super class o every classes. lets see suppose we want to see class C hierarchy tree.

```
 C.superclass
   => B
 B.superclass
  => A
 A.superclass
  => Object
 Object.superclass
  => BasicObject
 ```
 
You can see the whole hierarchy of class C. Point to note using this approach you will not find modules that are included or prepended in the parent classes.

There is another approach to find complete hierarchy including modules. According to Ruby doc ancestors. Returns a list of modules included/prepended in mod (including mod itself).
```
C.ancestors
 => [C, B, A, Mux, Object, Kernel, BasicObject]
Here, Mux and Kernel are Modules.
```
http://rubylearning.com/satishtalim/ruby_inheritance.html https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)
