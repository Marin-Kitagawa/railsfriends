# Ruby on Rails

# More Info

For documentation or for guides on Ruby on Rails go to [Ruby on Rails Guide](https://guides.rubyonrails.org) 

# Creating a controller via CLI

```ruby
rails g controller home index

# .erb = Embedded Ruby
**# Now we can go to http://localhost:3000/home/index**
```

In `routes.rb`, if you want to change the home page (root route path), you need to specify it like this

```ruby
# File : routes.rb
root 'home#index'

# We must specify it like this because we created the controlled in this form only (see above code block)
# Now http://localhost:3000/ will take you to the previous /home/index page. But the above URL won't work anymore.
```

To get all the routes in the app, run the command `rails routes`

# Creating a controlled MANUALLY

1. Create a file (say, `about.html.erb`) with extension `.erb` and save it in the `home` folder (because we created `home/index`
2. The in the `home_controller.rb` file, create a function (similar to `[views.py](http://views.py)` file in `django`) (say, `about`)

    ```ruby
    # home_controller.rb

    class HomeController < ApplicationController

    	# 	...
      
      def about
      end
      
    end
    ```

3. Finally, add the route in the `routes.rb` file.

    ```ruby
    get 'home/about'
    ```

# Adding Bootsrap

## Method 1

Copying the CSS and HTML from Bootstrap to the website we are designing.

## Method 2

Adding a `gem` in the `GemFile`

## Steps (method 1 only)

1. Copy the starter template from `Bootstrap` website and paste it in `application.html.erb` file above the `<html>` tag **WITHOUT MODIFYING ANYTHING THAT EXISTED PREVIOUSLY**
2. Now, from the old file code, replace the new `<h1>Hello, World!</h1>` with the `<%= yield %>` tag and copy all the configurations inside the `<head>` tag to the new one and change the title.

# Partials in RoR

1. Partial files are webpages that can be rendered in any of the other webpages depending on our necessity.
2. Partials **MUST**  begin with an underscore 
3. Partials can be included in other webpages in the following manner

    ```ruby
    # Partial file location /home/_header.html.erb  (underscore is a must for a partial)

    # Below file eg. application.html.erb

    <%= render 'home/header' %>

    # Note : No '_' is used within the tag <%= %>
    #====#
    # The '=' sign means we need RoR to output the page to the screen. 
    # We won't use the '=' sign while we are dealing with loops such as for etc.
    ```

# Link To tags

To link to a specific route in RoR, we use the `link_to` tag. It is used as follows:

1. For example, consider the text `Friends App` which is acting as a logo which must be linked to the homepage. 
2. We do the linking by
    1. Replace the `<a>` tag entirely (but retaining the class name) with the following

        ```ruby
        <%= link_to 'Friends App', root_path, class:"navbar-brand" %>

        # The Friends App is the anchor text used previously, root_path is the root path that we set in the routes.rb file
        # And the class is the class name of the <a> tag
        ```

    2. Do the same for `About Us` (replacing the `Link` from `Bootstrap`)

        ```ruby
        <%= link_to 'About us', home_about_path, class:"nav-link" %>

        # **NOTE: PATH IN link_to are denoted in the following manner
        # If the path is like this /a/b/c/d/e then it is converted to => a_b_c_d_e_path
        # We already had a variable called root in the routes.rb and hence we just used root_path
        # That is, in general, join the paths using _ and append _path at the end.**
        ```

    3. If you are not sure about what should be the path in the `link_to` tag, then run `rails routes` in the CLI and then use the prefix given for that page.

    # Scaffolds

    When we need a database with table(s) for storing data and performing CRUD operations (such as in blog posts etc), we use `scaffold` in RoR

    ### Steps

    1. Run the command `rails g scaffold <table_name> <column_1_name>:<column_1_type> <column_2_name>:<column_2_type>` etc.
    2. Example

        ```ruby
        rails g scaffold friends first_name:string last_name:string email:string phone:string twitter:string

        # [WARNING] The model name 'friends' was recognized as a plural, using the singular 'friend' instead. 
        # Override with --force-plural or setup custom inflection rules for this noun before running the generator.
        ```

    3. Even though RoR has created a migration, there will be no **schema**  `schema.rb` which is like a finished product i.e. pushing the migration into the DB.  That is making the DB live. This can be done by running the following command in the CLI

        ```ruby
        rails db:migrate
        ```

    4. Now, after this, the scaffold would have created a `friends` directory with many `HTML` pages and stylesheets. These stylesheets may conflict with our stylesheets. So, **delete** `scaffold.scss` file or delete its contents. 
    5. Now, you can access the route `[**http://localhost:3000/friends/**](http://localhost:3000/friends/)`. But it won't look good. There will also be an option for a `New Friend` which when clicked renders a form for us for creating the new "friend".
    6. If we create a new friend using the `New Friend` button, an option to `show, update, destroy` will also appear (built-in) and it will show the details of the just-now created friend with a success message and it will also be rendered on the page `[**http://localhost:3000/friends/**](http://localhost:3000/friends/)`
    7. Now, for ease of accessibility of the aforementioned URL, add to the Navbar. To do that we must know the route. Just run the `rails routes` command to get the prefix and add the `_path` suffix in the `link_to` tag.

    # Styling

    You can directly add styling to the html pages in the `friends` via the `friends.scss` and it will be reflected for all the pages in the `friends` directory.

    # Devise gem and User Management

    Go to [Ruby Gems](https://www.rubygems.org) and add `devise` gem to `GemFile` (one with the highest number of downloads $(> 89\text{ M})$

    Then run the command

    ```ruby
    bundle install
    ```

    Then go to the homepage of `devise` gem mentioned in the `Ruby Gems` page and then go to `Getting Started` 

    Run the command 

    ```ruby
    rails generate devise:install
    ```

    Then it will show the next set of instructions to be followed as

    ```html
    create  config/initializers/devise.rb
          create  config/locales/devise.en.yml
    ===============================================================================

    Depending on your application's configuration some manual setup may be required:

      1. Ensure you have defined default url options in your environments files. Here
         is an example of default_url_options appropriate for a development environment
         in config/environments/development.rb:

           config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

         In production, :host should be set to the actual host of your application.

         * Required for all applications. *

      2. Ensure you have defined root_url to *something* in your config/routes.rb.
         For example:

           root to: "home#index"

         * Not required for API-only Applications *

      3. Ensure you have flash messages in app/views/layouts/application.html.erb.
         For example:

           <p class="notice"><%= notice %></p>
           <p class="alert"><%= alert %></p>

         * Not required for API-only Applications *

      4. You can copy Devise views (for customization) to your app by running:

           rails g devise:views

         * Not required *

    ===============================================================================
    ```

    Now, after following all the above steps, go to the `devise` homepage and then follow the rest of the `Getting Started` instruction. \

    Now, you have to create a model (database) for users for sign in, registration etc.

    Run the following command for that

    ```ruby
    rails g devise <MODEL_NAME>
    ```

    Now, the DB is created and now, similar to the friends model, we need to push the migration. So, run the below command. It is also the next step mentioned in the homepage.

    ```ruby
    rails db:migrate
    ```

    Now, we need to access the views similar to friend views. So, run the command `rails routes` to find the prefix for the routes.

    Add separate navbar links for

    1. Sign Up
    2. Sign In
    3. Edit Profile
    4. Sign Out

    Copy all the route prefixes from the CLI and paste them to the respective navbar links

    Since all the methods will be either `GET` or `POST` there is no need to specify the method for first three. But for the `Sign Out`, the method is `DELETE`. So, we need to explicitly  mention the method before the `class`

    # Associations

    Now, we have two tables 

    1. User table
    2. Friends list

    Now, we need friends list for each user not a big one. So, we need to connect these two tables.

    You can find more information on Ruby Guides

    Now, we can do this by adding `belongs_to :user` in the `friends.rb` model (inside `models` folder)

    Now, since a `user` has many friends, we add `has_many :friends` in the `device.rb` file.

    Now, we need to add a new column to associate an user with friends. 

    To add a new column to an existing database, we run the following command

    ```ruby
    rails g migration add_<column_name>_to_<table_name> <column_name>:<column_type>:<optional such as index>

    # Here we do the following

    rails g migration add_user_id_to_friends user_id:integer:index
    ```

    Now, we created (generated) a migration. Now, we need to push it. So, we run

    ```ruby
    rails db:migrate
    ```

    Now, we have to create a user id field in the `_form.html.erb` file. But it must be hidden and must be populated with the current user's id. So, to do that we do the following to retrieve the user id.

    Go to `devise` homepage and then find `current_user` 

    If we use `<%= current_user %>` we get an object with the memory where the current user details are store. Now, if use `<%= current_user.inspect %>` we can see the contents of this object.

    Now, we only need the ID.

    So we do the following `<%= current_user.id %>` to populate the new field like this;

    ```ruby
    <%= form.number_field :user_id, id: :friend_user_id, class:"form-control", value: current_user.id, type: :hidden %>

    # To make sure that the id sticks to the friends table we use the id: :
    # To make the id invisible, we use hidden
    ```

    Now, if we add a new friend, it will throw an error saying that **1 error prohibited this friend from being saved: user must exist.**

    This is because, we only created the user id. But we haven't yet authorized the app to make the user id pass through the form. Thus, it looks for the user id (we have only authorized for the other 5) and hence it throws and error.

    That is, if we check the `friends_controller.rb` file, at the last, there will be set of params that will be permitted. There will be **NO** user id in those permitted params. So, add it.

    But still the friends  list of others will be visible to everyone.  This mustn't be there. We must change it.

    # More Associations

    So to rectify the previous flaw, we need `before_action` in the `friend_controller.rb`

    So, the user must be authenticated. So, add this in a **new line**  below the previous **before_action**

    ```ruby
    before_action :authenticate_user!, except: [:index, :show]

    # I'm not going to use the **except**. For anything, the user must authenticate.
    ```

    So, if the user is authenticated, then, he will be able to see her/his friend's list, edit, create or delete them.

    So, a more secure way to prevent user from editing other user data is by defining a function

    ```ruby
    def correct_user
        @friend = current_user.friends.find_by(id: params[:id])
        redirect_to friends_path, notice: "You are not authorized to perform this action." if @friend.nil?
      end
    ```

    And add a `before_action`

    ```ruby
    before_action :correct_user, only: [:edit, :update, :destroy]

    # I am not using **only
    # We must use only and except else it will not connect to localhost because of TOO MANY REDIRECTS**
    ```

    Also, we need to build current user's friends instead of creating a new Friend Object.

    So change

    ```ruby
    @friend = Friend.new
    ```

    To

    ```ruby
    @friend = current_user.friends.build
    ```

    Similarly change `create` function

    Replace the first line with

    ```ruby
    @friend = current_user.friends.build(friend_params)
    ```

    For more security reasons, show only the current user's friend list. To do this, go to `index.html.erb` and then add an if condition above the `<tr>` tag

    ```ruby
    <% if friend.user == current.user %>
    	<tr>
    	...
    	</tr>
    <% end %>
    ```

    # Style Modifications

    1. Merging first name and last name columns to single Name column
    2. Deleting `Show` and `Edit` buttons
    3. Including delete buttons in `Show` Page and in `Edit` page
    4. Making the show functionality in the First name + Last Name
