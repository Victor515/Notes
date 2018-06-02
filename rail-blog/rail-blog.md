
#	1. Install rail on windows:
http://installrails.com/
dependency:
* ruby > 2.26
* sqlite3

# 2. Create a blog system:

## 2.1 article CRUD

* rail routing:
http://guides.rubyonrails.org/routing.html
* form_with in form helper:
http://api.rubyonrails.org/v5.2.0/classes/ActionView/Helpers/FormHelper.html#method-i-form_with
* form_with resource oriented style:
http://api.rubyonrails.org/v5.2.0/classes/ActionView/Helpers/FormHelper.html#method-i-form_with-label-Resource-oriented+style
* strong parameters(come with rails security precatution):
http://guides.rubyonrails.org/action_controller_overview.html#strong-parameters
* active record migration:
http://guides.rubyonrails.org/active_record_migrations.html
* active record validation:
http://guides.rubyonrails.org/active_record_validations.html
* layout and rendering in Rails(partials, etc..)
http://guides.rubyonrails.org/layouts_and_rendering.html
* JS in Rails:
http://guides.rubyonrails.org/working_with_javascript_in_rails.html


## 2.2 comment CRUD


* Active Record Association:
http://guides.rubyonrails.org/association_basics.html

## 2.3 Authentication:


# 3. Layout with Bootstrap


* How to introduct Bootstrap in our blog system?
Add the following line to Gemfile, and then run `bundle install`:
```
gem 'bootstrap-sass', '3.3.7'
```
* The asset pipeline
http://edgeguides.rubyonrails.org/asset_pipeline.html
* Bootstrap doc
https://getbootstrap.com/docs/4.1/getting-started/introduction/
Basically we just add class to elements to change their format

# 4. Heroku Deploy
https://devcenter.heroku.com/articles/getting-started-with-rails5#database
In both `Gemfile` and `config/database.yml`, change sqlite3 to postresql

# 5. Some documentation:
* Rails 5.1.6 Guide
http://guides.rubyonrails.org/v5.1/
* Nice book to learn Ruby:
https://www.railstutorial.org/book/frontmatter
* Add custom class to form_with:
https://stackoverflow.com/questions/5315967/add-a-css-class-to-f-submit
