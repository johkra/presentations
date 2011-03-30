Ruby
====

---

Highly dynamic language

In tradition of SmallTalk and Lisp

Popular for scripting and web development

---

Hello World
-----------

    !ruby
    def say_hello(name)
        puts "Hello, #{name}"
    end

    say_hello "world"

---

# Dynamism (1)

Message passing:

    !ruby
    1 + 1
    => 2
    1.send(:+, 1)
    => 2
    
    "hello".send "length"
    => 5
