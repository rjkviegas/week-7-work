# Process

Taken from [Getting Started](https://guides.rubyonrails.org/getting_started.html)

1. Create Controller
```
rails generate controller Welcome index

create  app/controllers/welcome_controller.rb
 route  get 'welcome/index'
invoke  erb
create    app/views/welcome
create    app/views/welcome/index.html.erb
invoke  test_unit
create    test/controllers/welcome_controller_test.rb
invoke  helper
create    app/helpers/welcome_helper.rb
invoke    test_unit
invoke  assets
invoke    scss
create      app/assets/stylesheets/welcome.scss
```

Most Important
- ```app/controllers/welcome_controller.rb```
- ```app/views/welcome/index.html.erb```

2. Set App Homepage
Open app's ***routing file*** ```config/routes.rb```

Rails.application.routes.draw do
  get 'welcome/index'
 
  root 'welcome#index'
end

3.Create a ***resource***
A resource a collection of similar objects, such as articles, people, or animals. You can create, read, update, and destroy items for a resource, CRUD operations.

In ```config/routes.rb```
```
Rails.application.routes.draw do
  get 'welcome/index'
 
  resources :resources_name
 
  root 'welcome#index'
end
```

Use ```rails routes``` to see defined routes for all standard RESTful actions.

4. Create ResourceController
```rails generate controller Resource_Name```

A controller is a class that is defined to inherit from ApplicationController. Methods defined inside this class  will become the actions for this controller. These actions will perform CRUD operations on the resources.

Resource methods order:
- Index
- Show
- New
- Edit
- Create
- Update
- Destroy

5. Form builder

Example
```
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
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
Identifying scope tells ```form_with``` what the form is for

The action 'create' is used for new form submissions, the form should be pointed to that action using ```url: articles_path```.

The ```articles_path``` helper points the form to the URI Pattern associated with the ```articles``` prefix; and the form will (by default) send a ```POST``` request to that route.

When a form is submitted, the fields of the form are sent to Rails as parameters. These parameters can be referenced inside the controller actions. To see what these parameters look like, change the create action to this:
```
def create
  render plain: params[:article].inspect
end
```

6. Creating Models
Models in Rails use a singular name, and their corresponding database tables use a plural name.
```
rails generate model Article title:string text:text
```
Development environment by default, command will apply to the database defined in the ```development``` section of your ```config/database.yml``` file. To execute migrations in another environment,  such as production, you must explicitly pass it when invoking the command: ```rails db:migrate RAILS_ENV=production```.

Active Record is smart enough to automatically map column names to model attributes, which means you don't have to declare attributes inside Rails models, as that will be done automatically by Active Record.

7. Migration
```
rails db:migrate
```

8. Saving data in controller
Create action in controller class (includes validation)
```
def create
  @article = Article.new(article_params)
 
  if @article.save
    redirect_to @article
  else
    render 'new'
  end
end
 
private
  def article_params
    params.require(:article).permit(:title, :text)
  end
```

```@article.save``` returns a boolean indicating whether the article was saved or not.

Have to define our permitted controller parameters to prevent wrongful mass assignment.

9. Showing Resources
Show action in controller class
```
def show
    @article = Article.find(params[:id])
end
```
In View
```
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
```

10. List all resources
Index action in controller class
```
def index
    @articles = Article.all
end
```
In View
```
<h1>Listing Articles</h1>
 
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
    <th></th>
  </tr>
 
  <% @articles.each do |article| %>
    <tr>
      <td><%= article.title %></td>
      <td><%= article.text %></td>
      <td><%= link_to 'Show', article_path(article) %></td>
    </tr>
  <% end %>
</table>
```

11. Adding Links
The ```link_to``` method is a built-in view helper, creates a hyperlink based on text to display and where to go.

As current controller is used by default you don't need to specify the :controller option to link to an action in the same controller.
From index to new
```
<%= link_to 'New article', new_article_path %>
```
From specific resource page back to index or edit page for specific resource
```
<%= link_to 'Edit', edit_article_path(@article) %> |
<%= link_to 'Back', articles_path %>
```
Links for show and edit actions in lists
```
<td><%= link_to 'Show', article_path(article) %></td>
<td><%= link_to 'Edit', edit_article_path(article) %></td>
<td><%= link_to 'Destroy', article_path(article),
              method: :delete,
              data: { confirm: 'Are you sure?' } %></td>
```

12. Validation
Example
```
class Article < ApplicationRecord
  validates :title, presence: true,
                    length: { minimum: 5 }
end
```
Error message
```
<% if @article.errors.any? %>
  <div id="error_explanation">
    <h2>
      <%= pluralize(@article.errors.count, "error") %> prohibited
      this article from being saved:
    </h2>
    <ul>
      <% @article.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
    </ul>
  </div>
<% end %>
```

Fields that contain an error auto wrapped with a div with class ```field_with_errors```. Use CSS rule to make them standout.

13. Updating Resources
Edit action in controller class
```
def edit
  @article = Article.find(params[:id])
end
```
Similar view to new except:
```<%= form_with(model: @article, local: true) do |form| %>```

Passing the article object to the ```form_with``` method will automatically set the URL for submitting the edited article form. This option tells Rails that we want this form to be submitted via the ```PATCH HTTP``` method, which is the HTTP method you're expected to use to update resources according to the REST protocol.

Also, passing a model object to ```form_with```, like ```model: @article``` in the edit view above, will cause form helpers to fill in form fields with the corresponding values of the object. Passing in a symbol scope such as ```scope: :article```, as was done in the new view, only creates empty form fields.

Update action in controller class
```
def update
  @article = Article.find(params[:id])
 
  if @article.update(article_params)
    redirect_to @article
  else
    render 'edit'
  end
end
```

14. Partials
Partial files are prefixed with an underscore, used to remove duplication from views (such as ```edit``` and ```new```)

Everything except for the ```form_with``` declaration remains the same. Can use shorter, simpler ```form_with``` declaration to stand in for either of the other forms is that ```@article``` is a resource corresponding to a full set of RESTful routes, and Rails is able to infer which URI and method to use.

To use partial ```app/views/articles/_form.html.erb```
```
<%= render 'form' %>
```

15. Delete Resources
Destroy action in controller class
```
def destroy
  @article = Article.find(params[:id])
  @article.destroy
 
  redirect_to articles_path
end
```
Do not need to add a view for this action since redirection to the ```index``` action

16. Second Model
For a model holding a reference
```
rails generate model Comment commenter:string body:text article:references
```
Model
```
class Comment < ApplicationRecord
  belongs_to :article
end
```
Resource being referenced
```
class Article < ApplicationRecord
  has_many :comments
  validates :title, presence: true,
                    length: { minimum: 5 }
end
```

17. Adding routes for model holding reference
In ```config/routes.rb```
```
resources :articles do
  resources :comments
end
```
This creates comments as a ***nested resource*** within ```articles```. This is another part of capturing the hierarchical relationship that exists between articles and comments.

