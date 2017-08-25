#Ruby on Rails

###Useful commmands

	Create a new project  
	rails new *project_name* -d postgresql
	
	rails g scaffold_controller *controller_name*
	
	rake db: drop
	rake db: create
	rake db: migrate
	rake db: seed
	
	rails g scaffold
	
	rails s - starts up server

##Walkthrough

1) Make a new rails app:
  
		rails new *name* -d postgresql
		
Once your app is made you will see all your folders and files that have been set up.

**Routes**  
2) Setting up restful routes in your routes.rb:  

		resources :*resources_name*
		
**Controller**  
3) Create a controller in **app/controllers/name_controllers.rb**
In your new file include the class name which will inherit from the *ApplicationController*

**Views**   
4) Create your views in **app/views/new_folder/view_name.html.erb**


5) Add your restful route methods in controller e.g

	def index
	end

	def show
	end
	
6) **Index** view   
**In view:**   
Add your each loop in the view and print out all the items in your array/database.  

	<h1>Simpsons Characters</h1>

	<ul>
	    <% @characters.each do |character| %>
	        <li>
	            <h2><%= character.name %></h2>
	            <p><%= character.job %></p>        
	            <p><%= character.voice_actor %></p>        
	        </li>
	    <% end %>
	</ul>
	
	<%= link_to 'New Character', new_character_path %>
	
**In controller:**

	def index
		@characters = Character.all
	end


7) Create your model 
 
		touch app/models/character.rb

8) Create your data to migrate: 
 
		rails generate migration createCharacters name age:integer job voice_actor
		
9) Create your database

		rake db:create

10) Migrate your data

		rake db:migrate

**New**   
**In view**  
11) Create your form:

	<h1>New Characters</h1>

	<%= form_with(model: @character) do |form| %>

		<div class="field">
			<%= form.label :name %>
			<%= form.text_field :name, id: :character_name %>	
		</div>

		<div>
			<%= form.label :age %>
			<%= form.number_field :age, id: :character_age %>
		</div>

		<div class="field">
			<%= form.label :job %>
			<%= form.text_field :job, id: :character_job %>
		</div>

		<div class="field">
			<%= form.label :voice_actor %>
			<%= form.text_field :voice_actor, id: :character_name %>
		</div>

		<div class="field">
			<%= form.label :first_appearance %>
			<%= form.date_select :first_appearance, id: :character_first_appearance %>
		</div>

		<div class="field">
			<%=form.submit %>
		</div>
	<% end %>


**In controller:**

	def new
		@character = Character.new
	end

**Create**  
**In Controller**  

	@character = Character.create(params[:character])
		@character.save
		redirect_to @character
 
**Show**  
**In view**  

	<h1>Show</h1>

	<h2><%= @character.name %></h2>
	<p><%= @character.job %></p>
	<p><%= @character.voice_actor %></p>
	<p><%= @character.age %></p>
	<p><%= @character.first_appearance %></p>
	
	<%= link_to 'Destroy', @character, method: :delete %>
	<%= link_to 'Edit', edit_character_path(@character) %>
	
**In Controller**

	def show
		@character = Character.find(params[:id])
	end
	
**Edit**  
**In Views**

Copy in your **new** form into the edit view

**In Controller**
	
	def edit
		@character = Character.find(params[:id])
	end
	
	
**Update**
**In controller**

	def update

		@character = Character.find(params[:id])
		@character.update(character_params)
		redirect_to @character

	end
		
**Delete**  

**In Controller**

	def destroy

		@character = Character.find(params[:id])
		@character.destroy
		redirect_to characters_path

	end


