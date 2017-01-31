---
layout: post
title: Project Structure 102
published: true
---

_31/01/2017_

Welcome to the second post covering the project structure and code. In this post we'll peek in at the most important configurations and files and custom code

This description is based on the repository as it existed at the 16th of January (commit [59ebd8bbae112d6709d0affd7d0e74f1bf992510](https://github.com/CodeforAustralia/dtf-genesis/tree/59ebd8bbae112d6709d0affd7d0e74f1bf992510) to be exact) as there have been some significant changes and it's a moving target

### Rails-isms, Ruby-isms and a little about Regular Expressions
![]({{ site.baseurl }}/images/102_book.jpeg)

There are a few preliminary code structures I feel it worth getting out of the way before delving into the details of what we've done to make the CCR, we will assume basic familiarity with programming but no particular language should be required to understand the code presented.

The first of these I have grown to love in Ruby which is string interpolation, there are two significant types of strings we've used: those with single quote ```'``` and those with double quote ```"```. The single quote strings are literal strings and as far as we're concerned these never undergo interpolation and represent exactly the string that appears on screen, for example the code:

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

We also use a couple of **[regular expressions](https://en.wikipedia.org/wiki/Regular_expression#POSIX_basic_and_extended)** that I have skittishly encapsulated away into a couple of functions. These regular expressions are much simpler than the ones we began our scraping with and can be understood with familiarity of only a few regular expression characters. First up is the period ```.``` character, this character simply matches **any** non-newline character... (almost) everyone loves ```.```. Next up I feel obliged to introduce a trio, although we only use two of them in our code they are rather interrelated, these are the star ```*``` plus ```+``` and optional ```?``` regular expression **quantifiers**, they all operate on the character before them and specify how many of those characters to match. Pairing each one up with our period operator we get the three regular expressions;

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

The rest of the code has been written and arranged according to the Rails conventions for the most part and is reasonably self-documented internally. We have attempted to make the code as literate and self documenting as we could and should be reasonably easy for a moderately experienced Rails developer to grock


### Configuration
![]({{ site.baseurl }}/images/102_cfg.jpg)

Lets now have a closer look at some of the more important configuration files


#### config/environments/production.rb

This part of the production configuration (and a related segment in development.rb) define the mail server used for sending automated emails. The configuration sets up a gmail account as the mail server and defers setting the user name and password to the environment in which it is run. On Heroku there is a panel in the settings where you can set the environment variables gmail_username and gmail_password. The code segment ```ENV['gmail_username']``` gets the ```ENV``` variable provided by Rails and indexes into the value ```gmail_username``` to retrive the value set in the Heroku dashboard

    config.action_mailer.delivery_method = :smtp
    config.action_mailer.smtp_settings = {
     :address              => "smtp.gmail.com",
     :port                 => 587,
     :user_name            => ENV['gmail_username'],
     :password             => ENV['gmail_password'],
     :authentication       => "plain",
     :enable_starttls_auto => true
    }


#### config/initializers/scheduler.rb

This segment kicks off the scheduled scraping of Tenders Vic. Due to Heroku hosting putting the server to sleep when it is unused the scraping is performed during the day when the server is most likely to be active, however it would be preferable to scrape after hours if more reliable scheduling is avalable. This segment also begins with a bit of a kludge as the scheduler will error if a date from the past is given as a paramater, so if it is within five minutes of the target time it will defer the first scrape to the next day

    if Time.now > Time.parse("11:55:00 am")
      start = Time.parse("11:55:00 am") + 1.days
    else
      start = Time.parse("11:55:00 am")
    end
    daily_scrape = scheduler.every '1d', :first_at => start do #  '1h', :first_at => Time.now() + 5 do
      scrape_tenders_vic
    end

Our next bit of code triggers automated mail outs (currently all sent to my email for testing purposes) searching for all contracts that start or end today. Ideally the reporting officer could be contacted on contract start to confirm reporting dates and personnel. Then on the reporting or closing date an automated request for a report could be sent to the relevant officer

    notify_daily = scheduler.every '1d', :first_at => start do
      contracts_starting_today = Contract.where(vt_start_date: Time.now)
      contracts_starting_today.each do |cont|
        tweet_contract_start cont
        email_contract_start cont, "puzzleduck+dtf@gmail.com"
      end
      contracts_ending_today = Contract.where(vt_end_date: Time.now)
      contracts_ending_today.each do |cont|
        tweet_contract_end cont
        email_contract_end cont, "puzzleduck+dtf@gmail.com"
      end
    end


#### config/database.yml

In this extract we are looking at the database setup for the development version verses the production environment. You can observe that there are separate databases being used for development and production minimising the chance of accidentally performing development activities on the production database. We also observe the use of environment variables to set the database password, which is once again setup in the Heroku configuration dashboard

```
    development:
      <<: *default
      host: localhost
      database: genesis_development
      username: postgres
      password: q1w2e3r4t5

    production:
      <<: *default
      host: localhost
      database: genesis_production
      username: genesis
      password: <%= ENV['GENESIS_DATABASE_PASSWORD'] %>
```

#### config/secrets.yml

In our secrets file we see the recurring theme of hard coded development variables and production values being sourced from the environment variables. The secret key is used as a key for security purposes and should never be shared, for this reason the production key is encapsulated into an environment variable to prevent it ever being accidentally committed to the repository

    development:
      secret_key_base: 59f1857fda1f2aa79c7afe6233a00d11a5d65f89f69f66185467878775905ce9710de1a8da8f5e8c79d98a13189c897ed946fe40882940fad666b7fa997caa2e

    production:
      secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>


### The Data Base and schema.rb
![]({{ site.baseurl }}/images/102_erd.jpg)

At the top of the schema.rb file is the entry ```version: 20170106024818``` which specifies the last migration that was run against this database. If we create a new migration Rails will compare the time stamp in the migrations file name with this time stamp and stops the server from running until the pending migrations are run. This technique generally does a good job of synchronising the database and code so that new code is not run on an old database or vice versa

I have chosen to display the contracts entry here, but this is a big file with entries for every object in the database. As we mentioned, there is a correlation between this entry and the data stored across the migrations files and this relationship is managed by rails with the [```rails db:___``` commands](http://edgeguides.rubyonrails.org/active_record_migrations.html) such as ```rails db:migrate``` which will perform any migrations necessary (up or down) to synchronise the code and the database. ```rails db:rollback``` will reverse the last migration, ```rails db:reset``` will reset the database to its default state (be sure to backup any data first!) and there are a host of other commands for managing the database available in Rails but covering them all is a bit out of scope here

    ActiveRecord::Schema.define(version: 20170106024818) do
    ...
    create_table "contracts", force: :cascade do |t|
      t.string   "vt_contract_number"
      t.string   "vt_title"
      t.date     "vt_start_date"
      t.date     "vt_end_date"
      t.money    "vt_total_value",          scale: 2
      t.datetime "created_at",                        null: false
      t.datetime "updated_at",                        null: false
      t.integer  "vt_department_id"
      t.integer  "vt_contract_type_id"
      t.integer  "vt_value_type_id"
      t.integer  "status_index"
      t.integer  "vt_unspc_id"
      t.string   "vt_contract_description"
      t.integer  "vt_supplier_id"
      t.integer  "project_id"
      t.string   "vt_address"
      t.integer  "vt_status_id"
      t.integer  "vt_address_id"
      t.string   "vt_agency_person"
      t.string   "vt_agency_phone"
      t.string   "vt_agency_email"
      t.string   "vt_supplier_name"
      t.string   "vt_supplier_abn"
      t.string   "vt_supplier_acn"
      t.string   "vt_supplier_address"
      t.string   "vt_supplier_email"
      t.boolean  "autopurge"
      t.string   "ocds_id"
    end

### The (least important bits of the) all important app directory
![]({{ site.baseurl }}/images/102_controls.jpeg)
We'll only be having a zip through the less interesting parts of the app directory this time with a thorough coverage in the third instalment tentatively titled **"(not even almost) all about MVC frameworks"** so stay tuned.

#### app/assets/javascripts/pages.js

The ```pages.js``` code is served with static pages (those under the charge of the StaticPages controller) like the dashboard homescreen and the about page. We are looking here at the code that generates the bar chart displaying monthly contract total values. It is similar in principle to the pie chart (in that it is a data driven graph) but is simpler to examine. We start out by defining a size for the chart and calculate some bounds and margins. Then we import the data from Rails and find the maximum value for a contract to determine the maximum domain of our output

    var window_size = 400;

    window.onload = function (win) {
      var bar_margin = {top: 20, right: 30, bottom: 30, left: 40}
      var bc_width = window_size - bar_margin.left - bar_margin.right;
      var bc_height = window_size - bar_margin.top - bar_margin.bottom;

      var spending_data = ($('#spending').data('spending'));
      var barWidth = bc_width / spending_data.length;
        maximal = 0;
        spending_data.forEach(function(d) {
          if ((+d.value) > maximal) {
            maximal = d.value;
          }
      });
      maximal = maximal * 1.2;

D3 has rather good inbuilt scaling functions and here we create linear scales using the maximum value we found earlier and the width of the chart

      var heightscale = d3.scaleLinear()
          .domain([maximal,0])
          .range([bc_height, 0]);
      var widthscale = d3.scaleOrdinal()
          .range([0, bc_width], .1);

Now we start using D3 to draw something to the screen. The first line selects an empty element we placed on the dashboard and the next two define its width and height. Then we attach a title to the chart and style and place it

      var chart = d3.select(".barchart")
          .attr("width", bc_width + bar_margin.left + bar_margin.right)
          .attr("height", bc_height + bar_margin.top + bar_margin.bottom);
      chart.append("text")
           .attr("x", bc_width / 2)
           .attr("y", 20)
           .attr("class","charttitle")
           .style("font", "26px sans-serif")
           .style("fill", "steelblue")
           .text("Construction spend/month");

Now we hookup the data to our D3 chart and associate every data element with a "g" (graphic) element in our chart. The ```.enter()``` segment defines what happens when D3 finds a new element in the data set, in our case we are setting its location by performing a translation transform where we move each bar by ```i * barWidth```, thereby moving each months bar to the next adjacent position. It might seem a bit overkill in this situation (and it is), but D3 is often used with dynamic live data and we might have a chart that adds and removes columns as the data changes

      var bar = chart.selectAll("g")
           .data(spending_data)
         .enter().append("g")
           .attr("transform", function(d, i) { return "translate(" + i * barWidth + ",0)"; });

Now at each of the (ironically invisible) ```g``` objects we append a rectangle object and use the scales we made to set the width and height appropriately

      bar.append("rect")
           .attr("y", function(d) { return bc_height-heightscale(d.value); })
           .attr("height", function(d) { return heightscale(d.value); })
           .attr("width", barWidth - 1);

We then attach the month names to each of the columns and $millions values at the peak

      bar.append("text")
          .attr("x", barWidth / 2)
          .attr("y", bc_height+5)
          .attr("dy", ".75em")
          .text(function(d) { return d.name.slice(0,3); });
      bar.append("text")
         .attr("x", barWidth / 2)
         .attr("y", function(d) { return bc_height-heightscale(d.value)-15; })
         .attr("dy", ".75em")
         .text(function(d) {
           if (Math.round(d.value/1000000) > 0) {
             return "$" + Math.round(d.value/1000000) + "m";
           }
           return "< $1m";
         });

And that's all there is to it ;) seriously, we just went from data to chart & labels :D


#### app/assets/javascripts/segmentio.js

This is the full contents of the ```segment.js``` file and while it does seem a bit hefty it is a little piece of gold. With this snippet many different kind of analytics and data warehousing can be enabled through a simple web interface on the segment website. This can funnel data to or from Google Analytics, Tableax, Amazon S3 and so many more we didn't have time to experiment with them all. Many other code changes that would normally be required (for example to setup Google Analytics) are injected into the javasript that is served to respond to this code, so to enable Analytics no further code changes were necessary (and is supplied segment)

      window.analytics = window.analytics || []; // Create a queue, or get existing

      // A list of the methods in Analytics.js to stub.
      window.analytics.methods = ['identify', 'group', 'track',
        'page', 'pageview', 'alias', 'ready', 'on', 'once', 'off',
        'trackLink', 'trackForm', 'trackClick', 'trackSubmit'];

      // Define a factory to create stubs.
      window.analytics.factory = function(method){
        return function(){
          var args = Array.prototype.slice.call(arguments);
          args.unshift(method);
          window.analytics.push(args);
          return window.analytics;
        };
      };

      // For each of our methods, generate a queueing stub.
      for (var i = 0; i < window.analytics.methods.length; i++) {
        var key = window.analytics.methods[i];
        window.analytics[key] = window.analytics.factory(key);
      }

      // Define a method to load Analytics.js
      window.analytics.load = function(key){
        if (document.getElementById('analytics-js')) return;

        // Create an async script element based on your key.
        var script = document.createElement('script');
        script.type = 'text/javascript';
        script.id = 'analytics-js';
        script.async = true;
        script.src = ('https:' === document.location.protocol
          ? 'https://' : 'http://')
          + 'cdn.segment.io/analytics.js/v1/'
          + key + '/analytics.min.js';

        // Insert our script next to the first script element.
        var first = document.getElementsByTagName('script')[0];
        first.parentNode.insertBefore(script, first);
      };

      window.analytics.SNIPPET_VERSION = '2.0.9'; // version to track what's in the wild
      window.analytics.load('ACLMWlvkZzHKrlDBM4i3UBGaJf8FesSF'); // Load Analytics.js with our key
      window.analytics.page(); // Make the first page call to load the integrations

    // accommodate Turbolinks and track page views
    $(document).on('ready page:change', function() {
      analytics.page();
    })

    // track page views and form submissions TODO: update with our forms
    $(document).on('ready page:change', function() {
      console.log('page loaded');
      analytics.page();
      analytics.trackForm($('#edit_contract'), 'Edited a contract'); // ID attribute of a form and
      analytics.trackForm($('#new_contact'), 'Generated a contract'); // a name given to the event.
    })


#### app/assets/stylesheets/application.scss

The ```application.scss``` file defines styles that are shared across the entire application. Just like the ```pages.js``` file there are also scss files for each controller, but customising them too much can lead to an inconsistent user experience so most significant changes are made in the site wide ```application.scss```. First up we define some neat coloring and transparency for the site using fancy scss functions. Then we define a color scheme that is used throughout the styling for consistency and I have included a couple of examples below

    $orange: rgb(250,70,00);
    $alpha75: .70;
    $alpha50: .5;
    $alpha25: .25;
    $orangeAlpha75: rgba($orange, $alpha75);
    $orangeAlpha50: rgba($orange, $alpha50);
    $orangeAlpha25: rgba($orange, $alpha25);

    $main-text-color: #222200;
    $logo-color: #222200;
    $header-background: $orangeAlpha75;
    $main-background-color: #DFDAD3;
    $input-color: $orangeAlpha75;
    $input-active-color: $orangeAlpha50;
    $input-alt-color: #222200;
    $input-active-alt-color: #000000;

    ...

    .container {
      color: $main-text-color;
      width: 99%;
    }

    .navbar {
      background-color: $header-background;
    }


#### app/assets/mailers/application_mailer.rb

This little snippet composes and sends a simple proof of concept email reminder and is triggered by the scheduled events we had a look at earlier. This simple example just sends a static string to a single email address, but could easily be customized to email the reporting officer a link to the reporting form that needs to be completed

    class ApplicationMailer < ActionMailer::Base
      default from: 'benjamin@codeforaustralia.org'

      def announce_email(contract, email)
        puts "mailer CON:#{contract}"
        @contract = contract
        mail(to: email, subject: "Sample email", body: "not much yet")
      end
    end

#### robots.txt and meta data (the good kind)
![]({{ site.baseurl }}/images/102_robot.jpeg)
We have not explored the use of the robots.txt file in our project as people and robots scraping our site was not an issue that came up. However I feel it is pertinent to mention the file and its function in web culture and communication norms in discouraging errant web crawler activities

Meta data is something that we have also not had an opportunity to explore but would be great for improving government interface design, the metadata information is used in modern platforms like Slack and Twitter to display fancy previews of the webpage that can be embedded or shared with other users. For example the departments logos could be used as the preview image, the contract title as the header and a body made up of the most vital details such as start, end and value


### Gemfile and bundle
![]({{ site.baseurl }}/images/102_rubies.jpg)

We have used many libraries during the development process and these are specified in the file named "Gemfile". This file is most often processed by the ```bundle``` command which manages the Ruby packages (or gems) installed on the system. We examine just a few snippets of the file below

We specify the versions of Ruby and Rails that our project uses so that the ```bundle``` program can take care of managing which versions to run

    ruby '2.3.1'
    gem 'rails', '~> 5.0.0', '>= 5.0.0.1'

Here we specify that we are using the PostgreSQL database and the Puma web server which are minor upgrades to the default SQLite and WebBrick infrastructure used in Rails by default

    gem 'pg', '~> 0.18'
    gem 'puma', '~> 3.0'

The capybara and poltergeist libraries are used by our scraper to power a phantomjs browser for Tenders Vic and the rufus-scheduler library is used to automate the daily scraping routine

    gem 'capybara', '>= 2.1.0'
    gem 'poltergeist', '>= 1.9.0'
    gem 'phantomjs', :require => 'phantomjs/poltergeist'
    gem 'rufus-scheduler'

The last segment here demonstrates dependency groups in the Gemfile. These requirements are only used in specific configurations or environments. This example is only used during test commands so our production environment won't have these requirements installed. Guard and Minitest give us an improved and flashy interface when we run the tests. Scrutinizer and Capybara Screenshot are currently underutilized but provide for enhanced testing capabilities such as logging screen shots during tests for visual diffs and monitoring general code quality

    group :test do
      gem 'minitest-reporters',       '>= 1.1.9'
      gem 'guard',                    '>= 2.13.0'
      gem 'guard-minitest',           '>= 2.4.4'
      gem 'capybara-screenshot'
      gem 'scrutinizer-ocular'
    end


### Build configuration files (CI/CD)
![]({{ site.baseurl }}/images/102_timing.jpg)
(, .scrutinizer.yml, .travis.yml)
We have experimented with many continuious integration and analytic tools during the fellowship and I would highly recommend the segment.io and semaphore platforms for future projects. I have already covered segment, and semephore is the developers equivilent... with a simple setup and configuration you get automated builds, testing and deployment on every ```git push``` on a public repository (a massive time saver)

#### .codeclimate.yml

The Code Climate service monitors you codebase and reports on code quality metrics, of course all metrics must be taken with a grain of salt but hopefully you'll see the value in monitoring relative values over the lifetime of a project. For example monitoring the number of "fixme" and "todo" strings in a codebase can give you a sense of the ammount of work being delayed or put off. There may be good reasons or stylistic differences between developers that make any individual value unreliable, but by combining and aggregating general trends we can see when **too much** work is being deferred or back loaded.

The ratings-path segment specified which code should be analysed for quality (excluding auto-generated files for example) and the exclude segment prevents external dependencies and tests from being included in the analysis at all

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

The .gitignore file is used to flag files as not appropriate for version control. In the good-old-days this is where you would exclude any files that contained sensitive information, but as you have already seen this has been replaced in modern code with environment variables that can't be accidentally leaked. We have excluded a few sensitive data files such as performance report raw data and other restricted information (perhaps a local directory referenced by an environment variable might be a better precaution). Then a few of the more regular development artefacts such as the database itself, log files and cross reference indexes are also excluded from the repository here

    # private file of public data
    TendersVicDataSingleLine.csv
    db/backup/CprPerformanceReport.csv
    db/backup/CsrPerformanceReport.csv
    db/backup/Contract.csv
    db/data/performance.csv

    # ignore cscope/tags files
    *.db
    *.files
    *.out
    tags

    *.gem
    *.rbc
    /.config
