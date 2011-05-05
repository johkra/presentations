Ruby
====

---

Ruby
----

* Highly dynamic language
* Inspired by Perl, SmallTalk and Lisp
    - Everything is an Object
    - Higher Order functions
* Popular for scripting and web development

---

Hello World
-----------

    !ruby
    def say_hello(name)
        puts "Hello, #{name}"
    end

    say_hello "world"
    #Hello, world

---

# Dynamic behaviour (1)

Message passing:

    !ruby
    1 + 1
    #=> 2
    1.send(:+, 1)
    #=> 2
    
    "hello".send "length"
    #=> 5

---

# Dynamic behaviour (2)

Define classes at run time:

    !ruby
    Dog = Class.new
    Dog.send(:define_method, "bark",
        lambda { puts "Woof!" })

    waldo = Dog.new
    waldo.bark
    #Woof!

---

# Dynamic behaviour (3)

Open classes:

    !ruby
    class Fixnum
      alias_method :old_to_s, :to_s
      def to_s
        if self == 4 then "5"
        else old_to_s end
      end
    end

    2 + 2
    #=>5

---

# Dynamic behaviour (4)

methods_missing:

    !ruby
    class Factorial
      def method_missing(method, *args)
        match = method.to_s.match /f(\d+)!/
        return super if match.nil?
        $1.to_i.downto(1).reduce(:*)
      end
    end

    fact = Factorial.new
    fact.f25!
    #=> 15511210043330985984000000

---

# Advantages

* Domain specific languages
    - Sinatra, Sequel
* Concise code thanks to "magic"
* Less repetition (DRY)
* Generally more flexible and faster to develop
* REPL

---

# Disadvantages

* Static verification very difficult
    - testing to prevent trivial errors
* (Significantly) Slower
    - Rewrite parts in C/Java
    - JIT can be very fast (LuaJIT)
* Different mindset necessary -> Training
