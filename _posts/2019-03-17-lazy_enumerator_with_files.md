---
layout: post
title: "Lazy Enumerator with Files"
date:  2019-03-17  0:54:27
exclude_rss: true
categories: Ruby
---
In my previous post we discussed about the Enumerator and Lazy Enumerator, here we will apply it with the huge files. First thing
first lets open a txt file in Ruby and display the text matching a specific keyword.

I have a `dummy.txt` file which has the content shown below, as a fair play the number of lines can go up to as much as you want.

```text

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
.....
.....

```

Now we will fetch the first 5 lines of the above txt files using lazy enumerator.

```ruby
file = File.open('dummy.txt')

file.class # => File
file.each_line.class # => Enumerator

file.each_line.lazy.select { |line| line.match(/Lorem/) }.first(5)

# =>
#  ["Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n",
#  "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n",
#  "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n",
#  "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n",
#  "Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n"]

```

What if we have to match certain text from the entire txt file ?

```ruby
file = File.open('dummy.txt')

file.each_line.lazy.each_with_index.map { |line, i| "LINE #{ i }: #{ line }" }.select { |line| line.match(/lorem/i) }.first(3)

# =>
# ["LINE 0: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n",
# "LINE 6: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n",
# "LINE 12: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod\n"]

# After executing the above line the file object will point at Line 13 of the file
# In-order to get the pointer back to the beginning of the file execute the below code
file.rewind

```

Apart from `each_line`, in Ruby we have `each_char`, `each_code_point` and you can use the same methods to play around with it. I have
worked mainly in file system to fetch specific key words from large files and the enumerator provided enough methods to tackle my requirements. Hope this will help some of you guys as well.
