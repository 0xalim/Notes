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

## Microservice Two Requirements
1. take 2 input, generate 2 output
1. again, requires some sort of theme. given theme: 
 enter startDate endDate, computer totalDays totalWeeks
1. use express.js again (l2p2C)
1. test locally with gcs, make sure it works correctly
1. deploy on heroku (2nd cloud service)
1. 1 repository

* https://cca-teamash-microservice2.herokuapp.com/
* different repository;

## Front-end UI
1. Given cca-html, and ajax requests.
1. 1 input+button to call microservice 1 -> display output
1. 2 input+button to call microservice 2 -> display output
1. UI in github
1. 1 repository
