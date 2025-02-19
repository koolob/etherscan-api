**English** | [中文](https://github.com/nanmu42/etherscan-api/blob/master/README_ZH.md)

# etherscan-api

[![GoDoc](https://godoc.org/github.com/nanmu42/etherscan-api?status.svg)](https://godoc.org/github.com/nanmu42/etherscan-api)
[![CI status](https://github.com/nanmu42/etherscan-api/actions/workflows/ci.yaml/badge.svg)](https://github.com/nanmu42/etherscan-api/actions)
[![codecov](https://codecov.io/gh/nanmu42/etherscan-api/branch/master/graph/badge.svg)](https://codecov.io/gh/nanmu42/etherscan-api)
[![Go Report Card](https://goreportcard.com/badge/github.com/nanmu42/etherscan-api)](https://goreportcard.com/report/github.com/nanmu42/etherscan-api)

Golang client for the Etherscan.io API(and its families like BscScan), with nearly full implementation(accounts, transactions, tokens, contracts, blocks, stats), full network support(Mainnet, Ropsten, Kovan, Rinkby, Goerli, Tobalaba), and only depending on standard library. :wink:

# Usage

```bash
go get github.com/nanmu42/etherscan-api
```

Create an API instance and off you go. :rocket:

```go
import (
	"github.com/koolob/etherscan-api"
	"fmt"
)

func main() {
	// create a API client for specified ethereum net
	// there are many pre-defined network in package
	client := etherscan.New(etherscan.Mainnet, "[your API key]")
	
	// or, if you are working with etherscan-family API like BscScan
	//
	// client := etherscan.NewCustomized(etherscan.Customization{
	// Timeout:       15 * time.Second,
	// Key:           "You key here",
	// BaseURL:       "https://api.bscscan.com/api?",
	// Verbose:       false,
	// })

	// (optional) add hooks, e.g. for rate limit
	client.BeforeRequest = func(module, action string, param map[string]interface{}) error {
		// ...
	}
	client.AfterRequest = func(module, action string, param map[string]interface{}, outcome interface{}, requestErr error) {
		// ...
	}

	// check account balance
	balance, err := client.AccountBalance("0x281055afc982d96fab65b3a49cac8b878184cb16")
	if err != nil {
		panic(err)
	}
	// balance in wei, in *big.Int type
	fmt.Println(balance.Int())

	// check token balance
	tokenBalance, err := client.TokenBalance("contractAddress", "holderAddress")

	// check ERC20 transactions from/to a specified address
	transfers, err := client.ERC20Transfers("contractAddress", "address", startBlock, endBlock, page, offset)
}
```

You may find full method list at [GoDoc](https://godoc.org/github.com/nanmu42/etherscan-api).

# Etherscan API Key

You may apply for an API key on [etherscan](https://etherscan.io/apis).

> The Etherscan Ethereum Developer APIs are provided as a community service and without warranty, so please just use what you need and no more. They support both GET/POST requests and a rate limit of 5 requests/sec (exceed and you will be blocked). 

# Paperwork Things

I am not from Etherscan and I just find their service really useful, so I implement this. :smile:

# License

Use of this work is governed by an MIT License.

You may find a license copy in project root.
