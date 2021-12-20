# Guidelines for operator overloads

Operator overloads are a powerful feature, but a common pattern is people will try to use them in ways that seem clever at the time, but 
make using or maintaining that code be more difficult than it would be otherwise.


## Which operators should I override? 

Bottom line: don’t confuse your users.
 
Remember the purpose of operator overloading: to reduce the cost and defect rate in code that uses your class. If you create operators
that confuse your users (because they’re cool, because they make the code faster, because you need to prove to yourself that you can do
it; doesn’t really matter why), you’ve violated the whole reason for using operator overloading in the first place.


## Don't use operator overloads for things completely unrelated to the symbols

e.g. C++ cin/cout uses the bitwise shift operators for stuff completely unrelated to bitwise shifting. This was one of the biggest mistakes

## Don't use operator overloads for things that are only ever going to be one line.

Imagine you have a 'shopping basket' to which users can add items, you could try to be too clever and do it like:

```
$basket += $item;
```

But that isn't any easier to read than:

```
$basket->addItem($item);
```

And in fact is less explicit.


## Don't try to be cute

Anything even remotely similar to:

```
$cached_db = $db + $redis_cache;
```

## Most of the guidelines found here:

https://isocpp.org/wiki/faq/operator-overloading#op-ov-rules

1. Use common sense. If your overloaded operator makes life easier and safer for your users, do it; otherwise don’t. This is the most important guideline. In fact it is, in a very real sense, the only guideline; the rest are just special cases.

2. If you define arithmetic operators, maintain the usual arithmetic identities. For example, if your class defines x + y and x - y, then x + y - y ought to return an object that is behaviorally equivalent to x. The term behaviorally equivalent is defined in the bullet on x == y below, but simply put, it means the two objects should ideally act like they have the same state. This should be true even if you decide not to define an == operator for objects of your class.

3. You should provide arithmetic operators only when they make logical sense to users. Subtracting two dates makes sense, logically returning the duration between those dates, so you might want to allow date1 - date2 for objects of your Date class (provided you have a reasonable class/type to represent the duration between two Date objects). However adding two dates makes no sense: what does it mean to add July 4, 1776 to June 5, 1959? Similarly it makes no sense to multiply or divide dates, so you should not define any of those operators.

4. You should provide mixed-mode arithmetic operators only when they make logical sense to users. For example, it makes sense to add a duration (say 35 days) to a date (say July 4, 1776), so you might define date + duration to return a Date. Similarly date - duration could also return a Date. But duration - date does not make sense at the conceptual level (what does it mean to subtract July 4, 1776 from 35 days?) so you should not define that operator.

6. If you provide constructive operators, they should not change their operands. For example, x + y should not change x. For some crazy reason, programmers often define x + y to be logically the same as x += y because the latter is faster. But remember, your users expect x + y to make a copy. In fact they selected the + operator (over, say, the += operator) precisely because they wanted a copy. If they wanted to modify x, they would have used whatever is equivalent to x += y instead. Don’t make semantic decisions for your users; it’s their decision, not yours, whether they want the semantics of x + y vs. x += y. Tell them that one is faster if you want, but then step back and let them make the final decision — they know what they’re trying to achieve and you do not.

7. In general, your operator should change its operand(s) if and only if the operands get changed when you apply the same operator to intrinsic types. x == y and x << y should not change either operand; x *= y and x <<= y should (but only the left-hand operand).




