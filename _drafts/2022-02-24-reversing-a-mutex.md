---
layout: post
title:  "Reversing a Read-Write Mutex"
date:   2022-02-24 18:20:49 +0100
categories: programming concurrency golang
---

Writing concurrent programs is hard. A wealth of potential thread-safety issues require that 

The setup is simple: I want to make a thread-safe hashtable (in Go, a `map`). Keys correspond to a unique bank account identifier, and values correspond to that bank account's balance. A simplified example maps names to integers corresponding to dollars:

```go
type accountId      string
type accountBalance int

accounts := make(map[accountId]accountBalance)

accounts["Bob"] = 100   // Bob has $100 in his account
accounts["Alice"] = 150 // Alice has $150 in her account

// Prints 100 and 150
fmt.Printf("Bob has $%d and Alice has $%d", accounts["Bob"], accounts["Alice"]) 
```

This map is not thread-safe, i.e. there cannot be multiple concurrent gorountines reading and writing data to it. A simple way to make it thread-safe is to make the map a mutex, hence make reading and writing operations mutually exclusive. This is implemented by having each goroutine call the `Lock()` method, which prevents other threads from performing any operations on the map until the corresponding `Unlock()` method is called.

```go
type accountMutex {
    sync.Mutex
    mp map[accountId]accountBalance
}

accounts := accountMutex{
    mp: make(map[accountId]accountBalance),
}

// To make a thread-safe modification, call lock
accounts.Lock()

accounts.mp["Bob"] = 100   // Bob has $100 in his account
accounts.mp["Alice"] = 150 // Alice has $150 in her account

// Prints 100 and 150
fmt.Printf("Bob has $%d and Alice has $%d", accounts.mp["Bob"], accounts.mp["Alice"]) 

// Call unlock to allow other goroutines to act on the map
accounts.Unlock()
```

This method is fine, but in my own case this defeated the point of concurrency. Since all my server did was receive payments and change balances accordingly, a mutex map in a concurrent request handler might as well have 


Read-Write mutexes allow either multiple readers or one writer to interact with a shared data structure in a thread-safe way.