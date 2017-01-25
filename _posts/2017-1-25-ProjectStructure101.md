---
layout: post
title: Project Structure 101
---

_25/01/2017_

In this blog we will take you through a whirlwind tour of the application we've built during the fellowship, explain some of the conventions we've followed and where we've written our own playbook. We'll begin by having a look at the top level directory structure in our project and mention the most important configurations and files

In the second post we'll peek in at the most important configurations and files

In a third we'll take a closer look at the ```app/``` directory and see how this relates to the MVC architecture of rails then we'll end our journey by looking in at a few of the views used in the application and how they relate to their related controllers and models

This description is based on the repository as it existed at the 16th of January as there are currently errors in the CCR deployment


### Getting the source
![]({{ site.baseurl }}/images/101_src.jpg)
_It's fun... if you like source code_

The source code for the project and standalone scraper can be found at the following locations, visit these pages in a browser to view and explore the github projects online

[https://github.com/CodeforAustralia/dtf-genesis](https://github.com/CodeforAustralia/dtf-genesis)
[https://github.com/PuZZleDucK/VIC_Tenders](https://github.com/PuZZleDucK/VIC_Tenders)

In order to work on your own copy of the source you can fork your own copy of the repository or just clone a copy of ours by running the following commands

~~~ shell
$ git clone https://github.com/CodeforAustralia/dtf-genesis.git
$ git clone https://github.com/PuZZleDucK/VIC_Tenders
~~~
{: linenos="true" }

### Application root directory
![]({{ site.baseurl }}/images/101_1.jpg)
_You gotta start somewhere_

~~~
.
├── .git/
├── app/
├── bin/
├── config/
├── coverage/
├── db/
├── lib/
├── log/
├── public/
├── test/
├── tmp/
├── vendor/
├── .buildpacks
├── .codeclimate.yml
├── .gitignore
├── .ruby-gemset
├── .ruby-version
├── .scrutinizer.yml
├── .travis.yml
├── config.ru
├── erd.pdf
├── Gemfile
├── Gemfile.lock
├── LICENSE
├── Procfile
├── Rakefile
└── README.md
~~~

Some preliminary explanations about file and directory structure are probably in order. One of the more arcane customs are the files and directories beginning with a "." also referred to as "dot files", this is a unix convention for hiding files and is often used for configuration files a user is unlikely to require or use often

In particular the ".git/" directory is a very special directory and should not be created or edited manually. It is the directory the git revision control program uses to store all the versions, branches, tags and history of your source code. This directory is created automatically when you run the "git clone" commands above and are unique to your system and shout not be committed into the repository or shared in any way... which is a nice segway into the ".gitignore" file, which lists a bunch of files and directories that git should not backup which could range from system dependant configurations, or log files through to sensitive data files

The files with the extention ".md" are called "markdown files", these are text files with a little bit of extra formatting information allowing them to be easily converted into documents or web pages while still being readable in plain text. The ```README.md``` file contains much introductory material on setting up your own instance of the CCR system

We'll delve into the ```.codeclimate.yml```, ```.scrutinizer.yml```, ```.travis.yml``` and ```Gemfile``` in the "Project Structure 102" blog


### Config
![]({{ site.baseurl }}/images/101_2.jpg)
_Choices, choices, choices_

~~~
config/
├── environments/
│   ├── development.rb
│   ├── production.rb
│   └── test.rb
├── initializers/
│   ├── active_admin.rb
│   ├── active_record_extension.rb
│   ├── application_controller_renderer.rb
│   ├── assets.rb
│   ├── backtrace_silencers.rb
│   ├── clear_logs.rb
│   ├── cookies_serializer.rb
│   ├── devise.rb
│   ├── filter_parameter_logging.rb
│   ├── inflections.rb
│   ├── mime_types.rb
│   ├── new_framework_defaults.rb
│   ├── scheduler.rb
│   ├── session_store.rb
│   ├── smart_listing.rb
│   └── wrap_parameters.rb
├── locales/
│   ├── devise.en.yml
│   └── en.yml
├── application.rb
├── boot.rb
├── cable.yml
├── database.yml
├── environment.rb
├── puma.rb
├── routes.rb
└── secrets.yml
~~~

The configuration directory contains all kinds of important (no supprise) settings used by the application and hosting provider. The ```environment``` directory contains settings that may change between different deployment targets such as 'production' or 'testing'. Most of the ```initializers``` remain unused by us with the exception of ```scheduler.rb``` which kicks of recurring tasks such as daily web-scraping which we'll look at closer in 102. The ```locales``` directory contains internationalisation settings which we have not utilized

Other crucial ones we'll look at in depth next time are ```production.rb```, ```application.rb```, ```database.yml```, ```environment.rb```, ```puma.rb``` and ```secrets.yml```

### DB
![]({{ site.baseurl }}/images/101_3.jpg)
_Data is like mortar only it's incorporeal and doesn't stick thinks together_

~~~
db/
├── backup/
│   ├── ....csv
│   └── ....csv
├── data/
│   ├── agencies.csv
│   ├── contract-status.csv
│   ├── contract-types.csv
│   ├── council.csv
│   ├── csr_contract.csv
│   ├── csr_referee_contact.csv
│   ├── performance.csv
│   ├── supplier.csv
│   ├── TendersVicDataSingleLine.csv
│   ├── tendersvic_mock_contracts.csv
│   └── unspsc.csv
├── migrate/
│   ├── 20160920073558_create_contracts.rb
│   ├── 20160927043201_   "       "  .rb
│   ├── 20160927044609_   "       "  .rb
│   ├── 20160927045940_   "       "  .rb
│   └── 20170106024818_create_ccr_performance_reports.rb
├── schema.rb
└── seeds.rb
~~~

The ```db``` directory is also resonably self explanitory as the place to go for any database information. The ```backup``` and ```data``` directories contain various ".csv" files used to load or dump data to and from the CCR. The seeds file contains a few simple data items required to setup the CCR such as tables for the status codes and associated text

The ```migrate/``` directory and ```schema.rb``` file have a special relationship that Rails attempts to maintain. These two sources should describe the same database structure via two different means, the migrations describe how to build the database step by step as it was done throughout the project complete with renaming and reversed decisions. The ```schema.rb``` file contains the current state of the database and will be equivalent to running all the migrations and dumping the database state. We'll look a bit closer at the schema file in 102

#### tests
![]({{ site.baseurl }}/images/101_4.jpg)
_Just like walls, tests give you security and a fuzzy warm feeling_

~~~
test/
├── controllers/
│   ├── contracts_controller_test.rb
│   ├──    "     _controller_test.rb
├── fixtures/
│   ├── contracts.yml
│   ├──    "     .yml
├── helpers/
├── integration/
│   ├── linker_test.rb
│   └── scraper_test.rb
├── mailers/
├── models/
│   ├── contract_test.rb
│   ├──     "   _test.rb
└── test_helper.rb
~~~

The test directory contains all the setup, data and code to run and verify the tests. The tests for this project are minimal and simple and do not guarantee program correctness, but they are a handy sanity check and safety rope to use during program development

### App structure
![]({{ site.baseurl }}/images/101_5.jpg)
_With the foundations prepared we were ready to start adding "capabilities"_

~~~
app/
├── admin/
│   ├── contract.rb
│   ├──    "    .rb
│   └── user.rb
├── assets/
│   ├── config/
│   ├── images/
│   ├── javascripts/
│   │   ├── application.js
│   │   ├── charts.js
│   │   ├── segmentio.js
│   │   ├──     "    .js
│   └── stylesheets/
│       ├── application.scss
│       ├──      "     .scss
├── channels/
│   └── application_cable/
├── controllers/
│   ├── contracts_controller.rb
│   ├──     "    _controller.rb
│   └── suppliers_controller.rb
├── helpers/
│   ├── contracts_helper.rb
│   ├──     "    _helper.rb
│   └── value_types_helper.rb
├── jobs/
│   └── application_job.rb
├── mailers/
│   └── application_mailer.rb
├── models/
│   ├── contract.rb
│   ├──     "   .rb
│   └── user.rb
└── views/
    ├── contracts/
    ├──     "  /
    └── users/
~~~

We'll delve into the ```app``` directory heavily when we cover the Model, View, Controller paradim in the third article detailing the CCR system so I'll cover the ```models```, ```views```, ```helpers``` and ```controllers``` directories in detail then. The ```assets``` directory contains the images, styles and javascript included in almost every page on the site and we'll have a look at the most important of these in the 102 section. The ```admin```, ```channels```, ```jobs``` and ```mailers``` directories are not utilised in our project and not covered in detail.

Hope you enjoyed this overview and will be back soon to see the gritty details inside some of these files in Project Structure 102

![]({{ site.baseurl }}/images/101_6.jpg)
_Oh my, what have we created!_
