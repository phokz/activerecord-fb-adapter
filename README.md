# ActiveRecord Firebird Adapter
[![Gem Version](https://badge.fury.io/rb/activerecord-fb-adapter.svg)](http://badge.fury.io/rb/activerecord-fb-adapter)
[![Build Status](https://travis-ci.org/rowland/activerecord-fb-adapter.png?branch=master)](https://travis-ci.org/rowland/activerecord-fb-adapter)
![Downloads](https://img.shields.io/gem/dt/activerecord-fb-adapter.png)

<img src="/project_logo.png" align="left" hspace="10">
This is the ActiveRecord adapter for working with the [Firebird SQL Server](http://firebirdsql.org/). It currently supports Rails 3.2.x and 4.x. Although this adapter may not yet have feature parity with the 1st tier databases supported by Rails, it has been used in production by different people without issues and may be considered stable. It uses under the hood the [Ruby Firebird Extension Library](https://github.com/rowland/fb).

## What's supported

- Datatypes: string, integer, datetime, boolean, float, decimal, text (blob).
- Rails migrations and db/schema.rb generation.
- Linux, OS X, and Windows supported.

## Getting started

1) Install and start the Firebird Server in your machine (varies across operating systems).

2) Create a new Rails project.

```
rails new firebird_test
cd firebird_test
```

3) Edit the project Gemfile and add the **activerecord-fb-adapter** gem:

```ruby
gem 'activerecord-fb-adapter'
```

Then run:

```
bundle install
```

Bundler will install the gem and it's dependency, Fb, which is "native" (has C code) and will be compiled the first time. Be sure you have a Firebird installation with access to the "ibase.h" file for this to succeed.

4) Edit the **database.yml** for configuring your database connection:

```ruby
development:
  adapter: fb
  database: db/development.fdb
  username: SYSDBA
  password: masterkey
  host: localhost
  encoding: UTF-8
  create: true
```

The default Firebird administrator username and password are **SYSDBA** and **masterkey**, you may have to adjust this to your installation.

With the "create: true" option, a database will be created the first time the adapter tries to connect if it doesn't exist. Alternatively, if you're using Rails 4 or greater, you can use the "rake db:create" task.

5) Start the rails server in development mode

```
bundle exec rails server
```

Open your browser at http://localhost:3000, this will create the database on the first connection.

On Linux you may get:

```
Fb::Error (Unsuccessful execution caused by a system error that precludes successful execution of subsequent statements
I/O error during "open O_CREAT" operation for file "db/development.fdb"
Error while trying to create file
Permission denied
```

This is because, by default, the Firebird Server runs under the "firebird" user and group, which has no write access to the "db" folder of the project. To fix it run:

```
chmod o+w db
```
which will add write permission to "others" group.

6) Now you can start generating scaffolds or models and rails will create the corresponding migrations. Use **bundle exec rake db:migrate** and **bundle exec rake db:rollback** for migrating the database; this will update your **db/schema.rb** file automatically.

## License
It is free software, and may be redistributed under the terms specified in the MIT-LICENSE file.
