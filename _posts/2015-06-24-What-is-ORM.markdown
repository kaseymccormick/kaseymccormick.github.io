---
layout: post
title:  "What is an ORM"
date:   2015-24-16 14:50:39
categories: ruby sqlite html sinatra ORM
---
#What is an ORM#
ORM is Object Relational Mapping, it's a foundation to convert data between two systems that don't interact with eachother in my case Ruby and SQLite3. ORMs typically involve 4 main functions which are CRUD's functions that Create, Read, Update or Destory the information. The database I use, sqlite3 is limited on how it can manipulate or change information. It can filter, organize and give back data simply. It needs to be told when and how to update or delete data. 

**it doesn't have to have**
modules. 

##My ORM Solution##


**How it helps get all of a table's rows from the database** 
I have a module that can be used across classes to access all rows of a table by making a connection to the database, then asking to return all items from a specific table.
**For example:**
```ruby
  def all
    # Figure out the table's name from the class we're calling the method on.
    table_name = self.to_s.pluralize.underscore
  
    results = CONNECTION.execute("SELECT * FROM #{table_name}")

    results_as_objects = []
  
    results.each do |result_hash|
      results_as_objects << self.new(result_hash)
    end
  
    return results_as_objects
  end
```
 
**How it returns the information I ask for**
I have several methods that call for specific peices of information. I routinely call for a row of information for a certain id and also call for rows with a certain e-mail.
**For example:**
```ruby
  def find(id)
    # Figure out the table's name from the class we're calling the method on.
    table_name    = self.to_s.pluralize.underscore

    results = CONNECTION.execute("SELECT * FROM #{table_name} WHERE id = #{id}")
     
      if results.empty?
        return nil
      else
        result_hash = results.first
        self.new(result_hash)
      end
      
  end
```
 
**The conversions I have it make to the data**
Currently the conversions are mainly to change the array of hashes I receive into objects.  
**For example**
```ruby
  def all
    # Figure out the table's name from the class we're calling the method on.
    table_name = self.to_s.pluralize.underscore
  
    results = CONNECTION.execute("SELECT * FROM #{table_name}")

    results_as_objects = []
  
    results.each do |result_hash|
      results_as_objects << self.new(result_hash)
    end
  
    return results_as_objects
  end
```
