+++
date = '2011-11-04T17:42:41+05:30'
title = 'Integration testing Search - Clash of the indices'
+++

[ElasticSearch](http://elasticsearch.org/) is an Open Source (Apache 2), Distributed, RESTful, Search Engine built on top of [Apache Lucene](http://lucene.apache.org/). It promises to solve all the pain points of implementing search in a web application. [Tire](https://rubygems.org/gems/tire) is one of the popular ruby clients for elasticsearch. Though it is important to have integration test for models while using tire in a ruby app, it is equally important to make sure that the test doesn't corrupt the development indices.

One approach to overcome this is to create a dummy class in the test that includes the `Tire::Model::Search` and `Tire::Model::Callbacks` module and use it to create indices and test search in the integration test. While this seems simple, there is a cleaner approach, that lets us use the actual models in test and does not corrupt the development indices as well. There is a configuration in tire that lets you provide the prefix to the index name. By specifying the

```ruby
Tire::Model::Search.index_prefix Rails.env
```

in the `initializers/tire.rb` file, we can tell tire to use different indices for different environments. For example, the search in the dev mode will use `development_model_name` index and that in the test environment will use `test_model_name` index.
