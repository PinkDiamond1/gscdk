Go Smart Contracts Development Kit

# Quick Start

<h3>
  <a
    target="_blank"
    href="https://mybinder.org/v2/gh/uuosio/uuosio.gscdk/main?filepath=quickstart/quickstart.ipynb"
  >
    Quick Start
    <img alt="Binder" valign="bottom" height="25px"
    src="https://mybinder.org/badge_logo.svg"
    />
  </a>
</h3>

# What the Go Smart Contracts look like?

Here is an example

```go
package main

import (
    "github.com/uuosio/chain"
    "github.com/uuosio/chain/logger"
)

//table mytable
type MyData struct {
    primary uint64 //primary: t.primary
    name    string
}

//contract mycontract
type MyContract struct {
    Receiver      chain.Name
    FirstReceiver chain.Name
    Action        chain.Name
}

func NewContract(receiver, firstReceiver, action chain.Name) *MyContract {
    return &MyContract{receiver, firstReceiver, action}
}

//action sayhello
func (c *MyContract) SayHello(name string) {
    code := c.Receiver
    scope := code
    payer := c.Receiver
    mydb := NewMyDataDB(code, scope)
    primary := uint64(111)
    if it, data := mydb.Get(primary); it.IsOk() {
        if data.name != name {
            logger.Println("Welcome new friend:", name)
        } else {
            logger.Println("Welcome old friend", name)
        }
        data.name = name
        mydb.Update(it, data, payer)
    } else {
        logger.Println("Welcome new friend", name)
        data := &MyData{primary, name}
        mydb.Store(data, payer)
    }
}
```


# Installation
```bash
pip install gscdk
```

# Build Go Smart Contracts Compiler

Follow the steps in [Building](./BUILDING.md)

That will build tinygo command in compiler/build directory that support for building Go Smart Contracts.

#### Set PATH

```bash
export PATH=$(pwd)/compiler/build:$PATH
```

# eosio-go

## Initialize a project with "init" subcommand

"init" command initialize a project with contract name

```
eosio-go init mycontract
cd mycontract
```

## Generating ABI and Extra Code for Smart Contracts

```
eosio-go gencode
```

Code generation is also the default option for "build" command

## Build Go Smart Contracts Project

#### Compiling the Source Code

```bash
eosio-go build -o mycontract.wasm .
```

#### Disable Code Generation during Building

```bash
eosio-go build -gen-code=false -o mycontract .
```

#### Disable Striping WASM File after Building

```bash
eosio-go build -strip=false -o mycontract .
```
