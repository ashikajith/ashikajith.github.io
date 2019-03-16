---
layout: post
title: "Play Lazy with Enumerators but not too Lazy !!"
date:  2019-03-16  1:38:32
categories: Ruby
---

**Enumerator**, A simple class in Ruby which offers tons of features and flexibility to play with collection of objects. Each time I surf for this I learn more about it, that's how I felt about Enumerator. Now coming back to the post, I'll start with one end and lets see how far we can go. Without any delay let's start digging until I stop :)

# Enumerator Initialization

```ruby
# Normal Iteration
number = [1,2,3,4,5].each {|el| p el }
1
2
3
4
5
=> [1, 2, 3, 4, 5]
```

```ruby
# Revoking Enumerator
number = [1,2,3].each
=> #<Enumerator: [1, 2, 3, 4, 5]:each>
```
Now we that have invoked the Enumerator, lets play with it for a while.

```ruby
number.next
=> 1
number.next
=> 2
number.next
=> 3
number.next
 (irb):11:in `next'
StopIteration (iteration reached an end)
```

 Waww!! I think we have reached the limit, lets rewind it and execute it again. Feel free to execute the below code and do the above steps again.

```ruby
number.rewind
```

Now we use some of the quality methods to get some outputs from a set of ranges. For example a collection of numbers which are divisible by 2 and 3 from the range 1 to 100.

```ruby
(1..100).select{|num| num %2 == 0 && num %3 == 0}
=> [6, 12, 18, 24, 30, 36, 42, 48, 54, 60, 66, 72, 78, 84, 90, 96]
```

Nice and easy but If we need to get control on each iteration? Well for that we need to use the `next` method. Here are few alternatives.

```ruby
# Method 1
element = (1..100).each
loop do
    number = element.next
    p num if num % 2 == 0 && num % 3 == 0
end
# Method 2
twos_and_threes = Enumerator.new do |result|
    1.upto(100) do |num|
        result << if num % 2 == 0 && num % 3 == 0
    end
end
loop { p twos_and_threes.next }
```

IMO `Method 2` is kind of sexy :) but both methods are messy and ugly. The advantage is we have the control on each iteration so that we can do other operation for a specific element. But it is not efficient and thus we look out for **Lazy Enumerator**.

# Lazy Enumerator Initialization
Returns a class name Lazy Enumerator which enumerates value only if it is needed. Check out the document [here](https://ruby-doc.org/core-2.0.0/Enumerator/Lazy.html).

```ruby
# Lazy Enumerator
number = [1,2,3].lazy
=> #<Enumerator::Lazy: [1, 2, 3]>
number.next
=> 1
number.next
=> 2
```

No big difference with the first one, But lets get to our earlier example!

```ruby
number = (1..100).lazy.select{|num| num %2 == 0 && num %3 == 0}
=> #<Enumerator::Lazy: #<Enumerator::Lazy: 1..100>:select>
number.next
=> 6
irb(main):027:0> number.next
=> 12
irb(main):028:0> number.next
=> 18
irb(main):029:0> number.next
=> 24
# What if we want the complete set? call it with `force`
number.force #Works with to_a
=> [6, 12, 18, 24, 30, 36, 42, 48, 54, 60, 66, 72, 78, 84, 90, 96]
```

These methods will be really useful when it comes to Infinite series. Lets consider the below example.

```ruby
infinite_range = (1..Float::INFINITY)
number = infinite_range.lazy.select{|num| num % 1024 == 0 && num % 1033 == 0}
number.next
=> 1057792
irb(main):042:0> number.next
=> 2115584
irb(main):043:0> number.next
=> 3173376
irb(main):044:0> number.next
=> 4231168
```

# Where we can apply these ?

All these methods are created for a purpose. From my experience there was a time I had to process big text files to find a specific keyword or pattern and the Lazy Enumerator have saved me a lot to tackle those business problems. I will try to give a squeeze on the file management with Lazy Enumerator in my next post.
