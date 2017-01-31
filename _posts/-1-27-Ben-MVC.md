---
layout: post
title: Model, View, Controller<strike>, Party</strike>
---

_27/01/2017_

In a third we'll take a closer look at the ```app/``` directory and see how this relates to the MVC architecture of rails

Finally we'll end our journey by looking in at a few of the views used in the application and how they relate to their related controllers and models

This description is based on the repository as it existed at the 16th of January as there are currently errors in the CCR deployment



### Take the red pill and see how the MVC is structured

Intro to app structure

```
app/
├── controllers/
│   ├── contracts_controller.rb
│   ├──     "    _controller.rb
│   └── suppliers_controller.rb
├── helpers/
│   ├── contracts_helper.rb
│   ├──     "    _helper.rb
│   └── value_types_helper.rb
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


### Setup

### Rake Tasks
