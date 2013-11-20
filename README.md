# premailer-rails

CSS styled emails without the hassle.

[![Build Status][build-image]][build-link]
[![Gem Version][gem-image]][gem-link]
[![Dependency Status][deps-image]][deps-link]
[![Code Climate][gpa-image]][gpa-link]
[![Coverage Status][cov-image]][cov-link]
[![Bitdeli Badge][stats-image]][stats-link]

## Introduction

This gem is a drop in solution for styling HTML emails with CSS without having
to do the hard work yourself.

Styling emails is not just a matter of linking to a stylesheet. Most clients,
especially web clients, ignore linked stylesheets or `<style>` tags in the HTML.
The workaround is to write all the CSS rules in the `style` attribute of each
tag inside your email. This is a rather tedious and hard to maintain approach.

Premailer to the rescue! The great [premailer] gem applies all CSS rules to each
matching HTML element by adding them to the `style` attribute. This allows you
to keep HTML and CSS in separate files, just as you're used to from web
development, thus keeping your sanity.

This gem is an adapter for premailer to work with [actionmailer] out of the box.
Actionmailer is the email framework used in Rails, which also works outside of
Rails. Although premailer-rails has certain Rails specific features, **it also
works in the absence of Rails** making it compatible with other frameworks such
as sinatra.

premailer-rails works with actionmailer by registering a delivery hook. This
causes all emails that are delivered to be processed by premailer-rails. This
means that, by simply including premailer-rails in your `Gemfile`, you'll get
styled emails without having to set anything up.

## Installation

Simply add the gem to your `Gemfile`:

```ruby
gem 'premailer-rails'
```

premailer-rails requires either [nokogiri] or [hpricot]. It doesn't list them as
a dependency so you can choose which one to use. Since hpricot is no longer
maintained, I suggest you to go with nokogiri. Add either one to your `Gemfile`:

```ruby
gem 'nokogiri'
# or
gem 'hpricot'
```

If both gems are loaded for some reason, premailer chooses hpricot.

That's it!

## Configuration

Premailer itself accepts a number of options. In order for premailer-rails to
pass these options on to the underlying premailer instance, specify them
as follows (in Rails you could do that in an initializer such as
`config/initializers/premailer_rails.rb`):

```ruby
Premailer::Rails.config.merge!(preserve_styles: true, remove_ids: true)
```

For a list of options, refer to the [premailer documentation]. The default
configs are:

```ruby
{
  input_encoding: 'UTF-8',
  generate_text_part: true
}
```

If you don't want to automatically generate a text part from the html part, set
the config `:generate_text_part` to false.

Note that the options `:with_html_string` and `:css_string` are used internally
by premailer-rails and thus will be overridden.

## Usage

premailer-rails processes all outgoing emails by default. If you wish to skip
premailer for a certain email, simply set the `:skip_premailer` header:

```ruby
class UserMailer < ActionMailer::Base
  def welcome_email(user)
    mail to: user.email,
         subject: 'Welcome to My Awesome Site',
         skip_premailer: true
  end
end
```

[build-image]: https://travis-ci.org/fphilipe/premailer-rails.png
[build-link]:  https://travis-ci.org/fphilipe/premailer-rails
[gem-image]:   https://badge.fury.io/rb/premailer-rails.png
[gem-link]:    https://rubygems.org/gems/premailer-rails
[deps-image]:  https://gemnasium.com/fphilipe/premailer-rails.png
[deps-link]:   https://gemnasium.com/fphilipe/premailer-rails
[gpa-image]:   https://codeclimate.com/github/fphilipe/premailer-rails.png
[gpa-link]:    https://codeclimate.com/github/fphilipe/premailer-rails
[cov-image]:   https://coveralls.io/repos/fphilipe/premailer-rails/badge.png
[cov-link]:    https://coveralls.io/r/fphilipe/premailer-rails
[stats-image]: https://d2weczhvl823v0.cloudfront.net/fphilipe/premailer-rails/trend.png
[stats-link]:  https://bitdeli.com/

[premailer]:    https://github.com/premailer/premailer
[actionmailer]: https://github.com/rails/rails/tree/master/actionmailer
[nokogiri]:     https://github.com/sparklemotion/nokogiri
[hpricot]:      https://github.com/hpricot/hpricot

[premailer documentation]: http://rubydoc.info/gems/premailer/1.7.3/Premailer:initialize
