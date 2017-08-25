#Building a Ruby app

##Folder Structure 

- Project
	- controllers
		- main_controller.rb
		- static_controller.rb
	- models
		- model.rb
	- public
		- css
		- images
		- js
	- views
		- partial
			- navigation.erb
		- posts
			- new.erb
			- index.erb
			- show.erb
			- edit.erb
		- layout.erb
	- config.ru
	- seed.sql

##Require your gems
> In your config.ru file require the gems you will need. Using bundle init in the terminal will create a new gem file in your project folder. *bundle* will make sure that the relevant gems are downloaded.

##Create your Database

> In the terminal enter psql to start.
> The command CREATE DATABASE *name*

*\q* to quit  
*\c database* to connect

##Setting up seed.sql
Example below

	DROP TABLE IF EXISTS *table_name*;
	
	CREATE TABLE *table_name* (  
	*data types*  
		id SERIAL PRIMARY KEY,  
		title VARCHAR(255),  
		description TEXT  
	);  
	INSERT INTO *table_name* (title, description) VALUES ('Position 1', 'A description 1');
	

In the terminal use \c to connect to your database and use  
**psql -d *database_name* -f seed.sql**   
to seed your database.

##Restful routes
These are set up in your controller

- INDEX - get "/"
- SHOW - get "/:id"
- NEW - get "/new"
- CREATE - post "/"
- EDIT - get "/:id/edit"
- UPDATE - put "/:id"
- DELETE - delete "/:id"

##Setting up a Model
You model is the section which will interact with the database and do all the heavy lifting and logic.

Set up your class and include a method to allow connection to the database. e.g

	def self.open_connection  
		PG.connect( dbname: "*database_name*")  
		end
	
###Hydrating data

- Create a new instance of the class
- set *instance*.*field* = *instance*["*field*"]
- return *instance*

###INDEX Getting all data - self.all
- Open connection with database
- Find all data using SQL
- Hydrate data and return

example

	conn = self.open_connection  
			sql = "SELECT * FROM position ORDER BY id"  
			result = conn.exec(sql)
	
	position = result.map do |position|  
			self.hydrate position  
			end  
			position
		
###SHOW Showing data by specific id
- Open connection
- Find data by id using SQL
- Hydrate data and return

		def self.find id  
			conn = self.open_connection  
			sql = "SELECT * FROM positions WHERE id =#{id}"  
			result = conn.exec(sql)  
		   	self.hydrate result[0]
	   	end

###NEW Saving data
- Open connection
- Insert data using sql
- Get result  
- *Connection is opened on the class rather than on self*

		def save
				conn = Position.open_connection
				sql = "INSERT INTO positions(title, image, description, rating) VALUES ('#{self.title}','#{self.image}','#{self.description}','#{self.rating}')"
				result = conn.exec(sql)
		
			end

###UPDATE Updating data in database
- Open a connection with the database
- Update data using sql command
- Get result

		def update
				conn = Position.open_connection
				sql = "UPDATE positions SET title='#{self.title}', image ='#{self.image}', description='#{self.description}', rating='#{self.rating}'WHERE id = #{self.id}"
				result = conn.exec(sql)
		
			end

###DELETE Deleting entries in the database
To delete an entry we need the following:
- Create a connecion with the database
- Delete entry using the sql command
- Display results

	def self.destroy id
			conn = self.open_connection
			sql = "DELETE FROM table_name WHERE id = #{id}"
			result = conn.exec(sql)
		end


##Setting up views
**Please note that if you have layout.erb file this will need to be set up in order to see the views you have set up.**

###Index
This page will display all of our data in the database
Using the each method in Ruby we can go through each entry and print. Example below.

	<% @positions.each do |position| %> 
	<div> 
		<h3><%= position.title %></h3>
		<p><%= position.description %></p>
		<p>Difficulty rating: <%= position.rating %></p>
	</div>  
	<% end %>

Your instance variable created in index which will equal the all of the data found. Example:
*@positions = Position.all*
>**SQL "SELECT * FROM positions ORDER BY id"**

###Show
This page will show a specific item based on the item's id. This wuses the instance variable (@position) of your class?:

	<h3><%= @position.title %></h3>
	<img src="<%= @position.image %>">
	<p><%= @position.description %></p>
	<p>Difficulty rating: <%= @position.rating %></p>
	

The instance variable created in show will equal the data found with a specific id. 
*@position = Position.find id*

###New
This page will have a form and allow a new entry to to made to the database.

	<form action="/" method="POST">
	
	<label>Title</label>
	<input type="text" name="title">
	
	<label for="body">Body</label>
	<textarea name="body"></textarea>
	
	<input type="submit" name="Create Post">

###Create
- Create a new instance of class
- set class to equal the data collected in the field
- *e.g position.title = params[:title]* 
- use the save method on the class which will insert the fields into the database
- Redirect to home

		position = Position.new

		position.title = params[:title]
		position.image = params[:image]
		position.description = params[:description]
		position.rating = params[:rating]

		position.save

		redirect :"/"
		
###Edit
This page will allow the user to edit a particular item using a form. Example below

	<form action="/<%= @post.id %>" method="POST">
		<input type="hidden" name="_method" value="PUT">
		
		<label>Title</label>
		<input type="text" name="title" value="<%=@post.title %>">
	
		<label for="body">Body</label>
		<textarea name="body"><%= @post.body %></textarea>
	
		<input type="submit" name="Update Post">
	</form>
	
In the controller do not forget to set the id and create and instance variable to equal a specific item.

		id = params[:id]

		@position = Position.find id

###Update
To update changes we need to do the following:
- Find the right item using the ID
- Set information in class to equal info captured in the form 
- Use the update method on item

		id = params[:id].to_i

		position = Position.find id

		position.title = params[:title] 
		position.image = params[:image]
		position.description = params[:description]
		position.rating = params[:rating] 

		position.update

		redirect  :"/"
		
###Delete
To delete, we call the method destroy id onto the instance of the class?

		id = params[:id].to_i

		Position.destroy id

		redirect :"/"








