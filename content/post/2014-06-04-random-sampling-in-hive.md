---
title: Random sampling in Hive
author: joefkelley
layout: post
date: 2014-06-05
url: /736
sfw_pwd:
  - CZoEg2VZ1zEx
categories:
  - Programming

---
Let's say you have a Hive table with ten billion rows, but you want to efficiently randomly sample a fixed number- maybe ten thousand. The most obvious (and obviously wrong) way to do this is simply:

~~~SQL
select * from my_table
limit 10000;
~~~

Hive makes no guarantees about the order of records if you don't sort them, but in practice, they come back in the same order in which they're in the file, so this is far from truly random. As a next attempt:

~~~SQL
select * from my_table
order by rand()
limit 10000;
~~~

This does actually give you truly random data, but performance is not so good. In order to impose a total ordering, Hive must force all data to one single reducer. That reducer will sort the entire dataset. Not good. Luckily, Hive has a non-standard-SQL "sort by" clause that only sorts within a single reducer, and gives no guarantees that data will be sorted across reducers:

~~~SQL
select * from my_table
sort by rand()
limit 10000;
~~~

This is much better, but I'm not convinced it's truly random. The issue is that Hive's method of splitting data into multiple reducers is undefined. It might be truly random, it might be based on file order, it might be based on some value in the data. How Hive implements the limit clause across reducers is also undefined. Maybe it takes data from the reducers in order- i.e., all data from reducer 0, then all from reducer 1, etc. Maybe it round-robins through them and mixes everything together.

In the worst case, let's say the reduce key is based on a column of the data, and the limit clause is in order of reducers. Then the sample will be extremely skewed.

The solution is another non-standard Hive feature: "distribute by". For queries where the reduce key is not determined by the structure of the query (no "group by", no joins), you can specify exactly what you want the reduce key to be. If we distribute randomly, and sort within each reducer randomly, then it doesn't matter how "limit" functions.

~~~SQL
select * from my_table
distribute by rand()
sort by rand()
limit 10000;
~~~

Finally, as one last optimization, we can do some filtering map-side. If the total size of the table is known, then you can easily randomly select some proportion of the data on a record-by-record basis, like this:

~~~SQL
select * from my_table
where rand() <= 0.0001
distribute by rand()
sort by rand()
limit 10000;
~~~

In this case, since the total size is ten billion, and the sample size is ten thousand, I can easily calculate that's exactly 0.000001 of the total data. However, if the where clause was "rand() < 0.000001", it would be possible to end up with fewer than 10000 rows output. "rand() < 0.000002" would probably work, but that's really relying on a very good implementation of rand(). Safer to fudge it by a bit more. In the end it doesn't matter much since the bottleneck quickly becomes the simple full table scan to start the job, and not anything based on the volume sent to reducers.
