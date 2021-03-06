⚠️ *This repository is no longer active. Together with other strands of work it is further pursued here: [FIN4XPLORER](https://github.com/FuturICT2/FIN4XPLORER)*

<hr>

# GenesisApp

Mobile Application, which allows the user to create, deploy, utilize and obtain tokens.
It uses the Genesis Library. If you are interested in the library, check out: www.github.com/FuturICT2/Genesis. In case of questions,  please contact: bmark@ethz.ch.

# Content

- [Installation](#installation)
- [AppFunctions](#appfunctions)
- [First Run](#firstrun)
- [Implement own functionalities](#ownfunctionalities)
- [Http requests to Fin4 server](#fin4repo)


<a name=installation/>

# Installation

* clone the repository
* install adroid studio: https://developer.android.com/studio/index.html
* open the project with android studio
* Connect a phone through via usb or start an emulator
* Run the project, it will be deployed to your phone or emulator

<a name=appfunctions/>

# Functions of GenesisApp

The app currently consists of four views. These will be explained in the following. 

## Token creator

The creator view allows the user to create a new token with different properties, underlyings and operations. Currently only basic properties can be defined:
* name: Name of the Token
* symbol: Short string representation of the token name. I.e. BTC is the symbol of bitcoin
* Maximum supply: Token cap. I.e. bitcoin has a token cap of 21 million. 
* Decimals: How many decimal position should the token have. I.e. Euro has two (100 cents)
* Genesis supply: Number of tokens which were pre-mined

In Challenge 1: You can use this view to directly store your created tokens to the blockchain of your chosing. 

## Wallet

In future all different types of tokens will be displayed and the balance of the user will be shown. Currently, it only lists the tokens existing in the local SQLite datase

## Token Obtainer
In future, the user will be able to select a token in this view. This token will then dipslay an action (task), which the user has to perform. If the action is accepted as true, the user will be rewarded with tokens.

In Challenge 2, you can use this view to test your implementation and to integrate it with other parts of the tool.

## Projects
In future, the user will be able to trade his or her
tokens in this view. Moreover, projects can be listed, on which the user can spend his or her tokens (crowdsourcing etc.)
 
 <a name=firstrun/>
 
# First Run
* after starting the app, you will see your empty wallet
* swipe left to the Creator
* For the moment, you can specify in this view the (basic) properties of the token you want to create. Make sure to save each input.
* After saving the Token ("save Token"-button), it will be shown in the wallet overview. Just swipe back (right) to the wallet view.

<p align="center">
  <img src="./screenCreator.png" width="350"/>
  <img src="./screenWallet.png" width="350"/>
</p>

<a name=ownfunctionalities/>

# How to utilize the App to implement your own Functionalities (BIOTS Challenges 2018 - still interesting for BETH 2019)

In the following, we briefly explain, how the Genesis AndroidApp and the Genesis library can be used to boost your development process. 

We often got asked, how to differentiate between the two challenges. In the next section we will explain it in detail, but briefly:  

* Token creator: Its basically replacing the SQLite database in the android app with a distributed ledger technology (i.e. Ethereum) - it is a "blockchain" intense challenge

* Token obtainer: extend the Operation interface and implement actions and proofs to account for your problem, which you want to solve through incentivizing a specific behavior/ action. This challenge is more about utilizing IoT and oracles to validate if an action has actually happened. For this challenge you can pretend, that the android application is storing everythin to a DLT (i.e. Ethereum) (use it as a black box). It is a IoT/ Oracle intense challenge.

Of course, both parts can be combined and this would be the ultimate killer applciation ;)
<a name=biotschallenge_1/>
## Challenge 1

For this challenge, you can use the android application as it is and try to replace the SQLite database with a DLT (i.e. Ethereum). Some basic properties can already be defined inside the app. Hence, you can start with bringing these properties to the blockchain and afterwards think about more complex properties etc. 

For this, you need to implement the "IRepository" interface, which will be repsonsible for writing and retrieving information from the DLT (i.e. Ethereum). 

There is currently no library available, which allows developers to easily create tokens in java and to deploy them to Ethereum. Even a simple library will be a big contribution to the community :)

### Important Interfaces

* IRepository: interface to talk with the underlying database. 

## Challenge 2
In this challenge, you will impelement a own custom operation, which can be performed with a token. 
For this you need to implement the Operation interface, create your own action(s), which this operation support, and the type of proof needed to show that one performed the action.

For the start, you can use the SQLite implementation of the IRepository interface and assume, that your operations etc. are stored on the DLT (i.e. Ethereum). 
In practice, you will need to add some functions/ tables to the SQLite database. Look at the example code for hints how you can do this (in case of questions, just ask us). 

In the end, you will need to add your operation as a feature to the "Token Creator". 
Afterwards you will be able to test your token on the phone :)

### Important interfaces

* IAction: Something which can be performed (a behavior). I.e. it can represent planting a tree.
* IClaim: Represents a claim of a performed action. I.e. It could hold Marcus (resp. his publickey) as a field and the performed action (planting tree), together with a proof (i.e the signature of an oracle, which aproves that the action was performed by marcus). 
* IOperation: The class is responsible to create actions and which evaluates if a claim of an action is valid (i.e. evaluating the proof). In this case, it will write the action to the database/ DLT (i.e. Ethereum). Moreover it is repsonsible for rewarding participants, when a claim is evaluated as true. High-level one can think about it as something which addresses a specific problem, respectively, which is the solution of a specific problem. I.e. Having more green spaces in town (solution to a unlively urban environment). Hence this operation can generate different actions: plant a tree, plant flowers, do not cut trees etc. And take claims of this actions as an input.

<a name=fin4repo/>

# Fin4 Repository
In a different repository, the [ fin4 server](https://github.com/FuturICT2/fin4-core) is found. This server is the core of the finance 4.0 infrastructure. It provides amongst others necessary APIs and business logic to deploy tokens, actions and claims to blockchain systems. 
Clients can communicate with this server via the http protocol. 

As described [ before ](#biots_challenge_1), in order to connect the GenesisApp with a Blockchain system (and to deploy tokens "globally" and not just "locally") one needs to replace SQLlite database ([BasicSQLiteRepo](https://github.com/FuturICT2/GenesisApp/tree/master/app/src/main/java/com/example/mcb/genesisapp/Repository/SQLite)) with a Blockchain "database".

How this can be done, is demonstrated with the [ Fin4Repository ](https://github.com/FuturICT2/GenesisApp/tree/master/app/src/main/java/com/example/mcb/genesisapp/Repository/Fin4).

This repository extends the BasicSQLiteRepo and implements the IFin4Repo interface. Both, the Fin4Repository and the IFin4Repo are not very sophisticated and just there to showcase how an android app can communicate with the fin4 server via http.

## Usage 
Currently, the GenesisApp uses the Fin4Repository as a default. In order to change back to the BasciSQLiteRepo, one needs to comment and uncomment the specific lines in the StateActivity, as depicted: 
<p align="center">
  <img src="./img/getRepoFunction.png" width="800"/>
</p>

The Fin4Repository can either talk with the life instance of the fin4 server (Elm webapp: www.finfour.net) or a local instance of the fin4 server ([instructions](https://github.com/FuturICT2/fin4-core)). 
Using Android Studios, one can either test code in an emulator or an android phone which is connected via usb with the pc. Currently, the emulator works only with a locally deployed fin4 server and an android works only with the life instance of the server. One can specify this in the Fin4Repository via commenting/ uncommenting the following lines:
<p align="center">
  <img src="./img/serverUrl.png" width="800"/>
</p>

In order to fetch tokens from the fin4 server, which are then shown in the wallet of the Genesis app, one needs to click on the "Genesis" logo in the top right:

<p align="center">
  <img src="./img/app_before_genesis_2.png" width="350"/>
  <img src="./app_after_genesis.png" width="350"/>
</p>

# Software Architecture
