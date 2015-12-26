---
layout: post
title: "Builidng a CLI Using Ruby Commander"
date: 2015-12-25T16:38:15+08:00
categories: [ruby]
---

This is a tutorial on using Ruby and Commander to build a command line interface (CLI) app.

## Gemfile

Create the project directory and add a Gemfile.

```
mkdir ruby-cli
cd ruby-cli
touch Gemfile
```

The `Gemfile` has only 1 gem - [commander](https://github.com/tj/commander):
```
source "http://rubygems.org"
gem "commander"
```

Install with `bundle install`. If you don't have [bundler](http://bundler.io), then install with `gem install bundler` first.


## The Code

This is the code for our simple `ruby-cli.rb`:

```ruby
#!/usr/bin/env ruby

require 'rubygems'
require "bundler/setup"
require 'commander/import'

program :name, "Ruby CLI"
program :version, '1.0.0'
program :description, 'A simple cli that does nothing'

command :extract do |c|
		c.syntax = 'ruby-cli extract foo -bar=123'
		c.description = 'Extract foo and bar'
		c.option '--bar STRING', String, 'Option bar with a string'
		c.action do |args, options|
				options.default :bar => 'BAR'
				puts 'Extracting..'
				puts "Args: #{args}"
				puts "Option Bar: #{options.bar}"
		end
end
```

We can now run `ruby ruby-cli.rb extract`. But we can simply run `./ruby-cli.rb extract`, since the first line declared the ruby shebang. All we need is to `chmod a+x ruby-cli.rb` so that the file has executable permission.

Using `commander`, we have described our CLI easily. Run `./ruby-cli.rb --help`, and it will provide a nice help on all the commands that is possible. In our code, we only have `extract` command.

In our `extract` command, we print out the args and options.

There is only 1 option `--bar`, with the default string "BAR". 


## Ruby Gem

Till now, you can ruby `./ruby-cli.rb ...`. 

But we could install it as a gem, and run `ruby-cli ...`.

To do that, move the script to `bin`, and without the extension

```
mkdir bin
mv ruby-cli.rb bin/ruby-cli
```

Next we create the file `ruby-cli.gemspec`.

```
Gem::Specification.new do |s|
  s.name = 'ruby-cli'
  s.version = '1.0.0'
  s.date = '2015-12-25'
  s.summary = "Our simple CLI"
  s.description = "Our very very simple CLI"
  s.authors = [ "Luke Skywalker" ]
  s.email = 'lukeskywalker@gmail.com'
  s.executables << 'ruby-cli'

  s.add_dependency 'commander'
end
```

To build the gem:

	gem build ruby-cli.gemset

To install the gem:

	gem install ruby-cli-1.0.0.gem

Finally, you can run `ruby-cli extract foo -bar=1213`!

_PS: This is my first tutorial with using Ruby. I have been fighting against using Ruby for many times, since languages like Python and Go are great, and I am sure Swift will be usable for scripting some day too. But, so many existing tools use Ruby, that I can't avoid it. Two of my favorites - [Jekyll](https://github.com/jekyll/jekyll) and [fastlane](https://github.com/fastlane/fastlane)._

