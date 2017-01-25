---
layout: post
title: What If... The DTF fellowship was with Tenders Vic?
---

_25/01/2017_

### What if... the DTF fellowship was with Tenders Vic?
![]({{ site.baseurl }}/images/IF_6.jpg)
I wanted to try exploring some of the blocks we experienced at DTF and show off some auxilary work I've been able to achieve by looking at what might have happened in a subtly different universe... so subtle that it's the universe we live in now. The DTF public servant who formed the challenge we participated in and championed its progress through the process has recently been reassigned into another unit in the same grouping as Tenders VIC. This has already resulted in increased contact between our team and the Tenders Vic team and if the challenge was being formed now it would probably be identified as the bottleneck worth targeting. I'm guessing that our project challenge would have been a government spending dashboard using the tenders Vic data


### Are you sure you don't want me to scrape anything?
![]({{ site.baseurl }}/images/IF_7.jpg)
With my preffered task unneccessary I wonder what I would have done early in the project. We would have still been prototyping a UI on paper so that much would have been much the same. What would have changed in the early stages of the project is that we would have already know what data we had to work with and have sample extracts to play with. We also could have built on prior open source "government spending dashboards" like the UKs [gov.uk](https://www.gov.uk/performance/g-cloud) and Australias own [LoveMeTender](http://lovemetender.com.au/)

It would have also been exciting to consider an "all levels of government" dashboard by combining council budgets, Tenders Vic and Aus Tenders data. This dataset could be combined with the [Planning Alerts](http://planningalerts.org.au/) data sets to create a "whole of economy" view of construction across Victoria. This would allow departments to monitor the general market trends and better predict the best moment to enter the market based on industry capacity and utilization


### Getting a dataset onto data.vic.gov
![]({{ site.baseurl }}/images/IF_8.jpg)
It was always interesting talking to the Tenders Vic people and demonstrating the amazing things that could be done with the data when it's put into the right hands. I wish I could have been in the meetings we keep hearing about where they decide that updating the current system would cost too much and we'd better just keep using it. I wish I could have been there, so I could jump up and yell "cow-dung! dammit, every day you continue to use this system costs you hours of manual fiddeleing and costs other departments hours of agony!", I'd take a deep breath and compose myself, strighten my shirt and gently cough, before blurting out "don't you see that people give up because this system is just too clunky, they give up on getting the right data for the descision they need to make, they give up on getting timely information on the projects that matter to their units, they just start to give up on the public service as a whole and come to expect this cr@$ which costs Australia more and more and more in wasted productivity in the long run!"... and that's about when they kick me out of those meetings

Despite being kicked out of the meetings I'm assuming one of the Tenders Vic crew would step up as custodian of the Tenders Vic data (that, or we have another shouting match) and then it's just a matter of writing a tool on whatever platform Tenders Vic runs to access the data and upload onto a morph.io database. This then unlocks the download and API functionality we've had to expose through scraping and gives us data accessible enough to get onto the Data.Vic platform


### Linking Tenders with Contracts
![]({{ site.baseurl }}/images/IF_9.jpg)
With the Tenders Vic data now on Data.Vic we were then able to turn our attention on the linking of tenders and contract data. Thankfully we still had the same interview opportunity with the PRU team and just like in our universe the significance of the interview passed us by on the day. However in this universe during our next sprint planning session we all agreed that writing a little app for the PRU was a huge priority and we realized that by writting a little app here we could have big wins in terms of reducing data entry errors, providing links to the related tenders of a contract, tracking the project a contract falls under and breaking out address information where relevant


### Divergent Futures
![]({{ site.baseurl }}/images/IF_10.jpg)
After creating the PRU tool for transferring tenders to contracts the Code for Australia fellowship was starting to wrap up
