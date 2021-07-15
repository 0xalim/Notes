# Sprint 1

sprint one is basic in the sense of, not much skill needed to write lots of code
and such. we use everything that's been provided (ajax for ui; express.js for ms).
but theres lots to setup first, multiple accounts and repositories.

idea is to have 2 microservices and 1 frontend ui that interact with both 
microservices. to be more familiar with this microservice thinking protocol
instead of monolithic big programs that fail. so we will be using as many 
services as we can (azure, heroku, github(bitbucket.io for dr.)).

## Microservice One Requirements
1. take 1 input, generate 2 output
1. require some sort of idea to go with the above;
 like chinese zodiac year provided by the dr.
1. use express.js (l2p2C)
1. test locally with gcs, make sure it works correctly
1. deploy on azure (1st cloud service)
1. 1 repository

* https://teammate1-ash.azurewebsites.net
* different repository {ash812763/cca-teamash-microservice1}

## Microservice Two Requirements
1. take 2 input, generate 2 output
1. again, requires some sort of theme. given theme: 
 enter startDate endDate, computer totalDays totalWeeks
1. use express.js again (l2p2C)
1. test locally with gcs, make sure it works correctly
1. deploy on heroku (2nd cloud service)
1. 1 repository

* https://cca-teamash-microservice2.herokuapp.com/
* different repository {git.heroku.com/cca-teamash-microservice2.git}

## Front-end UI
1. Given cca-html, and ajax requests.
1. 1 input+button to call microservice 1 -> display output
1. 2 input+button to call microservice 2 -> display output
1. UI in github
1. 1 repository

* https://cca-teamash.bitbucket.io/
* different repository {bitbucket}

## Tasks:
1. [x] migrate to gcs
1. [x] create micro1 {azure}
1. [x] create micro2 {heroku}
1. [x] create ui {bitbucket}
1. [x] work on micro1 {express.js}
1. [x] work on micro2 {express.js}
1. [x] link micro1 with ui {ajax: in vid l5}
1. [x] link micro2 with ui {ajax: in vid l6}

## Completed: A few words
whole project is not hard; think the idea of this was to let people group up
together and teach them more of git usage, working together, and the whole idea
of using front+back(end) for the first time - for some people.

entire of sprint 1 is not hard at all - people can pull off little to no work if
they wanted to. every piece of code is there, the only thing people needed to 
actually put some effort in was the implementation of the microservices. which,
honestly is not bad amount of work now that I look at it.

For microservice 1 i did a ASCII -> Binary+Hex converter. very simple calculator
thankfully javascript has a couple of inbuilt functions (mainly .toString(x,rdx))
that could auto convert to hex.

for microservice 2 i took 2 ASCII input -> Add them/Subtract them. this was 
actually easier than microservice 2. i just made 3 pairs of 2 variables for,
input, hex to decimal. and then return the values.

implementing to the ui was a bit more tricky than expected. i had errors in my 
initial html file so i had to fix those. i initially started this sprint with the
idea of keeping everything on github/heroku but had to migrate ui to have 
the frontend ui. if i had followed the exact format for bitbucket,micro1,micro2
then it would've been no hassle. 

i've got an issue outputting my 2i2o microservice giving a status of 304. not sure
what that's all about but not in the mood to fix it so i called it done. in the 
response header you could see the values anyways so i didn't care and will let it
be until i actually take this course.

everything is currently pushed to github under cca-teamash-microservice1; 
cca-teamash-microservice2; cca-teamash

