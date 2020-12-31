[![license](https://img.shields.io/:license-mit-blue.svg)](https://github.com/OzqurYalcin/nestpay/blob/master/LICENSE.md)
[![documentation](https://pkg.go.dev/badge/github.com/OzqurYalcin/nestpay)](https://pkg.go.dev/github.com/OzqurYalcin/nestpay/src)

# Nestpay
NestPay (EST) (Akbank, İş Bankası, Finansbank, Denizbank, Kuveytturk, Halkbank, Anadolubank, Hsbc, Ziraat Bankası) Omnipay Sanal POS API with golang

# Installation
```bash
go get github.com/OzqurYalcin/nestpay
```

# Akbank sanalpos satış işlemi
```go
package main

import (
	"encoding/xml"
	"fmt"

	nestpay "github.com/OzqurYalcin/nestpay/src"
)

func main() {
	api := &nestpay.API{"akbank"} // "akbank","asseco","isbank","finansbank","denizbank","kuveytturk","halkbank","anadolubank","hsbc","ziraatbank"
	request := nestpay.Request{}
	request.ClientId = "" // Müşteri No
	request.Username = "" // Kullanıcı adı
	request.Password = "" // Şifre
	// Ödeme
	request.Type = "Auth"
	request.Mode = "P"                           // TEST : "T" - PRODUCTION "P"
	request.IPAddress = "1.2.3.4"                // Müşteri IP adresi (zorunlu)
	request.Number = "4242424242424242"          // Kart numarası
	request.Expires = "02/20"                    // Son kullanma tarihi (Ay ve Yılın son 2 hanesi) MM/YY
	request.Cvv2Val = "123"                      // Cvv2 Kodu (kartın arka yüzündeki 3 haneli numara)
	request.Total = "1.00"                       // Satış tutarı
	request.Instalment = ""                      // Taksit sayısı
	request.Currency = nestpay.Currencies["TRY"] // Para birimi
	// Fatura
	request.BillTo.Name = ""     // Kart sahibi
	request.BillTo.Company = ""  // Fatura unvanı
	request.BillTo.TelVoice = "" // Telefon numarası
	// 3D (varsa)
	//request.PayerTxnId = ""
	//request.PayerSecurityLevel = ""
	//request.PayerAuthenticationCode = ""
	//request.CardholderPresentCode = ""
	response := api.Transaction(request)
	pretty, _ := xml.MarshalIndent(response, " ", " ")
	fmt.Println(string(pretty))
}
```

# Akbank sanalpos iade işlemi
```go
package main

import (
	"encoding/xml"
	"fmt"

	nestpay "github.com/OzqurYalcin/nestpay/src"
)

func main() {
	api := &nestpay.API{"akbank"} // "akbank","asseco","isbank","finansbank","denizbank","kuveytturk","halkbank","anadolubank","hsbc","ziraatbank"
	request := nestpay.Request{}
	request.ClientId = "" // Müşteri No
	request.Username = "" // Kullanıcı adı
	request.Password = "" // Şifre
	// İade
	request.Type = "Credit"
	request.Mode = "P"                           // TEST : "T" - PRODUCTION "P"
	request.OrderId = "ORDER-"                   // Sipariş numarası
	request.Total = "0.00"                       // İade tutarı
	request.Currency = nestpay.Currencies["TRY"] // Para birimi
	response := api.Transaction(request)
	pretty, _ := xml.MarshalIndent(response, " ", " ")
	fmt.Println(string(pretty))
}
```

# Akbank sanalpos iptal işlemi
```go
package main

import (
	"encoding/xml"
	"fmt"

	nestpay "github.com/OzqurYalcin/nestpay/src"
)

func main() {
	api := &nestpay.API{"akbank"} // "akbank","asseco","isbank","finansbank","denizbank","kuveytturk","halkbank","anadolubank","hsbc","ziraatbank"
	request := nestpay.Request{}
	request.ClientId = "" // Müşteri No
	request.Username = "" // Kullanıcı adı
	request.Password = "" // Şifre
	// İptal
	request.Type = "Void"
	request.Mode = "P"                           // TEST : "T" - PRODUCTION "P"
	request.OrderId = "ORDER-"                   // Sipariş numarası
	request.Total = "0.00"                       // İptal tutarı
	request.Currency = nestpay.Currencies["TRY"] // Para birimi
	response := api.Transaction(request)
	pretty, _ := xml.MarshalIndent(response, " ", " ")
	fmt.Println(string(pretty))
}
```