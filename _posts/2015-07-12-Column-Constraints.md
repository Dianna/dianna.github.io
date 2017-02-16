---
layout: post
title: "Column Constraints and Foreign Keys"
published: true
---

### The Weird Problem
Coming into a new project recently, I decided to handle the initial database creation for my team. Our project required relational data, so we went with MySQL, using Bookshelf.js with Knex.js as the ORM. I'd had previous experience with MySQL, but less with Bookshelf.js. I quickly ran into a problem that I just couldn't solve. For the life of me I could not use the primary key of one table as the foreign key in another and without that, my relational database would have no relations. As you have likely come to realize, this problem was not supposed to be the difficult to solve problem I'd forseen in the project. Rather it had crept, insidiously into my database and stalled further progress. Here's a simplified version a users and transactions table:

Broken Code:
{% highlight javascript %}
db.schema.createTable('users', function(user){
  user.increments('id').primary();
});
db.schema.createTable('transactions', function(transaction){
  transaction.increments('id').primary();
  transaction.integer('buyer_id', 11).references('users.id').notNullable();
  transaction.integer('seller_id', 11).references('users.id').notNullable();
});
{% endhighlight %}

In the model files I defined the two tables one to many and many to one relationship respectively, but the foreign key columns of the transactions table still showed no relation to the users table. After an hour of grinding on the problem another programmer suggest I try adding .unsigned() to the foreign key columns. He said this had worked for him but he didn't know why. Sure enough, it worked for me to... but why?

### The Solution
Fixed Code:
{% highlight javascript %}
db.schema.createTable('users', function(user){
  user.increments('id').primary();
  user.string('username', 20).notNullable();
});
db.schema.createTable('transactions', function(transaction){
  transaction.increments('id').primary();
  transaction.integer('buyer_id', 11).unsigned().references('users.id').notNullable();
  transaction.integer('seller_id', 11).unsigned().references('users.id').notNullable();
{% endhighlight %}

As I researched, I found that people had similar issues with pure MySQL tables as a result of differing constraints on the primary id and foreign key columns of two tables. On a hunch I switched over to Sequel Pro after changing back to the broken code and found that the 
{% highlight javascript %}
user.increments('id').primary();
{% endhighlight %}
line of code was marked as unsigned, even though I hadn't explicitly written that into my code, which was why the 
{% highlight javascript %}
transaction.integer('buyer_id', 11).references('users.id').notNullable();
{% endhighlight %} 
line of code could not reference the former- it was signed.

I have not as of yet been able to find any good documentation on whether Bookshelf.js automatically marks incrementing columns as unsigned or integer columns as signed, but hopefully this saves someone an unnecessary hour grinding on an easily fixed problem.
