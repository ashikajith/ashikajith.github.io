---
layout: post
title: "Ruby Equal Operator"
date:  2019-02-08 13:37:32
exclude_rss: true
categories: Ruby
---

Recently one of my junior colleagues asked about when we should use === operator and when we got to use == operator. As old school hack, I slipped past the question by asking him to google it. Even though he clarified his query, I thought of mentioning  about it as my first post here. It’s rather outdated but might help newbie Ruby developers in the future.

<b>Basically the === operator is used to check if a particular instance belong to the ancestors of the class. Let's dig into it using a `String`. </b>

<pre>
    >> 'We belong to the same Family'.class.ancestors
    # => [String, Comparable, Object, Kernel, BasicObject]

    >> String === "We belong to the same Family"
    # => true

    >> Object === "We belong to the same Family"
    # => true

    >> Comparable === "We belong to the same Family"
    # => true

    # How about we check with Integer class ?

    >> Numeric === "We belong to the same Family"
    # => false
</pre>
<b>The Equality operator can also be used to double up to identify an element that can fall under the range of elements.</b>
<pre>
    >> /^[A-Z a-z]*$/ === "We belong to the same Family"  
    # => True

    >> /^[a-z]*$/ === "I AM FROM UPPERCASE"  
    # => False

    # For a range, Triple Equal is an alias of `includes?`
    >> ("a".."z") === "a"
    # => true
</pre>

<b> Last but not the least, the Equality operator can be used to call a `Proc` method. </b>

Consider the following example..

<pre>
    my_proc = Proc.new do |argument|
        puts "This is my proc and I called #{argument}"
    end

    # Normal call method
    >> my_proc.call('using call method')
    # => This is my proc and I called using call method

    # Use of Equality operator
    >> my_proc === ('using Equality operator')
    # => This is my proc and I called using Equality operator
</pre>
