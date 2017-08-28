FAKTUROMANIA DOCS!
===================
----------


INSTRUKCJA
-------------

Pierwszą czynnością, którą należy wykonać jest rejestracja w serwisie **https://fakturomania.pl** przy pomocy emaila oraz hasła. Jest to niezbędne w celu integracji w API. Po stworzeniu konta można korzystać z endpointów:


> **CZYNNOŚCI WSTĘPNE:**

> Pierwszą czynnością, którą należy wykonać jest rejestracja w serwisie **fakturomania.pl** przy pomocy emaila oraz hasła. Jest to niezbędne w celu integracji w API. Po stworzeniu konta można korzystać z endpointów:

----------

> **INFORMACJE OGÓLNE:**

> Wszystkie adresy rozpoczynają się z prefixem **https://app.fakturomania.pl**
> **PUT** i **POST** requesty muszą mieć nagłówek  `Content-Type: application/json`
> Po zalogowaniu się do requestów dodajemy nagłowek: `Auth-Token: {value}`

#### <i class="icon-file"></i> LOGOWANIE DO SERWISU

* `POST   /api/v1/session`

Request body

    {
        "email": "jan.kowalski@email.com",
        "password": "secret",
        "remember": "false"
    }

Response body

	{
	  "value": "n7oemu62mckdv2l1c8t5fg59q6h9h9mmhjlpe2melc6aquaub7f",
	  "userEmail": "jan.kowalski@email.com",
	  "valid": 1468922607548
	}

**VALUE** - identyfikator sesji
**VALID** - unix time ważności sesji

#### <i class="icon-file"></i> DODAWANIE FAKTURY SPRZEDAŻOWEJ

Wystawianie faktury sprzedażowej w naszym imieniu.

* `POST   /api/v1/sale`

Request body

	{
	   "documentName":"", empty string albo własna numeracja, jeśli z niej korzystamy 
	   "documentNameIsCustom":false, (opcjonalnie) tylko jeśli korzystamy z własnej numeracji
	   "saleDate":1503885600000, - data sprzedaży timestamp
	   "issueDate":1503885600000, - data wystawienia timestamp
	   "contractor":{
	      "contractorId":179,
	      "contractorVersionId":187
	   },
	   "records":[
	      {
            "ordinal": 1, - numer pozycji
            "name": "Najem lokalu za miesiąc czerwiec", - nazwa usługi / towaru
            "classificationName": "PKWIU", - można zostawić puste (nie ma na fakturze)
            "classificationCode": "333", - można zostawić puste (nie ma na fakturze)
            "unit": "szt", - jednostki miary (Opcjonalnie)
            "quantity": 1.00, - ilość
            "netPrice": 800.00, - cena netto
            "netValue": 800.00, - wartość netto
            "vatRate": "23%", - stawka vat
            "vatValue": 184.00, - wartość vat
            "grossValue": 984.00 - wartość brutto
	      }
	   ],
	   "paymentInfo":{
	      "paymentDeadline":1503885600000, - termin płatności
	      "paymentMethod":"CASH", -  metoda płatnosci, "CASH" / "BANKTRANSFER" / "CARDPAYMENT" / "ONLINEPAYMENT" 
	      "paymentStatus":"PAID" - status płatności "UNPAID" / "PAID" / "PARTLYPAID",
          "amountPaid":135, - (opcjonalnie) wartość zapłacona (tylko dla PAID / PARTLYPAID)
          "paymentDate":1503885600000 - (opcjonalnie) tylko dla "PAID" / "PARTLYPAID"
	   },
	   "buyerName":"Jan Kowalski", - osoba która podpisuje fakturę jak kupujący
	   "sellerName":"Sławomir Nowak", - osoba która podpisuje fakturę jak sprzedaący
	   "comments":"towary handlowe", - informacje dodatkowe
	   "bankName":"Bank Testowy", - nazwa banku
	   "bankAccountNumber":"33 3333 3333 3333 3333 3333 3333" - numer konta
	}

