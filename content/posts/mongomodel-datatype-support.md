+++
date = '2011-12-29T08:34:28+05:30'
title = 'Datatype support for MongoModel'
+++

[Mongomodel](https://github.com/spohlenz/mongomodel) is a ruby gem that does the Data mapping for mongo db. It supports the most commonly used data types. Recently our team had a requirement to handle (persist and retrieve back data from mongodb) OpenStruct. Since the mongomodel documentation doesn't provide enough information on how to extend it to support a new datatype, we tried to give it a shot ourselves and to our surprise it was much easier and straight forward than what we thought.

All that mongomodel expects is to have a class (converter) that can serialize and deserialize the datatype to be persisted in the mongodb. So, we'll start by defining the class with 'from_mongo' and 'to_mongo' methods in it.

```ruby
module MongoModel
   module Types
      class MongoOpenStruct
         def from_mongo(value)
            OpenStruct.new(value)
         end
         def to_mongo(value)
            value.marshal_dump
         end
      end
   end
end
```

Alright. Now we need to tell mongo to use this class if it comes across a OpenStruct property in any mongomodel document. Mongomodel uses an internal hash map to identify the converter for a given datatype. So, all that we need to do is to add an entry (just after the 'converter' class defnition) for OpenStruct in the mapping.

```ruby
Types::CONVERTER.merge! ::OpenStruct => Types::MongoOpenStruct.new
```

Thats it! We can now start using OpenStruct properties in any mongomodel document.

```ruby
class Article < MongoModel::Document
   property :some_property, OpenStruct
end
```
