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
