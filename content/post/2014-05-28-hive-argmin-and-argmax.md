---
title: Hive argmin and argmax
author: joefkelley
layout: post
date: 2014-05-28
url: /727
sfw_pwd:
  - I2bSUdo5IV0N
categories:
  - Programming

---
By far the most common Hive question on StackOverflow is how to do argmin and argmax. Usually there's a lot of other complex thinking wrapped around the problem, but most of the time when someone is having a hard time writing a tricky query, it boils down to them not knowing how to do argmin or argmax.

It's funny because this is a feature that Hive developers consciously chose not to implement. It would not be hard to simply add these two as "built-in" UDFs. But they haven't been added because there is a simple, but totally non-obvious work-around, as explained in [this JIRA ticket][1], but summarized more clearly here.

The key is that Hive compares structs by the order of their fields. For instance, {a:1,b:2} is less than {a:2,b:1}. So if you want the value of column x that minimizes y, you can do:

~~~SQL
min(named_struct('y', y, 'x', x)).x
~~~

As an example, let's say you have a table with three columns: customer_id, store_id, amount_spent. You want to know, for each store, which customer spent the most. The query would be:

~~~SQL
select store_id, max(
    named_struct(
        'amount_spent', amount_spent,
        'customer_id', customer_id
    )
).customer_id
from customer_spending
group by store_id
~~~

This seems like a good place to use macros, which were added in Hive 0.12, but unfortunately those are strictly-typed, so you'd have to have one for every pair of different types, and they also don't support UDAFs, only UDFs, so you can't include "min" or "max" in one. Oh well, maybe macros will get some love in a future release.

 [1]: https://issues.apache.org/jira/browse/HIVE-1128
