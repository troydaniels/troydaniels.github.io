---
layout: post
title:  "Learn You a Haskell P2"
date:   2017-10-09 23:00:20 +1100
categories: Learning Haskell
comments: true
---


So I've gone ahead and bought [Learn You A Haskell for Great Good], which should come tomorrow - and in the interim, I've been working through the online version of the text.

[Learn You A Haskell for Great Good]: http://learnyouahaskell.com/

The language itself is a little hard to get my head around - being completely different to other languages I'm familiar with.

Here's what I've discovered so far:

* Every function must return a value
* Haskell makes heavy use of lists
* There's a bunch of patern matching
* It favours recursion over itteration

---
<br>
Working throught the text, my solutions to the first 10  of the [Ninety-Nine Lisp Problems] can be found below:

[Ninety-Nine Lisp Problems]: http://www.ic.unicamp.br/~meidanis/courses/mc336/2006s2/funcional/L-99_Ninety-Nine_Lisp_Problems.html

1: Find the last element of a list.

{% highlight hs startinline=true %}
myLast :: [a] -> a
myLast [] = error "Last element of empty list."
myLast [x] = x
myLast (x:xs) = myLast xs
{% endhighlight %}

Apart from what feels like slightly awkward syntax, this and the next few answers are pretty trivial. The LHS of the equality behaves a bit like a switch, and taking this example, we simply recurse on the tail of the list until we reach the last element. 

2: Find the last but one element of a list.
{% highlight hs startinline=true %}
myButLast :: [a] -> a
myButLast [] = error "Second last element of empty list."
myButLast [x] = error "Second last element of single element."
myButLast (xs) = last (init (xs))
{% endhighlight %}

3: Find the K'th element of a list. The first element in the list is number 1.
{% highlight hs startinline=true %}
elementAt :: [a] -> Int -> a
elementAt [] n = error "Empty list."
elementAt (x:_) 1  = x
elementAt (x:xs) n = elementAt xs (n-1)
{% endhighlight %}

Similarly here, we recurse again on the tail, decreasing a counter as we go. When the counter reaches 1, we return the next element.

4: Find the number of elements of a list.
{% highlight hs startinline=true %}
myLength :: [a] -> Int
myLength [] = 0
myLength (x:[]) = 1
myLength (x:xs) = 1 + myLength xs
{% endhighlight %}

5: Reverse a list.
{% highlight hs startinline=true %}
myReverse :: [a] -> [a]
myReverse [] = []
myReverse (x:[]) = [x]
myReverse (x:xs) = (myReverse xs) ++ [x]
{% endhighlight %}

6: Find out whether a list is a palindrome. A palindrome can be read forward or backward; e.g. (x a m a x).
{% highlight hs startinline=true %}
isPalindrome :: (Eq a) => [a] -> Bool
isPalindrome xs = xs == reverse xs
{% endhighlight %}

Rather pleasing - we call ```reverse``` on a string, test for equality, and return the result as a ```Bool```.

7: Flatten a nested list structure.
{% highlight hs startinline=true %}
data NestedList a = Elem a | List [NestedList a]

flatten :: NestedList a -> [a]
flatten (Elem a)   = [a]
flatten (List (x:xs)) = flatten x ++ flatten (List xs)
flatten (List [])     = []
{% endhighlight %}

A little more complex this time, we have to define our own data type (of which the definition itself is recursive), and then we recurse in two different directions, concatenating and returning the answer.

8:  Eliminate consecutive duplicates of list elements.
{% highlight hs startinline=true %}
compress :: Eq a => [a] -> [a]
compress [] = []
compress (x:[]) = [x]
compress (x:xs)
    | x == (head xs)  = (x : compress (tail xs))
    | otherwise       = (x : compress xs)
{% endhighlight %}

Slightly more complex again, two guard statemtents on the last line test to see if there is a duplicate element, and recurses on the remainder of the list. We've also stated explicitly in the function definition that we're testing for equality over ```a``` - which isn't strictly necessary in this question, however I have found becomes increasingly important to avoid pesky compiler errors as the functions (and their types) become increasingly complex.

I heard on a podcast today that said that a running joke amongst Haskell programmers says, you'll know you function works when the compiler stops complaining, which so far seems fairly accurate.

9: Pack consecutive duplicates of list elements into sublists. If a list contains repeated elements they should be placed in separate sublists.
{% highlight hs startinline=true %}
pack :: Eq a => [a] -> [[a]]
pack [] = []
pack [x] = [[x]]
pack (x:xs)
  |  (x == head xs) = (x:head (pack xs)):tail (pack xs)
  | otherwise = [x]:pack xs
{% endhighlight %}

At this point, I found myself slowing down considerably, having to think more thoroughly about how to define the problem recursively. In the first guard statement, we're recursing in two separate directions, and it takes some thought to convince yourself that that this statement indeed yields the desired structure.


10: Run-length encoding of a list. Use the result of problem P09 to implement the so-called run-length encoding data compression method. Consecutive duplicates of elements are encoded as lists (N E) where N is the number of duplicates of the element E.
{% highlight hs startinline=true %}
pack :: Eq a => [a] -> [[a]]
pack [] = []
pack [x] = [[x]]
pack (x:xs)
  |  (x == head xs) = (x:head (pack xs)):tail (pack xs)
  | otherwise = [x]:pack xs

encode :: Eq b => [b] -> [(Int, b)]
encode (xs) = [(length x, head x) | x <- pack xs]
{% endhighlight %}

Having made it through question nine, however, this problem is conceptually easier to solve. A new feature here in the list comprehension in the last line of the function, but it's pretty close to a set definition in maths, so it's not too bad really.

---
<br>
And that's it! First ten down - only took 7 months. I'll ensure to get through the next ten more hastily next time.
