+++
date = '2012-03-16T21:40:15+05:30'
title = 'Missing Data During Tire Indexing'
+++
Am working on a RoR project, which uses Postgres for data storage and [ElasticSearch](http://elasticsearch.org/) (ES) for providing the search functionality. We were using [Tire](https://rubygems.org/gems/tire) to talk to the ES server. Ran into an issue recently, where in the search results didn't return a particular document.  For the purpose of this post lets consider a model like the one defined below.

```ruby
class Document < ActiveRecord::Base
# t.integer id
# etc., etc.,
end
```
The particular record, that i was looking for, was available in the DB, but was not available in the ES index. So, the first and foremost thing that i did is to re-build the index. Tire provides you with a rake task to import the indices. So, I can do

```ruby
rake tire:import CLASS=Document FORCE=true
```

to rebuild my indices. Did that and tire reported that it has indexed all the records from the database to ES. But the document that i was looking for is still not returned in the search results.

Suspected ES to be the culprit and
1) Checked the number of documents indexed and found that the number of records, that tire reported to have indexed, and the actual number of documents indexed did not match.
2) Grep-ed the ES data folder for the unique identifier of the missing document and it was nowhere to be found in those folders.
Read some more in the Tire documentation and found that every record in the database can be indexed individually. Enter rails console.

```ruby
Document.find('The missing document').update_index
```

I was expecting this to fail and throw some exception to give me some clue in this debugging process. But to my surprise it was successful and the document was returned in the search results. Digging the tire documentation for some more time tells you that you can also do

```ruby
Document.index.import Document.all
```

to index all the documents from DB to ES. Again, I was expecting this to give me the same results as the rake task, because ideally they are doing the same thing. I was in for a surprise again. The document still came up in the search results. This is when I started suspecting the rake task and looked up the tire source code to see what it actually does. Ignoring the code for progress bar and other things, only the three steps are of interest to me.

```ruby
index = Tire::Index.new(klass.tire.index.name)
index.delete
index.create :mappings => klass.tire.mapping_to_hash, :settings => klass.tire.settings
index.import(klass, 'paginate' {}) do |documents|
      documents
end
```

So, I tried doing the same thing in the rails console, by just replacing the 'klass' with 'Document'. And the document went missing again. But this time it gave some visibility into what is happening internally. When we ask tire to rebuild the indices, it doesn't get all the documents in one go and build the index. It fetches the documents page by page (with a max of 1000 documents per page) by calling the 'paginate' method, and the paginated results had duplicate records across the pages.

We were using 'kaminari' in our project to take care of the pagination and hence we had to inject this method for the models that needs indexing and it looked like:

```ruby
module Paginatable
   module KlassMethods
      def paginate(options)
         page(options[:page]).per(options[:per_page])
      end
   end
   
   def self.included(klass)
      klass.extend KlassMethods
   end
end
```

I just had to change it to include some default ordering, to make sure that tire gets all the records (without any duplicates) for indexing.

```ruby
def paginate(options)
    order("#{table_name}.id").page(options[:page]).per(options[:per_page])
end
```
