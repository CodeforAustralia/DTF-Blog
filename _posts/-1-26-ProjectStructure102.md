---
layout: post
title: Project Structure 102
published: true
---

_26/01/2017_

In the second post we'll peek in at the most important configurations and files and custom code

In a third we'll take a closer look at the ```app/``` directory and see how this relates to the MVC architecture of rails

Finally we'll end our journey by looking in at a few of the views used in the application and how they relate to their related controllers and models

This description is based on the repository as it existed at the 16th of January as there are currently errors in the CCR deployment

### Rails-isms


### Documentation
├── erd.pdf
├── LICENSE


### Configuration


├── .codeclimate.yml
├── .scrutinizer.yml
├── .travis.yml
├── Gemfile

config/
├── environments/
│   ├── production.rb
├── initializers/
│   ├── scheduler.rb
├── application.rb
├── database.yml
├── environment.rb
├── puma.rb
└── secrets.yml

db/
├── schema.rb

app/
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


####  (secrets & environment... robots and meta)

#### Gemfile and bundle

#### Build configuration files (CI/CD)

#### .git and .gitignore
