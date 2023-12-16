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
- The special request handler API server then sends a request to
