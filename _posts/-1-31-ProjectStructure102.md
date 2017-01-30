---
layout: post
title: Project Structure 102
published: true
---

_27/01/2017_

Welcome to the second post covering the project structure and code. In this post we'll peek in at the most important configurations and files and custom code

This description is based on the repository as it existed at the 16th of January (commit [59ebd8bbae112d6709d0affd7d0e74f1bf992510](https://github.com/CodeforAustralia/dtf-genesis/tree/59ebd8bbae112d6709d0affd7d0e74f1bf992510) to be exact) as there have been some significant changes and it's a moving target

### Rails-isms, Ruby-isms and a little about Regular Expressions
![]({{ site.baseurl }}/images/102_book.jpeg)

There are a few preliminary code structures I feel it worth getting out of the way before delving into the details of what we've done to make the CCR, we will assume basic familiarity with programming but no particular language should be required to understand the code presented. The first of these I have grown to love in Ruby which is string interpolation, there are two significant types of strings we've used: those with single quote ```'``` and those with double quote ```"```. The single quote strings are literal strings and as far as we're concerned these never undergo interpolation and represent exactly the string that appears on screen, for example the code:

    x = 'not #{interpolated}'

would result in x containing ```not #{interpolated}```. However the strikingly similar code:

    string = 'variable value'
    x = "interpolated #{string}"

would result in the value of x depending on the variable ```string``` and in this example would be ```interpolated variable value```. Ruby uses the ```#{}``` sequence to escape a little bit of code within a string at runtime

Ruby also have some special syntax for arrays and ranges in an array it is common enough for a language to allow indexing into a string like it was an array, and in Ruby the string literal itself can be used to index into, so for example ```"string"[2]``` would evaluate to ```"r"```. A neat array trick of many modern languages that Ruby also provides is **ranges**, where the beginning and ending subscripts of an array can be provided to produce a sub-array or sub-string, lets look at three examples:

1. ```"string"[2...4]``` would evaluate to  ```"ri"```
2. ```"string"[2..9]``` would be ```"ring"``` and
3. ```"string"[1..-2]``` would result in ```"trin"```

The first example demonstrates the exclusive ```...``` by not including the "n", where using the inclusive ```..``` range operator would have resulted in the string ```"rin"```, the second example demonstrates how Ruby can be helpful and returns an array as long as it can and the last example shows how Ruby uses negative indexes into an array as a shortcut for indexing from the end, such that -1 is the last array entry, -2 the second last, and so on.

![]({{ site.baseurl }}/images/102_foil.jpeg)

We also use a couple of **[regular expressions](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended)** that I have skittishly encapsulated away into a couple of functions. These regular expressions are much simpler than the ones we began our scraping with and can be understood with familiarity of only a few regular expression characters. First up is the period ```.``` character, this character simply matches **any** non-newline character... (almost) everyone loves ```.```. Next up I feel obliged to introduce a trio, although we only use one of them in our code they are rather interrelated, these are the star ```*``` plus ```+``` and optional ```?``` regular expression **quantifiers**, they all operate on the character before them and specify how many of those characters to match. Pairing each one up with our period operator we get the three regular expressions;

1. ```.*``` which matches any string of **zero or more** characters,
2. ```.+``` which matches any string of **one or more** characters and
3. ```.?``` which matches any string of **zero or one** characters.

Now we've got the regular expression basics done how do we use this. One last bit of regular expression syntax we'll explore is the pair of parenthesis or **grouping** symbols ```(``` and ```)``` which are used to group a segment of a regular expression. We can use them in a simple example in just a minute but to do so we'll need one little bit of Ruby which is the regular expression matching syntax ```//``` which operate a bit like quotes around a string and are used on strings in conjunction with the **match** function like so: ```"string".match(/.?.?.?/)```, this returns a **MatchData** object with the value ```"str"``` made up of the three optional characters. If we now start grouping the regular expression buy putting parenthesis around the first optional character and the last two optional characters like this ```"string".match(/(.?)(.?.?)/)``` we now get a match (with the value ```"str"```) along with two groups where the first is ```"s"``` and the second is ```"tr"```

One last bit of tecnocrati I'd like to demystify before we proceed to the code is some basics of **URL syntax**. The humble url is made up of many components but often the function or significance of the component is taken for granted or often just uses default values. Lets start with the misleadingly simple example of "google.com" (and can often be reached by typing "goog\<return\>") which is only shorthand for the much more complete "https://www.google.com:443/?gfe_rd=cr&ei=3eyNWN_3JNPM3gfE0L3wBA" which much more nicely demonstrates the components we might wish to tease apart. The first component is the **protocol** and is usually "http" or more often these days "https" which I won't go into here. The "www" in our example is an identifier to the server of what kind of request we are making, this is often assumed to be a **web** or www request if not specified. the "google.com" is the address of the **server** where result will be served from. The value ":443" specifies that we wish to use the default https **port**, but could be specified if another port on the server is expecting requests. Our example does not specify a **page** for the server to serve so the default page is selected, but optionaly a path is specified between the last slash and the question mark. The question mark ```?``` signifies the beginning of the **parameters** sent to the page. The paramaters are **key-value pairs** specified in the format ```key=value``` and are delimited by ampersand ```&``` characters, so our example has two pairs "gfe_rd=cr" and "ei=3eyNWN_3JNPM3gfE0L3wBA" which are probably referrer or tracking identifiers. A final note about URL syntax, the **space character** is not allowed in URLs so the space character is often represented by the value "%20", so to assign "this string" to the key "x", we would need to translate the string into URL syntax and use ```"x=this%20string"```


### Documentation
![]({{ site.baseurl }}/images/102_docs2.jpeg)

The CCR system we've created has been licensed under the **GPL v3** and a copy of this license is in the file called "LICENSE" and I'd just like to take a moment to outline why this licence has been selected. If you look at the [choose a license](https://choosealicense.com/) site the tag line for GPL v3 is "I care about **sharing improvements**" and it goes on to summarise the requirements as "anyone who distributes your code or a derivative work to make the source available under the same terms". It seems to be a recurring theme within government to have a custom piece of software written and then not have the expertise or even access to the program or source code to make even minor adjustments and improvements. The GPL v3 was written expressly to prevent this kind of vendor lock in situation and allows the user of a software product to remain in control. Many software vendors will recommend against the GPL family of licenses because the lose bargaining power to the user

The actual requirements of the GPL v3 are more complete than the summary given at the choose a license site and include requirements for a reasonably competent engineer (pay attention public service, you may want a few of these people one day) to be able to build the program and deploy a derivative work. This allows users to be free from vendor lock in while still allowing an ecosystem of vendors to develop and support the software

Moving on to the software documentation, the ```erd.pdf``` file contains the entity relation diagram for the database and seems to be missing from recent releases, however it can be easily refreshed by running the ruby ```bundle exec erd``` program. You can see that the contracts are the centre of the action in this database

The rest of the code has been written and arranged according to the Rails conventions for the most part and is reasonably self-documented internally. We have attempted to make the code as literate and self documenting as we could and should be reasonably easy for a compitant Rails developer to grock


### Configuration
![]({{ site.baseurl }}/images/102_cfg.jpg)

Lets now have a closer look at some of the more important configuration files


├── environments/
│   ├── production.rb
├── initializers/
│   ├── scheduler.rb
├── application.rb
├── database.yml
├── environment.rb
├── puma.rb
└── secrets.yml


### db, secrets & environment
![]({{ site.baseurl }}/images/102_erd.jpg)
├── schema.rb


### The (somewhat) all important app directory
![]({{ site.baseurl }}/images/102_controls.jpeg)
We'll only be having a zip through the app directory this time with a thourough coverage in the third installment tentativly titled "(not even almost) all about MVC frameworks" next time.
├── assets/
│   ├── javascripts/
│   │   ├── application.js
│   │   ├── charts.js
│   │   ├── segmentio.js
│   │   ├──     "    .js
│   └── stylesheets/
│       ├── application.scss
├── jobs/
│   └── application_job.rb
├── mailers/
│   └── application_mailer.rb


####  (robots and meta)
![]({{ site.baseurl }}/images/102_robot.jpg)
We have not explored the use of the robots.txt file in our project as people and robots scraping our site was not an issue that came up. However I feel it is pertinant to mention the file and its function in web culture and communication norms

Meta data is something that we have also not had an opportunity to explore but would be great for improving government interface design, the metadata information is used in modern platforms like Slack and Twitter to display fancy previews of the webpage that can be embedded or shared with other users


### Gemfile and bundle
![]({{ site.baseurl }}/images/102_rubies.jpg)

We have used many libraries during the development process and these are specified in the file named "Gemfile". This file is most often processed by the ```bundle``` command which manages the Ruby packages (or gems) installed on the system. We examine just a few snippets of the file below

    ruby '2.3.1'
    gem 'rails', '~> 5.0.0', '>= 5.0.0.1'

We specify the versions of Ruby and Rails that our project uses so that bundle can take care of managing which versions to run

    gem 'pg', '~> 0.18'
    gem 'puma', '~> 3.0'

Here we specify that we are using the PostgreSQL database and the Puma web server which are minor upgrades to the default SQLite and WebBrick infrastructure used in Rails by default

    gem 'capybara', '>= 2.1.0'
    gem 'poltergeist', '>= 1.9.0'
    gem 'phantomjs', :require => 'phantomjs/poltergeist'
    gem 'rufus-scheduler'

The capybara and poltergeist libraries are used by our scraper to power a phantomjs browser for Tenders Vic and the rufus-scheduler library is used to automate the daily scraping routine

    group :test do
      gem 'minitest-reporters',       '>= 1.1.9'
      gem 'guard',                    '>= 2.13.0'
      gem 'guard-minitest',           '>= 2.4.4'
      gem 'capybara-screenshot'
      gem 'scrutinizer-ocular'
    end

This segment demonstrates dependency groups in the Gemfile. These requirements are only used in specific configurations or environments. This example is only used during test commands so our production environment won't have these requirements installed. Guard and Minitest give us an improved and flashy interface when we run the tests. Scrutinizer and Capybara Screenshot are currently underutilized but provide for enhanced testing capabilities such as logging screen shots during tests for visual diffs and monitoring general code quality

### Build configuration files (CI/CD)
![]({{ site.baseurl }}/images/102_timing.jpg)
(, .scrutinizer.yml, .travis.yml)
We have experimented with many continuious integration and analytic tools during the fellowship and I would highly recommend the segment.io and semaphore platforms for future projects.

#### .codeclimate.yml

    engines:
      rubocop:
        enabled: true
      bundler-audit:
          enabled: true
      fixme:
          enabled: true
      markdownlint:
          enabled: true
      eslint:
          enabled: true
      csslint:
          enabled: true
      duplication:
          enabled: true

The Code Climate service monitors you codebase and reports on code quality metrics, of course all metrics must be taken with a grain of salt but hopefully you'll see the value in monitoring relative values over the lifetime of a project. For example monitoring the number of "fixme" and "todo" strings in a codebase can give you a sense of the ammount of work being delayed or put off. There may be good reasons or stylistic differences between developers that make any individual value unreliable, but by combining and agregating general trends we can see when **too much** work is being deferred or back loaded

        config:
            languages:
          - ruby
          - javascript
    ratings:
      paths:
      - app/**
      - lib/**
      - "**.rb"
      - "**.go"
    exclude_paths:
    - spec/**/*
    - "**/vendor/**/*"
    - public/assets/**


#### .git and .gitignore
![]({{ site.baseurl }}/images/102_cfg2.jpg)
