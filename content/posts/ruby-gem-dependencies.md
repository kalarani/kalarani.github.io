+++
date = '2011-05-06T14:39:07+05:30'
title = 'Ruby - Gems - Manage dependencies'
+++

Had a chance to work on a Ruby on Rails project recently. It is almost impossible to have a ruby application without using any gems. Gem is a packeged ruby application or library which can be used in other applications (equivalent to a jar in a java project). You can specify the gem dependencies for your application in the Gemfile. The [bundler](http://gembundler.com/) takes care of resolving the gem dependencies of the application for you. It makes sure that the same version of gems are used in all the machines/environments wherever you develop/deploy the application.

Still, you could run into the problem of nested dependencies not being resolved, as I did. My application was dependent on a gem (say Gem-A), which was dependent on another gem (say Gem-B). You'll naturally expect the bundler to resolve this and install Gem-B for your app. But the bundler failed to resolve this in my case.

Let's take a small peek into how gems are built. Gems are ruby projects in themselves. In case of  a gem, you have another place (apart from the Gemfile) where its gem dependencies should be specified. Enter the gemspec file. The gemspec file contains the specification of the gem, like the name, version, authors. A sample gemspec file would look like this:

```ruby
Gem::Specification.new do |s|
  s.name              = 'my_gem'
  s.version           = '1.0.0'
  s.authors           = ["Me", "Myself"]
.
.
.
  s.add_runtime_dependency      'activesupport',  '~> 3.0'
  s.add_development_dependency  'rails',          '~> 3.0'
  s.add_development_dependency  'rspec',          '~> 2.0'
  s.add_development_dependency  'rake'
  s.add_development_dependency  'sqlite3'
end
```

If any application is dependent on 'my_gem', the bundler will look at the runtime dependencies from the my_gem.gemspec file and it'll install the 'activesupport' gem as well, since it is a nested dependency.

Coming back to the problem in hand, the root cause was in the way Gem-A has specified its dependencies. It has added Gem-B as a dependency in its Gemfile, but hasn't specified it as a runtime dependency in its gemspec file and hence the bundler cannot identify Gem-B as a dependency. To resolve this, I had to add Gem-B as a dependency to my application as well.

It is a good practice to specify your runtime and development dependencies in gemspec and include it in your Gemfile, while developing gems. Your Gemfile will look something like:

```ruby
source 'http://rubygems.org'
gemspec
gem 'ruby-debug',   :platform => :ruby_18
gem 'ruby-debug19', :platform => :ruby_19
```

This will save a lot of time for the developers who use your gem.
