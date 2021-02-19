# Setting up a Rails project from scratch

These are a general set of commands for setting up a Rails web app with a handful of go-to gems like Devise, Simple Form, and Bootstrap (among others).

There's always more to do, and everyone has their own quirks, but these are mine and they work for me. Hope it's helpful.

## Type these in your terminal
```bash
cd dev/path
rails new rails-project --database=postgresql
yarn add bootstrap jquery popper.js
git add .
git commit -m "Init commit"
gh repo create
git push origin master
```
## Add these to your gem file
```ruby
# production
gem 'autoprefixer-rails'
gem 'font-awesome-sass', '~> 5.12.0'
gem 'simple_form'
gem 'devise'

# development only
gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
```
## Type these in your terminal
```bash
bundle install
rails generate simple_form:install --bootstrap
rails generate devise:install
```


## Add responsive viewport meta to application.html.erb
```html
<!-- app/views/layouts/application.html.erb -->
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
```
##  Import Boostrap JS library using webpack
```javascript
// filepath: config/webpack/environment.js
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

## Type these in your terminal
```bash
git add .
git commit -m "Init commit"
gh repo create
git push origin master
```

Thanks,
Simon
