## APP END
application end is provider-agnostic. Anyone can register under anyone and use their service
which means govind@mulearn  can register under mulearn but use the services of ktu as well

while login username and authentication must be verified by host network of user, the app will receive a session token with reasonable timeout.

![Imgur](https://i.imgur.com/QCpNB2T.png)

app ends will be initiating api calls to the network 
which will be all the prescribed edpoints disecribed in layer zero, along with the adisanal edpoints for auth, level, leagsy account handler

 our implimention plan :- provide high level schema and genaric implemention for others to adapte 

## Network 

### governences : - layer 

we want MOP to be transpernt and demcortic 

![Imgur](https://i.imgur.com/YqxxNKV.png)


everything is transpernt in grovencens, any action to be performed must submited as a proposal, any one can submit a proposal and any can view the propasals but the propsal can be only voted by  board members (active providers)

to make a proposal you need to registered in the giovernance website with your own seperate account ( this is not linked with network in other ways ) and for any proposal related comms this account and its mail will be used

- all proposals will be passed on a 51% vote only
- to be a provider a propsal must be made, proposal should include info such as contact info , provider name , etc ,etc ( we will provide a template )
-  any user can comment on the thread 
- any user can make any proposal for any request to change things on the network
- providers can also be kicked out with a similar proposal 
- votes can be made by only active providers , after 30 days of inactivity the provider will be removed from voting rights and need to send a new proposal to be board member again
- when a provider is gaining voting rights , they will be given an auth token and decryption key for encryption in addition to the usual acc password . YOU MUST NOT LOSE IT , this cannot be reissued and you will have to wait for time out and make a new proposal again . Without the token you cannot vote
- a proposal has a life time of 10 days ( can be changed via proposal ) , if it is not passed by that duration it is auto rejected , submit proposal again
governance layer exists completely seperate to the network , the only common link is execution queue 
- if a account that makes a proposal 3 times in a row is rejected , account is banned 
- new accounts must have a min timeout of 20 days before submitting proposals but they can comment
The developers will only execute tasks loaded in the exectuion queue and nothing else once the network is up

The developers will be seperate entitity supported by every provider , they can put their own member in ( again via proposal )

If you want to cancel an execution or appeal , that too should go via proposal mechanism , this ensures a very scalable democractic fast process

Providers should bear the cost of the developers too

#### Implementation of governance : 
this will be first issued as a specification at initial phase since mulearn is the sole board member 
for implementation we need a front end and simple backend with an exection quese , its quite straightorward 

### Network Core
![Imgur](https://i.imgur.com/jPcKtQH.png)

- all requests are sent to network-core-handler which then refers the registry table to match the alias to the actual api url for the provider
- the addresses to send the request is loaded in the gateway along with the payload ( message ) , the context will cary any additional message such as domain which will be handled by the provider tiself
- if a resource is being written into a new provider db ( guest or foreign ) there will be a check to see if that user exist , if not the response header will have a error message .
- The special request handler API server then sends a resonse to the client side requesting if the user has a legacy account on this provider they are interacting with for the first time in the network . If they are new to the provider then a check is done if its a valid user in the users host provider and then a entry is added to the providers user mapping db . If they already have a legacy account on the provider the provider must create a new mapping to the main id with the legacy id ( such as goving@mulearn having an already exisint ktu id such as 1011@ktu , then in the provider ktu for the user mapping there must be a user mapping such as   govind@mulearn -> 1011@ktu , searches willbe done on govind@mulearn and that search must forward to the internal mapping 1011@ktu ) . A user is supposed to only have one universal id , everything else is mapped one way ( from universtal id to legacy id , no reverse ).Once this is done everything can function in usual manner , removing any additional complexities
- Special request hanlder also handles the function of level querying , when a user makes a level enquiry , the network searches the home provider for that days cache of level . if it exists that is returned directly , but if not then the response header will contain error msg for cache not found , which will trigger the speical request handler . This will send a request across all providers to give their stored karma points and then the received values are aggreagated to find the level of the user , this is then sent to the home network to write in their cache for the user's level and returned to the user as well

Implementation details:
specifying all the response and request header formats and how to handle them in the specs
Implement the network in a clusterized scalable manner

## Provider End

![Imgur](https://imgur.com/F2HmGDD.png)

- The provider is to implement all the endpoints that were previously mentioned and give out the correct response in the context headers , it should also handle all the info passed in the header for request as discussed above too
- The database of the provider handles information as described in the pic . The referral list is actually part of the planned mucoin expansion


## Example flow for requests :
