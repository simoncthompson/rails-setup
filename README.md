# Setting up a Rails project from scratch

These are a general set of commands for setting up a Rails web app with a handful of go-to gems like Devise, Simple Form, and Bootstrap (among others).

There's always more to do, and everyone has their own quirks, but these are mine and they work for me. Hope it's helpful.

## Type these in your terminal
```bash
cd dev/path
rails new rails-project --database=postgresql
yarn add bootstrap jquery popper.js
```
## Add these to your gem file
```ruby
# production
gem 'autoprefixer-rails'
gem 'font-awesome-sass', '~> 5.12.0'
gem 'simple_form'
gem 'devise'
gem 'pundit'

# development only
gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
```
## Type these in your terminal
```bash
bundle install
rails generate simple_form:install --bootstrap
rails generate devise:install
rails g pundit:install
rails active_storage:install
rails webpacker:install:stimulus
```

## Add these to your configs
```ruby
# config/environments/development.rb
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

# config/routes.rb
root to: "home#index" # root *somewhere*
```

```ruby
# config/storage.yml

cloudinary:
  service: Cloudinary

# config/environments/development.rb
config.active_storage.service = :cloudinary
```

## Add Pundit and Devise actions to application controller
```ruby
# app/controllers/application_controller.rb
# class ApplicationController < ActionController::Base
  # [...]
  before_action :authenticate_user!
  include Pundit

  # Pundit: white-list approach.
  after_action :verify_authorized, except: :index, unless: :skip_pundit?
  after_action :verify_policy_scoped, only: :index, unless: :skip_pundit?

  rescue_from Pundit::NotAuthorizedError, with: :user_not_authorized
  def user_not_authorized
    flash[:alert] = "You are not authorized to perform this action."
    redirect_to(root_path)
  end

  private

  def skip_pundit?
    devise_controller? || params[:controller] =~ /(^(rails_)?admin)|(^pages$)/
  end
# end
```

## Add responsive viewport meta to application.html.erb
```html
<!-- app/views/layouts/application.html.erb -->
<!-- head -->
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">

<!-- body -->
<p class="notice"><%= notice %></p>
<p class="alert"><%= alert %></p>
```
## Rename application.css to application.scss

##  Import Boostrap JS library using webpack
```javascript
// config/webpack/environment.js
// const { environment } = require('@rails/webpacker')
const webpack = require('webpack')
environment.plugins.prepend('Provide',
  new webpack.ProvidePlugin({
    $: 'jquery',
    jQuery: 'jquery',
    Popper: ['popper.js', 'default']
  })
)
// module.exports = environment ### -in
```

```javascript
// app/javascript/packs/application.js
import 'bootstrap';
```

```css
<!-- rename app/assets/stylesheets/application.css to .scss -->
<!-- add the below line to it -->
@import "bootstrap/scss/bootstrap"
```

## Type these in your terminal
```bash
rails g devise:views
# git it
git add .
git commit -m "Init commit"
gh repo create
git push origin master
```

Thanks,
Simon
