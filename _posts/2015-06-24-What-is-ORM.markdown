---
layout: post
title:  "What is an ORM"
date:   2015-06-24 4:50:39
categories: ruby sqlite html sinatra ORM
---
#What is an ORM#
ORM is Object Relational Mapping, it's a foundation to convert data between two systems that don't interact with eachother in my case Ruby and SQLite3. ORMs typically involve 4 main functions which are CRUD's functions that Create, Read, Update or Destory the information. The database I use, sqlite3 is limited on how it can manipulate or change information. It can filter, organize and give back data simply. It needs to be told when and how to update or delete data. 

**it doesn't have to have**
modules. 

##My ORM Solution##


**How it helps get all of a table's rows from the database** 
I have a module that can be used across classes to access all rows of a table by making a connection to the database, then asking to return all items from a specific table.
**For example this module:**
```ruby
  #method called all to return all rows from a table as objects
  def all
    # Figure out the table's name from the class we're calling the method on.
    table_name = self.to_s.pluralize.underscore
    
    #set variable results to what the database returns as an array of hashes
    results = CONNECTION.execute("SELECT * FROM #{table_name}")

    #creates an empty array called results as objects
    results_as_objects = []
  
    #take results variable (each of them) and loop through the hash
    results.each do |result_hash|
    
    #in the empty aray put objects from the hash
      results_as_objects << self.new(result_hash)
    end
  
    #guarenteeing the method returns the array of objects
    return results_as_objects
  end
```
 
**How it returns the information I ask for**
I have several methods that call for specific peices of information. I routinely call for a row of information for a certain id and also call for rows with a certain e-mail.
**For example this module:**
```ruby
  #method called find that requires id peramiter (intiger form)
  #this will always return one row or none because it looks up by id which is a primary key
  def find(id)
    # Figure out the table's name from the class we're calling the method on.
    table_name    = self.to_s.pluralize.underscore
    
    #set a variable results to the array of hash that the connection to the database returns
    results = CONNECTION.execute("SELECT * FROM #{table_name} WHERE id = #{id}")
      # results has nothing in it
      if results.empty?
        # return nil (this uses the falsey logic in other methods)
        return nil
      #otherwise if results has something  
      else
        #set result_hash to the first hash in the array of results 
        result_hash = results.first
        #create a new instance of the class that's using this method
        self.new(result_hash)
      end
      
  end
```
 
**The conversions I have it make to the data**
Currently the conversions are mainly to change the array of hashes I receive into objects.  
**For example this section of the all method**
```ruby
    #creates an empty array called results as objects
    results_as_objects = []
  
    #take results variable (each of them) and loop through the hash
    results.each do |result_hash|
    
    #in the empty aray put objects from the hash
      results_as_objects << self.new(result_hash)
    end
  
    #guarenteeing the method returns the array of objects
    return results_as_objects

```
