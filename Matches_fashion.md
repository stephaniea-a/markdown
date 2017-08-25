#Matches Fashion

###Sign in link
id=login

##Current Customer

Email
id = j_username

Password
id = j_password

##Create Account

Title
id = register.title
class cs__selected

Firstname  
id = register.firstName

Lastname
id = register.lastName

Email
id = register.email

Password
id = pwd
name = pwd

Confirm Password
id = register.checkPwd

Countries
id = address.country
class = default-opt

Mobile
id = register.phone

Create account button
type ="submit"

Checkbox
class = custom__check

###Methods for SignInPage
def get_element(target)
	@browser.element(target)
end

def input_fields(target)
	@browser.text_fields(target)
end

def click_button(target)
	@browser.button(target).click
end

def click_link(url)
	@browser.link(href: url)
end


##Tests

require'spec-helper'


Describe Matches do
	
	before(:all) do 
		@matches = Matches.new
	end

	context 'creating a new user account' do 

		it '' do
		end

	end

	context 'signing in as'


end

