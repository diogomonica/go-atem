# Atem

BlackMagicDesign ATEM switcher protocol implementation. Written in go.

## Usage
```sh
go get -u github.com/bdogan/go-atem
```

## Initialize Atem Client
Atem client requires ip address to connect device

```go
package main

import (
	"go-atem"
	"log"
)

type app struct {
	atemClient atem.Atem
}

func (at *app) onAtemConnected()  {
	log.Printf("Atem connected on %s. UID:%d\n", at.atemClient.Connection.LocalAddr().String(), at.atemClient.UID)
	log.Printf("Product Id: %s, Protocol Version: %s\n", at.atemClient.ProductId.String(), at.atemClient.ProtocolVersion.String())
}

func (at *app) onAtemClosed()  {
	log.Println("Connection closed")
}

func main() {

	// Create Application
	app := app{
		atemClient: atem.Create("192.168.0.2", true),
	}

	// Set connected handler
	app.atemClient.On("connected", app.onAtemConnected)

	// Set closed handler
	app.atemClient.On("closed", app.onAtemClosed)

	// Make connection
	app.atemClient.Connect()

}
```

## Reference

* [BlackMagic ATEM Switcher Protocol @kasperskaarhoj](https://www.skaarhoj.com/fileadmin/BMDPROTOCOL.html)