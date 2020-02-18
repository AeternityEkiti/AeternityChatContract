# How to create a Chat Contract for a simple Chat App


## Tutorial Overview
This tutorial takes a look at a smart contract, written in Sophia ML for a Chatting aepp but also provides another fundamental and deeper understanding about general basics of the language itself.

## Prequisites
 - Basic understanding of the sophia programming language

## Smart Contract
First of all lets start by creating the a record that will store the information about the users profile.
```Sophia
contract ChatContract=
 record usersRecord={
  name:string,
  discipline:string,
  status:string,
  dpUrl:string
  }

```
Ok, lets proceed by defining  some of the things that we will like to keep track of through out the lifetime of our contract, such as each `users friends`, `mesages sent to the user`, the `friend requests` a user has received and a `users profile`, we will store all this things in our state, and lets also create our init function, as you might already know, the init function returns the value of the state at the point of deployment of the contract

```Sophia
contract ChatContract=
 record state={
  usersProfile:map(address,usersRecord),
  usersFriend:map(address,list(address)),
  usersMessages:map(address,map(address,list(string))),
  friendRequests:map(address,list(address)),
  newestFriend:map(address,address)
  }
  
 stateful entrypoint init()={usersProfile={},usersFriend={},usersMessages={}, friendRequests={},newestFriend={}} 
```
So lets create a function that will allow a user register his profile.Now, this function must be stateful because it will modify the usersProfile field in our state,it must also be defined as an entrypoint because it will be called out of our contract.

```Sophia
 stateful entrypoint registerProfile(name':string,discipline':string,status':string,dpUrl':string)=
    let newProfile={name=name',discipline=discipline',status=status',dpUrl=dpUrl'}
    put(state{usersProfile[Call.caller]=newProfile})
```
We will also create the function that will return a particular users profile, we will use the Map.lookup_default function which allows you to specify a default value for a map to return if it does not have a particular key

```Sophia
 entrypoint getProfile()=
    Map.lookup_default(Call.caller,state.usersProfile,{name="",discipline="",status="",dpUrl=""})  

```
Next, lets create a function that will allow a user to send a friend request to another user using his address, this function will take in the other users address, and store it in the field of the present users friend Requests  Map which is in the state.

```Sophia
 stateful entrypoint sendFriendRequest(friendsAddress':address)=
     let emptyAddressList:list(address)=[Call.caller]
     switch(Map.lookup(friendsAddress',state.friendRequests))
       None =>   onlyOneFriendRequest(friendsAddress',emptyAddressList)
       Some(x)=> modifyFriendsRequest(x,emptyAddressList, friendsAddress')
       
 stateful function modifyFriendsRequest(oldList:list(address),newFriendList:list(address),friendsAddress':address):list(address)=
    let newList=newFriendList++oldList
    put(state{friendRequests[friendsAddress']=newList})
    newList
    
 stateful function onlyOneFriendRequest(friendsAddress':address,newFriendList:list(address))=
     put(state{friendRequests[friendsAddress']=newFriendList})
     newFriendList
     

```


