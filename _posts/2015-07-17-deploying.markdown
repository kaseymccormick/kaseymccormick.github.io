---
layout: post
title:  "Deploying"
date:   2015-06-18 09:10:39
categories: ruby deploying tutorial
---

##The assignment##

##My Solution - Song inventory Management##

**Additional features planned** 
 
###User Abilities###

###Some of the logic###


deploying -> taking our application and putting it on to a server elsewhere that we can access publicly via a url.

###steps###
*rackup* - Heroku has no way of knowing what ruby file to run
  in your projects root directory add config.ru
  require './app.rb'
  run Sinatra::Application
  
  in terminal typer: rackup
  
  * can access same application with diferent port
  
  
*database* - heroku uses Postgresql so you need to define different databases for development and for production

add into app.rb 
> configure:development do
ActiveRecord::Base.establish_connection(adapter:'sqlite3', database:_
> end

PUT IN APP>RB
production insert code
configure :production do 
db = URI.parse(Env['DATABASE_RL'])
.....
code copied from heroku's website

database gems
gem sqlite3 (identify invironments used)
gem pg (identify)

Code in slide.slack
EVERY TIME YOU UPDATE GEM FILE RUN BUNDLE INSTALL


heroku doesnt give a database every time you have to ask for it
via heroku addons:create heroku-postgresql
(is a limt of databases you can have per project which is one, a limit on how many rows of data you can have in that database. if exceed then pay for higher teir)
*then git push heroku master*

