# Rails

## What is Rails

Rails is a web application development framework written in the Ruby programming language.

The Rails philosophy includes two major guiding principles:

- **Don't Repeat Yourself**: DRY is a principle of software development which states that "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system." By not writing the same information over and over again, our code is more maintainable, more extensible, and less buggy.
- **Convention Over Configuration**: Rails has opinions about the best way to do many things in a web application, and defaults to this set of conventions, rather than require that you specify minutiae through endless configuration files.

## Installing Rails

Pre-Requisites
- Ruby
- SQLite3
- Node.js
- Yarn

## Controller

A controller's purpose is to receive specific requests for the application. Routing decides which controller receives which requests. Often, there is more than one route to each controller, and different routes can be served by different actions. Each action's purpose is to collect information to provide it to a view.

To create a new controller, you will need to run the "controller" generator and tell it you want a controller called "Welcome" with an action called "index", just like this:
```
$ rails generate controller Welcome index
```
Rails will create several files and a route for you.

A controller is simply a class that is defined to inherit from ApplicationController. It's inside this class that you'll define methods that will become the actions for this controller. These actions will perform CRUD operations on the articles within our system.

## View

A view's purpose is to display this information in a human readable format. An important distinction to make is that it is the controller, not the view, where information is collected. The view should just display that information. By default, view templates are written in a language called eRuby (Embedded Ruby) which is processed by the request cycle in Rails before being sent to the user.

## ```config/routes.rb```

to tell Rails where your actual home page is located open the file ```config/routes.rb``` in your editor.

This is your application's routing file which holds entries in a special DSL (domain-specific language) that tells Rails how to connect incoming requests to controllers and actions.
```
Rails.application.routes.draw do
  get 'welcome/index'
 
  root 'welcome#index'
end
```

```root 'welcome#index'``` tells Rails to map requests to the root of the application to the welcome controller's index action and ```get 'welcome/index'``` tells Rails to map requests to http://localhost:3000/welcome/index to the welcome controller's index action. This was created earlier when you ran the controller generator (```rails generate controller Welcome index```).

## Form

To create a form within this template, you will use a form builder. The primary form builder for Rails is provided by a helper method called ```form_with``.

```
<%= form_with scope: :article, local: true do |form| %>
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

When you call ```form_with```, you pass it an identifying scope for this form. In this case, it's the symbol ```:article```. This tells the ```form_with``` helper what this form is for. Inside the block for this method, the ```FormBuilder``` object - represented by ```form``` - is used to build two labels and two text fields, one each for the title and text of an article. Finally, a call to ```submit``` on the ```form``` object will create a submit button for the form.

There's one problem with this form though. If you inspect the HTML that is generated, by viewing the source of the page, you will see that the ```action``` attribute for the form is pointing at ```/articles/new```. This is a problem because this route goes to the very page that you're on right at the moment, and that route should only be used to display the form for a new article.

The form needs to use a different URL in order to go somewhere else. This can be done quite simply with the ```:url``` option of ```form_with```. Typically in Rails, the action that is used for new form submissions like this is called "create", and so the form should be pointed to that action.

Edit the form_with line inside app/views/articles/new.html.erb to look like this:
```
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
```

By default ```form_with``` submits forms using Ajax thereby skipping full page redirects. To make this guide easier to get into we've disabled that with ```local: true``` for now.

## Model

Models in Rails use a singular name, and their corresponding database tables use a plural name. Rails provides a generator for creating models, which most Rails developers tend to use when creating new models. To create the new model, run this command in your terminal:

$ rails generate model Article title:string text:text
With that command we told Rails that we want an Article model, together with a title attribute of type string, and a text attribute of type text. Those attributes are automatically added to the articles table in the database and mapped to the Article model.

Rails responded by creating a bunch of files. For now, we're only interested in app/models/article.rb and db/migrate/20140120191729_create_articles.rb (your name could be a bit different). The latter is responsible for creating the database structure, which is what we'll look at next.

Active Record is smart enough to automatically map column names to model attributes, which means you don't have to declare attributes inside Rails models, as that will be done automatically by Active Record.

## CRUD Controller Methods

A frequent practice is to place the standard CRUD actions in each controller in the following order: index, show, new, edit, create, update and destroy. You may use any order you choose, but keep in mind that these are public methods; as mentioned earlier in this guide, they must be placed before declaring private visibility in the controller.

ISNECUD
