FAKTUROMANIA DOCS!
===================
----------


INSTRUKCJA
-------------

Pierwszą czynnością, którą należy wykonać jest rejestracja w serwisie **fakturomania.pl** przy pomocy emaila oraz hasła. Jest to niezbędne w celu integracji w API. Po stworzeniu konta można korzystać z endpointów:


> **CZYNNOŚCI WSTĘPNE:**

> Pierwszą czynnością, którą należy wykonać jest rejestracja w serwisie **fakturomania.pl** przy pomocy emaila oraz hasła. Jest to niezbędne w celu integracji w API. Po stworzeniu konta można korzystać z endpointów:

----------

> **INFORMACJE OGÓLNE:**

> Wszystkie adresy rozpoczynają się z prefixem **https://app.fakturomania.pl**
> **PUT** i **POST** requesty muszą mieć nagłówek  `Content-Type: application/json`

#### <i class="icon-file"></i> LOGOWANIE DO SERWISU

* `POST   /api/v1/session`

Request body

    {
        "email": "john@jogn.com",
        "password": "secret",
        "remember": "false"
    }

Response body

	{
	  "value": "n7oemu62mckdv2l1c8t5fg59q6h9h9mmhjlpe2melc6aquaub7f",
	  "userEmail": "john@jogn.com",
	  "valid": 1468922607548
	}

**VALUE** - identyfikator sesji
**VALID** - unix time ważności sesji

#### <i class="icon-file"></i> DODAWANIE FAKTURY SPRZEDAŻOWEJ

Wystawianie faktury sprzedażowej w naszym imieniu.

* `POST   /api/v1/sale`

Request body

	{
	   "documentType":"FVS", - bez zmian (zostawiamy jak na przykładzie)
	   "documentName":"", - bez zmian (zostawiamy jak na przykładzie)
	   "saleDate":1468965599000, - data sprzedaży timestamp
	   "issueDate":1468965599000, - data wystawienia timestamp
	   "contractor":{
	      "contractorId":179,
	      "contractorVersionId":187
	   },
	   "records":[
	      {
	         "ordinal":1, - numer pozycji
	         "name":"towar", - nazwa usługi / towaru
	         "classificationName":"PKWIU", - można zostawić (nie ma na fakturze)
	         "classificationCode":"333", - można zostawić (nie ma na fakturze)
	         "unit":"", - jednostki miary
	         "quantity":1, ilość
	         "netPrice":99.19, - cena netto
	         "netValue":99.19, - wartość netto
	         "vatRate":"23%", 
	         "vatValue":22.81, - wartość vat
	         "grossValue":122 - wartość brutto
	      }
	   ],
	   "paymentInfo":{
	      "paymentDeadline":1468965599000, - termin płatności
	      "paymentMethod":"CASH", -  metoda płatnosci, "CASH" albo "BANKTRANSFER" 
	      "paymentStatus":"UNPAID" - status płatności "UNPAID" albo "PAID"
	   },
	   "buyerName":"", - osoba która podpisuje fakturę jak kupujący
	   "sellerName":"", - osoba która podpisuje fakturę jak sprzedaący
	   "comments":"", - informacje dodatkowe
	   "bankName":"lll", - nazwa banku
	   "bankAccountNumber":"33 3333 3333 3333 3333 3333 3333" - numer konta
	}

Response body

	{
	  "invoiceId": 349,
	  "versionUuid": "b4ba4220-64aa-45f7-abb0-d2ac1e0e007c",
	  "versionId": 622,
	  "created": 1468922814227,
	  "modified": 1468922814227,
	  "documentName": "FVS 1/7/2016",
	  "documentType": "FVS",
	  "saleOrPurchase": "S",
	  "saleDate": 1468893600000,
	  "issueDate": 1468893600000,
	  "contractor": {
	    "contractorId": 179,
	    "contractorVersionId": 187,
	    "created": 1468922814272,
	    "modified": 1468922814272,
	    "name": "PNIEWSKA MARIOLA",
	    "nip": "529-102-50-41",
	    "street": "rynek Tadeusza Kościuszki 3",
	    "street2": "",
	    "postalCode": "99-417",
	    "postalCity": "Bolimów"
	  },
	  "records": [{
	    "id": 541,
	    "invoiceVersionId": 622,
	    "ordinal": 1,
	    "name": "towar",
	    "classificationName": "PKWIU",
	    "classificationCode": "333",
	    "unit": "",
	    "quantity": 1,
	    "netPrice": 99.19,
	    "netValue": 99.19,
	    "vatRate": "23%",
	    "vatValue": 22.81,
	    "grossValue": 122.00
	  }],
	  "state": "NEW",
	  "organizationId": 102,
	  "buyerName": "",
	  "sellerName": "",
	  "comments": "",
	  "paymentInfo": {
	    "paymentDeadline": 1468893600000,
	    "paymentMethod": "CASH",
	    "amountTotal": 122.00,
	    "amountPaid": 0.00,
	    "paymentStatus": "UNPAID"
	  },
	  "amountNet": 99.19,
	  "amountGross": 122.00,
	  "additionalInvoiceInfo": {
	    "bankAccount": {
	      "bankName": "lll",
	      "bankAccountNumber": "33 3333 3333 3333 3333 3333 3333"
	    }
	  }
	}
