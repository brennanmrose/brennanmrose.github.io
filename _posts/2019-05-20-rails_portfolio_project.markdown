---
layout: post
title:      "Rails Portfolio Project"
date:       2019-05-20 13:25:39 -0400
permalink:  rails_portfolio_project
---


Offering your users a way to create two objects on one form can significantly improve user experience on your app. Take for example an app that allows you to store books. Would you want for your user to have to go to two different pages to create a book and create an author? Absolutely not! Let's review how to create both objects on one view. It involves making updates on the model, the view, and the controller. 

1. **Update Your View**

     Assuming you are using form_for with Rails to create your form you will simply need to add a `fields_for` for your new                object. You can checkout documentation on `fields_for` [here](https://apidock.com/rails/ActionView/Helpers/FormHelper/fields_for).
		 On our book storage app it would look something like this:
		 ```<%= f.fields_for :author do |c| %>```
			  ```<%= c.label :name %>```
			  ```<%= c.text_field :name %>```
		```<% end %> ```

2. **Update Your Model**

     Next you'll want to add `accepts_nested_attributes_for :author` on your Book model. This method creates a method          `author_attributes=` on your Book model that allows it to access the Author class in order to create a new object. For         more reading on this subject you can check out the documentation [here](https://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html). Yet some folks opt for writing a         custom setter instead of using the Rails package `accepts_nested_attributes_for`. You may want to do this if you                 don't validate uniqueness on the nested object as `accepts_nested_attributes_for` always creates a new instance, so        your custom setter could use `find_or_create_by`. However if you validate uniqueness this will be handled in your form.


3. **Update Your Controller**

     The last step is to handle the new object in your controller. This is a two-part step:
		 First you must update your strong params to accept your new object. You may do this by adding the name of your new      object and the word attributes that contains an array of attributes. For our book storage example it would look                        something like `author_attributes: [:name]`.
		 Second you will need to properly initialize your new object so that it displays in your view. You may do this by building          the secondary object on your primary object, such as `@song.build_author`. We do this instead of                                       `@song.author.build` because if the song doesn't already have an author the first part of the chained method will return `nil` and there will be nothing to build upon. As outlined in the [Rails guides](https://edgeguides.rubyonrails.org/association_basics.html):
		 
> When initializing a new has_one or belongs_to association you must use the build_ prefix to build the association, rather than the association.build method that would be used for has_many or has_and_belongs_to_many associations.
		 