Response body

    {
      "invoiceDetails": {
        "invoiceId": 9226,
        "versionUuid": "26d17499-a6d0-42ca-b297-f4d0984b0391",
        "versionId": 21765,
        "created": 1503927237230,
        "modified": 1503927237230,
        "documentType": "FVS",
        "state": "NEW"
      },
      "invoiceInfo": {
        "documentName": "FVS 481/2017",
        "documentNameIsCustom": false,
        "saleDate": 1503885600000,
        "issueDate": 1503885600000,
        "bankName": "PEKAO S.A.",
        "bankAccountNumber": "80 1240 1109 1111 0010 2504 1018"
      },
      "contractorInfo": {
        "contractorId": 2922,
        "contractorVersionId": 3177,
        "name": "Jan Kowalski Company",
        "nipPrefix": "PL",
        "nip": "999-999-00-00",
        "street": "ul. Zielona 12",
        "street2": "",
        "postalCode": "58-200",
        "postalCity": "Dzierżoniów"
      },
      "recordsInfo": {
        "amountNet": 800.00,
        "amountGross": 984.00,
        "records": [{
          "ordinal": 1,
          "name": "Najem lokalu za miesiąc czerwiec",
          "classificationName": "PKWIU",
          "classificationCode": "333",
          "unit": "",
          "quantity": 1.00,
          "netPrice": 800.00,
          "netValue": 800.00,
          "vatRate": "23%",
          "vatValue": 184.00,
          "grossValue": 984.00
        }]
      },
      "paymentInfo": {
        "paymentDeadline": 1503885600000,
        "paymentDate": 1503885600000,
        "paymentMethod": "CASH",
        "amountTotal": 984.00,
        "amountPaid": 984.00,
        "paymentStatus": "PAID"
      },
      "taxInfo": {
        "vatReckoningRule": "ACCRUAL"
      }
    }
#### <i class="icon-file"></i> POBIERANIE FAKTURY SPRZEDAŻY

* `GET   /api/v1/sale/{invoiceId}/pdf `


#### <i class="icon-file"></i> WYSYŁANIE FAKTURY SPRZEDAŻOWEJ EMAILEM

* `POST   /api/v1/sale/pdf/sendViaMail `

Request body

    {
      "invoiceId": 21,
      "emails": [
        {
          "Email": "email123@gmail.com"
    
        },
        {
          "Email": "email234@gmail.com"
        }
      ]
    }

Response body

	"Success"
	
#### <i class="icon-file"></i> WYSYŁANIE WIELU FAKTUR SPRZEDAŻOWYCH EMAILEM
    
* `POST   /api/v1/sale/pdf/sendManyViaMail `

Request body

    {
      "invoiceIds": [21,22,23],
      "emails": [
        {
          "Email": "email123@gmail.com"
    
        },
        {
          "Email": "email234@gmail.com"
        }
      ]
    }
    
Response body

    "Success"
	    

#### <i class="icon-file"></i> POBIERANIE KONTRAHENTA

* `GET   /contractors/{contractorId} `

Response body

	{
	  "contractorId": 101,
	  "contractorVersionId": 117,
	  "created": 1469008248023,
	  "modified": 1469008248023,
	  "name": "FIRMA TESTOWA",
	  "nip": "644-340-68-04",
	  "street": "ul. gen. Stefana Grota-Roweckiego 38",
	  "street2": "",
	  "postalCode": "41-214",
	  "postalCity": "Sosnowiec"
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
	
#### <i class="icon-file"></i> EDYCJA KONTRAHENTA

* `PUT   /contractors/{contractorId} `

Request body

    {
      "name": "FIRMA TESTOWA",
      "nip": "644-340-68-04",
      "street": "ul. gen. Stefana Grota-Roweckiego 38",
      "street2": "",
      "postalCode": "41-214",
      "postalCity": "Sosnowiec"
    }
	
Response body

	{
	  "contractorId": 101,
	  "contractorVersionId": 119,
	  "created": 1469008254703,
	  "modified": 1469008254703,
	  "name": "FIRMA TESTOWA",
	  "nip": "644-340-68-04",
	  "street": "ul. gen. Stefana Grota-Roweckiego 38",
	  "street2": "",
	  "postalCode": "41-214",
	  "postalCity": "Sosnowiec"
	}
	
#### <i class="icon-file"></i> SPRAWDZANIE CZY KONTRAHENT JEST W BAZIE

* `GET  /contractors/nip/{nip} `

Response body
 
    {
      "contractorId": 102,
      "contractorVersionId": 102,
      "created": 1469090000092,
      "modified": 1469090000092,
      "name": "\"ARKOM\" SPÓŁKA Z OGRANICZONĄ ODPOWIEDZIALNOŚCIĄ",
      "nip": "839-112-19-69",
      "street": "ul. Marii Zaborowskiej 29",
      "street2": "",
      "postalCode": "76-200",
      "postalCity": "Słupsk"
    }
    
LUB

    404 Not Found

**NIP** Nip w formacie xxx-xxx-xx-xx -> np. /contractors/nip/839-112-19-69

----------

W RAZIE PYTAŃ MAIL: pomoc@fakturomania.pl
TELEFON: 510-452-541 (w godzinach rozsądnych)