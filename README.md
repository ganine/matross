# matross

## Usage

Put matross in the `:development` group of your `Gemfile`:

```ruby
group :development do
  gem 'matross', :git => 'git://github.com/innvent/matross.git'
end
```

Run `bundle exec capify .` in the project root folder:

```bash
$ bundle exec capify .
```


## What's inside?

We made a bunch of additions and customizations. Below we list the most relevant ones.

* **Foreman by default**:
* **Custom foreman upstart template**: we use a custom upstart template, that enables `console log`, allowing `logrotate` to work properly.

## Overriding default templates
We have our opinions, but don't know everything. What works for us, may not fit your needs since each app is a unique snowflake. To take care of that `matross` allows you to define your own templates to use instead of the built in ones. Look at the included ones in `lib/matross/templates` to see how we think things should go.

For example to override the default `lib/matross/templates/unicorn.rb.erb` simply put your template in your applications `config/matross/unicorn/unicorn.rb.erb`. The general idea is `config/matross/#{recipe_name}/#{erb_template}`

## Managing application daemons with Foreman

Foreman has freed us of the tedious task of writing `init` and `upstart` scripts. Some of our `matross` recipes automatically add processes - such as the `unicorn` server - to the Procfile.

If you have an application Procfile with custom daemons defined, such as `delayed_job`, they will be concantenated with all the processes defined in `matross`, resulting in one final Procfile-matross file that will be used to start your app and export init scrips.

You can specify the number of each instance defined in Procfile-matross using the `foreman_procs` capistrano variable.
Supose you have a process called `dj` and want to export 3 instances of it:

```ruby
set :foreman_procs, {
    dj: 3
}
```

We also modified the default upstart template to log through upstart instead of just piping stdout and stderr into files. Goodbye nocturnal logexplosion. (Like all templates you can override it)
