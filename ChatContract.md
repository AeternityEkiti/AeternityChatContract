# How to create a Chat Contract for a simple Chat App


## Tutorial Overview
This tutorial takes a look at a smart contract, written in Sophia ML for a Chatting aepp but also provides another fundamental and deeper understanding about general basics of the language itself.

## Prequisites
 - Basic understanding of the sophia programming language

## Smart Contract
First of all lets start by creating the a record that will store the information about the users profile.
```Sophia
contract ChatContract=
 record usersProfile={
  name:string,
  discipline:string,
  status:string,
  dpUrl:string
  }

```
Ok, lets proceed by defining  some of the things that we will like to keep track of through out the lifetime of our contract, such as each `users friends`, `mesages sent to the user`, the `friend requests` a user has received and a `users profile`, we will store all this things in our state

```Sophia
contract ChatContract=

 record state={
  usersProfile:map(address,usersProfile),
  usersFriend:map(address,list(address)),
  usersMessages:map(address,map(address,list(string))),
  friendRequests:map(address,list(address)),
  newestFriend:map(address,address)
  }
  
```
