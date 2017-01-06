---
layout: post
title:  "Solved: Factorialize a Number"
date:   2016-12-09
---

While "factorialize" is far from being a real word, this is a real challenge.
The task is this: given an integer, return the factorial of that integer.
For example, `factorialize(5)` should return `5 * 4 * 3 * 2 * 1` which equals `120`.

This challenge (from the Free Code Camp "Basic Algorithm Scripting" tasks) can be done in numerous ways, and I'm sure there's a better way to do it, but I'll provide two solutions I thought of for you here.

The first solution I'll provide was created by yours truly a couple of months ago when I was first learning to code.

#### Solution 1
```
function factorialize(num) {
  var product = num;

  if(num === 0){
    return 1;
  }
  else{
    while (num > 1){
      product *= num - 1;
      num = num - 1;
    }
  }

  return product;
}
```
{: .language-javascript}

Let's make this better. First of all, there's no reason to arrive at the factorial by counting down (`5 * 4 * 3 * 2 * 1`). I can more easily get the correct answer by counting up. Second of all, there's no need to update *two* variables, `product` and `num`.  One good thing about this solution, however, is that it establishes a base case (if `num` is 0, return 1). The base case is very important. Okay, onwards.

#### Solution 2 (Going up!)
```
function factorialize(num){
  var product = 1;
  for(var i = 1; i <= num; i++){
    product *= i;
  }
  return product;
}
```
{: .language-javascript}

Much better. Let's review: base case is handled implicitly (`product` is initially set to 1 and the code in the for loop will be skipped since `i = 1` already satisfies the break condition `i <= num`), the handy `*=` operator appears once again. This operator is very cool: it multiplies two numbers (left side and right side of the operator), and assigns the result to the left side (`product` in this case). Finally, we return product and we're done!

I keep accidentally ending sentences with semicolons instead of periods. What's happening to me...
