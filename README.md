### This is one of the worst performing Rails apps ever.

Currently, the home page takes this long to load:

```bash
Rendered author/index.html.erb within layouts/application (12392.4ms)
Completed 200 OK in 12677ms (Views: 6560.1ms | ActiveRecord: 6108.7ms)
```

The view takes 6 seconds to load. The AR querying takes 6 seconds to load. The page takes 12 seconds to load. That's not great.

What can we do?

Well, let's focus on improving the view and the querying!

Complete this tutorial first:
[Jumpstart Lab Tutorial on Querying](http://tutorials.jumpstartlab.com/topics/performance/queries.html)



### Requirements
* add an index to the right columns
* implement caching
* implement eager loading vs lazy loading on the right pages.
* replace ruby lookups with ActiveRecord methods.


##### Ruby vs ActiveRecord

Let's try to get some ids from our Article model.

Look at Ruby:
```ruby
puts Benchmark.measure {Article.select(:id).collect{|a| a.id}}
  Article Load (2.6ms)  SELECT "articles"."id" FROM "articles"
  0.020000   0.000000   0.020000 (  0.021821)
```

The real time is 0.021821 for the Ruby query.

vs ActiveRecord

```ruby
puts Benchmark.measure {Article.pluck(:id)}
   (3.2ms)  SELECT "articles"."id" FROM "articles"
  0.000000   0.000000   0.000000 (  0.006992)
```

The real time is 0.006992 for the AR query. Ruby is about 300% slower.