# Home task for Java Engineer role
This is Project wireframe for home task

Required dependency documentation inside [HELP.md](HELP.md)



## Introduction
This is the home task for the role of the  developer in pawaTech.
We are happy that you expressed interest in this role and would like to see your ability to code, learn and ask questions.
We are not expecting you to implement everything listed in the task, but it’s really important to try.

## Before you start to implement the task
Please read all the instructions for the task and provide your estimation for the MVP of the task.
With estimation please provide feedback about the complexity.
Don’t forget to ask questions if something is unclear for you.

NB! Questions made on Friday will be answered no earlier than next week.

## Overview

In this task, you will work with technologies that we are using in the company on a daily basis.
The main idea is to implement a small multi-service project that emulates the accepting bets.

You will be required to implement 2 micro-services. Communication between services will be made over gRPC

To help you begin, we are providing you wireframe project with existing wallet service module and API required to implement.
You have semi implemented REST endpoints and Unimplemented Java interface. Grpc server already has own API.
There is solved some issue related gRPC generation in different platforms.


## Mandatory
Use listed required technologies; we know that a lot of new and fancy technologies exist, but please follow the requirements.

* Don’t overcomplicate design.
* Don’t implement a multi-currency system.
* Authentication should be trivial - return account id when credentials are correct. API provided in wallet service.

## Requirement
Implement 2 microservices: **betting** and **wallet**.

**Betting service responsibilities:**
* Accept bets
* Get information about placed bets for account
* Get bet slip full details
Basically, a “bet slip” is an abstract “ticket” with some monetary value in the scope of this task. If you know/learn anything about betting and odds, and show this knowledge somehow, it’s a plus.



**Wallet service responsibilities:**
* Create account
* Deposit and withdraw money
* Get account balance
* Provide internal gRPC API to create transactions for bet placement
* Provide internal gRPC API to create transactions for bet winning
* **Update all REST and gRPC api to support decimals for money flow**
 


## Database
Database schema migration should be done with liquibase.
The database is MySQL only.
## Technologies requirements
* Java 17+
* Spring Boot 2.5+
* Spring JDBC or Spring JPA
* MySQL 8
* gRPC and Profobuf
* Gradle
* JUnit
* Git and host at GitHub 

**Extra points for technologies:**
* Docker
* Test containers for integration tests
* CI over Github actions
API test or a client to demonstrate behavior




## Server requirements:
* General technical requirements:
* REST endpoints with JSON
* Internal communication between services using gRPC
Error handling should be generic for all rest endpoints using an error informative error code. (like “EMAIL_EXISTS”)

## Wallet service flows:
Via REST:
* Create an account and return the account ID for a specific email
* Using account ID create a Deposit to the account
  * The deposit amount could be only a positive value
  * Return current balance state after deposit
* Using account ID request money Withdrawal
  * The withdrawal amount could be only a positive value
  * You can’t take more money than you have on your balance
* Using account ID request for current balance state
* List of operations made for the account
  * Operation type: deposit, withdrawal, bet, win
  * When it was
  * Amount transferred

## Betting service flows:
* Accept bets
  * You should accept list of bet item IDs ( imaginary outcome ids, in real life it could be like you chose that Tallinn win Tartu, or vice versa, or 
  draw)
    * Same id could be in different bets, but not in same
  * With each ID you should have some Odds ( multiplier of your stake) and it should be more than 1
  * You should have some positive stake
  * Account should have money on the wallet
  * Reserve money from account on the wallet microservice
  
* Get single bet details
  * Provide bet placement datetime
  * Total odds ( multiplication of bet items odds)
  * List of items ( id and odds, status ( pending , win, loose) )
  * Show settled status win/lose

* Get the list of bets
  * List of bet slips
  * Show settled status win/lose of each betslip
  * Show total odds of each betslip
  * Show bet placement datetime
* Management endpoint - settlement
  * Settle specific bet item by Id - possible status is WIN or LOSE
  * Update all bet items with id and set their status
    * Only those bet items that have status PENDING
  * Get all affected betslip by this settlement:
    * If betslip is PENDING and bet item has status LOSE, then mark betslip with result LOSE and mark status as SETTLED
    * If betslip is PENDING and bet item has status WIN then:
      * All bet items have status WIN then:
        * mark betslip with status WIN and status SETTLED
        * Add money to the wallet account = total odds * stake
      * Some bet items has status PENDING, don’t do anything with betslip
* Return via API stats:
  * How many betslips affected
  * How many betslips won
  * How many betslips lost
  * How many betslip still pending from affected



## What will give more points
* Test coverage unit and integrations
* Added Docker and Docker compose tooling to simplify local start
* Non-blocking REST and Grpc operations.
* Swagger for REST endpoints

## Project delivery
* The project should be shared via GitHub - it should be your repository.
  * Public
  * Private, then we will proviate user, who will review you task and you should grand access to it
* The project should have git history, non-single commit delivery
* The project should have documentation
  * How to start a project on an empty machine
  * How to test and validate task
* Instructions to verify some test cases that you are designed should be easily reproducible
  * A simple script that executes something
  * List of CURLs that could be copy/pasted to terminal
* Nice to have:
  * Docker compose file that helps to start all the services locally
