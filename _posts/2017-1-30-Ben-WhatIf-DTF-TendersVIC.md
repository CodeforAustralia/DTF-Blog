---
layout: post
title: What If... The DTF fellowship was with Tenders Vic?
---

_30/01/2017_

### What if... the DTF fellowship was with Tenders Vic?
![]({{ site.baseurl }}/images/spider-hi.jpeg)
I wanted to try exploring some of the blockers we experienced working at the Department of Treasury and Finance and show off some auxiliary work I've been able to achieve by looking at what might have happened in a subtly different universe... so subtle that it's almost the universe we live in now. The DTF public servant who formed the challenge we participated in and championed its progress through the process has recently been reassigned into another unit in the same grouping as Tenders VIC. This has already resulted in increased contact between our team and the Tenders Vic team and if the challenge was being formed now it would most probably be identified as the biggest bottleneck worth working on. I'm guessing that our project challenge would have been a government spending dashboard using the tenders Vic data, much like what we've been able to create for the construction group


### Are you sure you don't want me to scrape anything?
![]({{ site.baseurl }}/images/spider_ready.jpeg)
With my preferred task unnecessary I wonder what I would have done early in the project. We would have still been prototyping a UI on paper so that much would have been much the same. What did change in the early stages of the project was that we already knew what data we had to work with and had sample extracts and example data sets to play with. We also could have built on prior open source "government spending dashboards" like the UKs [gov.uk](https://www.gov.uk/performance/g-cloud) and Australias own [LoveMeTender](http://lovemetender.com.au/)

It would have also been exciting to consider an "all levels of government" dashboard by combining council budgets, Tenders Vic contracts and Aus Tenders contracts. This dataset could be combined with the [Planning Alerts](http://planningalerts.org.au/) data sets to create a "whole of economy" view of construction across Victoria. This would allow departments to monitor the general market trends and better predict the best moment to enter the market based on industry capacity and utilisation

There was a few simple and easy quick wins working with the Tenders Vic group to improve the site in-situ. Simply adding an appropriatly crafted [robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard) improved the searchability of the site and adding appropriate [meta tags](http://www.w3schools.com/tags/tag_meta.asp) to the headers of pages made links and shares of contracts more useful and presentable. Then adding social media links and share buttons only required cosmetic changes


### Getting a dataset onto data.vic.gov
![]({{ site.baseurl }}/images/spider-meeting.jpeg)
It was always interesting talking to the Tenders Vic people and demonstrating the amazing things that could be done with the data when it's put into the right hands. In this world I was in some of the meetings where they decide that updating the current system would cost too much and we'd better just keep using it... like those are the only options. That's about when I jump up and yell "No dammit!, every day you continue to use this system costs you hours of manual fiddling and costs other departments hours of uncertainty and agony!", I'd take a deep breath and compose myself, straighten my shirt and gently cough, before blurting out "don't you see that people give up because this system is just too unwieldy, they give up on getting the right data for the decision they need to make, they give up on getting timely information on the projects that matter to their units, they just start to give up on the public service as a whole and come to expect this level of service, and this costs Australia more and more and more in wasted productivity every day it's allowed to continue!"... and that's about when they kick me out of those meetings

I suppose because of the small wins we'd made early, despite being kicked out of the meetings, Tenders Vic was convinced that something could be done now without spending a small fortune on an upgrade. One of the Tenders Vic crew stepped up to become custodian of the Tenders Vic data and then it was just a matter of writing a tool on the Tenders Vic platform to access the database and upload onto a morph.io database. This then unlocked the download and API functionality to get onto the Data.Vic platform which we had to get by web scraping and [freedom of information requests](https://www.righttoknow.org.au/request/contracts_published_on_tenders_v) in the normal world


### Linking Tenders with Contracts
![]({{ site.baseurl }}/images/spider-link.jpeg)
With the Tenders Vic data now on Data.Vic we were then able to turn our attention on the linking of tenders and contract data. Thankfully in this parallel world we still had the same interview opportunity with the PRU team and just like in our universe the significance of the interview passed us by on the day. However in this universe during our next sprint planning session we all agreed that writing a little app for the PRU was a huge priority and we realised that by writing a little app here we could have big wins in terms of reducing data entry errors, providing links to the related tenders of a contract, tracking the project a contract falls under and breaking out address information where relevant


### Mapping the Future
![]({{ site.baseurl }}/images/spider_maps.jpeg)
After creating the PRU tool for transferring tenders to contracts the Code for Australia fellowship was starting to wrap up and we only had a few weeks to go could we use the newly available address data to create a map of government spending across Victoria. Unfortunately we made the mistake of rushing and ended up committing the cardinal sin of software development by breaking the build... several times, often before demonstrations with important stakeholders resulting in sketchy feedback. While a more cautious and incremental approach might have yielded some fruit by the end of the fellowship our rushed attempt had to be abandoned and we had to roll back to a prior stable version loosing quite a bit of work and having nothing new to demonstrate in our final sprint showcases, dampening enthusiasm for the program as a whole

![]({{ site.baseurl }}/images/spider_crystal.jpeg)
