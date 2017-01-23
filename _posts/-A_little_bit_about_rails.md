---
layout: post
title: Take the red pill and see how deep we go
---

_12/01/2017_

_Some image text_

![]({{ site.baseurl }}/images/Rails_1.jpg)

### Project structure

In this blog we will take you through a whirlwind tour of the application we've built during the fellowship, explain some of the conventions we've followed and where we've written our own playbook. We'll begin by having a look at the top level directory structure in our project and peek in at the most important configurations and files. Then we'll take a closer look at the ```app/``` directory and see how this relates to the MVC architecture of rails. Finally we'll end our journey by looking in at a few of the views used in the application and how they relate to their related controllers and models

#### App

```
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
```

#### Config (secrets & environment... robots and meta)

```
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
```

#### DB

```
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
```

#### Gemfile and bundle

#### Build configuration files (CI/CD)

#### .git and .gitignore

#### tests
```
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
```


### App structure

Intro to app structure

```
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
```

#### MVC

#### libraries (internal vs external)

#### Assets & pipeline

#### Helpers, mailers, jobs, ...



### View structure

As the contract view has a few unique properties let's start off looking at the departments views for a picture of how a view normally looks and behaves

#### partials

#### multiple extension processing pipeline

#### erb/haml/scss/sass



### Code for Australia Ignite talks

_My Ignite talk on Electronic Gaming Machines_

<iframe width="560" height="315" src="https://www.youtube.com/embed/VDg2JiLcEO8" frameborder="0" allowfullscreen></iframe>


_Some other image text_

![]({{ site.baseurl }}/images/Rails_2.jpg)