#### <i class="icon-file"></i> POBIERANIE FAKTURY SPRZEDAŻY

* `GET   /api/v1/sale/{invoiceId}/pdf `


#### <i class="icon-file"></i> WYSYŁANIE FAKTURY EMAILEM

* `POST   /api/v1/sale/{invoiceId}/pdf/sendViaMail `

Request body

    {
        "email": "john@jogn.com" - email na który wysyłamy
    }

Response body

	"Pomyślnie wysłano fakturę"

#### <i class="icon-file"></i> DODAWANIE FAKTURY KOSZTOWEJ

* `POST   /api/v1/sale`

REQUEST BODY

		{
		  "documentType": "FVZ",
		  "documentName": "FSK 12\/12\/2016",
		  "saleDate": 1468944000000,
		  "issueDate": 1468944000000,
		  "contractor": {
		    "contractorId": 179,
		    "contractorVersionId": 187
		  },
		  "categoryId": {
		    "tier1": 8, - kategoria kosztu
		    "tier2": 1 - podkategoria kosztu
		  },
		  "records": [
		    {
		      "ordinal": 1,
		      "name": "USŁUGA XXX",
		      "classificationName": "PKWIU",
		      "classificationCode": "333",
		      "unit": "nd.",
		      "quantity": 1,
		      "netPrice": 123,
		      "netValue": 123,
		      "vatRate": "23%",
		      "vatValue": 28.29,
		      "grossValue": 151.29
		    }
		  ],
		  "paymentInfo": {
		    "paymentDeadline": 1468944000000,
		    "paymentMethod": "CASH",
		    "paymentStatus": "UNPAID"
		  },
		  "comments": "",
		  "sellerName": "",
		  "buyerName": "",
		  "bankName": "lll",
		  "bankAccountNumber": "33 3333 3333 3333 3333 3333 3333"
		}

RESPONSE BODY

	{
	  "invoiceId": 350,
	  "versionUuid": "414e6b35-34de-4401-a786-ae31a4baa2dd",
	  "versionId": 623,
	  "created": 1468924371841,
	  "modified": 1468924371841,
	  "documentName": "FSK 12/12/2016",
	  "documentType": "FVZ",
	  "saleOrPurchase": "P",
	  "saleDate": 1468893600000,
	  "issueDate": 1468893600000,
	  "contractor": {
	    "contractorId": 179,
	    "contractorVersionId": 187,
	    "created": 1468924371887,
	    "modified": 1468924371887,
	    "name": "PNIEWSKA MARIOLA",
	    "nip": "529-102-50-41",
	    "street": "rynek Tadeusza Kościuszki 3",
	    "street2": "",
	    "postalCode": "99-417",
	    "postalCity": "Bolimów"
	  },
	  "records": [{
	    "id": 542,
	    "invoiceVersionId": 623,
	    "ordinal": 1,
	    "name": "USŁUGA XXX",
	    "classificationName": "PKWIU",
	    "classificationCode": "333",
	    "unit": "nd.",
	    "quantity": 1,
	    "netPrice": 123.00,
	    "netValue": 123.00,
	    "vatRate": "23%",
	    "vatValue": 28.29,
	    "grossValue": 151.29
	  }],
	  "state": "NEW",
	  "organizationId": 102,
	  "categoryId": {
	    "tier1": 8,
	    "tier2": 1
	  },
	  "buyerName": "",
	  "sellerName": "",
	  "comments": "",
	  "paymentInfo": {
	    "paymentDeadline": 1468893600000,
	    "paymentMethod": "CASH",
	    "amountTotal": 151.29,
	    "amountPaid": 0.00,
	    "paymentStatus": "UNPAID"
	  },
	  "amountNet": 123.00,
	  "amountGross": 151.29,
	  "additionalInvoiceInfo": {
	    "bankAccount": {
	      "bankName": "lll",
	      "bankAccountNumber": "33 3333 3333 3333 3333 3333 3333"
	    }
	  }
	}


#### <i class="icon-file"></i> ZAPISYWANIE KONTRAHENTA

* `POST   /api/v1/contractors `

Request body

	{
	  "name": "FIRMA TESTOWA",
	  "nip": "123-456-789",
	  "street": "WIELKA 44",
	  "street2": "",
	  "postalCode": "05-825",
	  "postalCity": "GRODZISK MAZOWIECKI"
	}

Response body

	{
	  "contractorId": 396,
	  "contractorVersionId": 428,
	  "created": 1468924664066,
	  "modified": 1468924664066,
	  "name": "FIRMA TESTOWA",
	  "nip": "123-456-789",
	  "street": "WIELKA 44",
	  "street2": "",
	  "postalCode": "05-825",
	  "postalCity": "GRODZISK MAZOWIECKI"
	}
	

----------