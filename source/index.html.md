---
title: API Reference

language_tabs:
  - json
  - php

toc_footers:
  - <a href='http://www.brasilbookings.com.br'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: false
---

# Introdução

Bem-vindo(a) a API do Brasil Bookings!


# Autenticação

> Para autenticação utilize o seguinte código:

```php
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

> Você deve substituir `MINHAAPIKEY` por sua API Key.

A API do Brasil Bookings utiliza API Keys para autorizar o acesso à API.
A API Key deve ser informada em todos os requests ao servidor, no header da requisição, conforme o exemplo a seguir:

`X-API-KEY: MINHAAPIKEY`

<aside class="notice">
Você deve substituir <code>MINHAAPIKEY</code> com a sua API key exclusiva.
</aside>

# Tipos de Quarto

Através da rota `/rooms_api/rooms` e suas derivadas, você pode retornar os dados básicos dos tipos de quartos existentes.

## Objeto `rooms_api/rooms`

> Objeto rooms_api/rooms

```json
{
  "RoomsRS": {
    "0": "Rooms",
    "Rooms": {
      "Success": "",
      "HotelCode": "1",
      "0": {
        "ID": "1",
        "Title": "Quarto 1",
        "Adults": "3",
        "Children": "2"
      },
      "1": {
        "ID": "2",
        "Title": "Quarto 2",
        "Adults": "2",
        "Children": "0"
      },
      "2": {
        "ID": "3",
        "Title": "Quarto 3",
        "Adults": "4",
        "Children": "0"
      },
      "3": {
        "ID": "4",
        "Title": "Quarto 4",
        "Adults": "5",
        "Children": "0"
      }
    }
  }
}
```

Ao consultar este método, este será o objeto que você irá receber como resposta.

Propriedade | Descrição
-----------: | ---------
**HotelCode** <br>string | Código do hotel no Brasil Bookings
**ID** <br>integer | ID do Quarto no Brasil Bookings
**Title** <br>string | Título do quarto
**Adults** <br>integer | Quantidade máxima de hóspedes adultos do quarto
**Children** <br>integer | Quantidade máxima de hóspedes crianças do quarto

# Tipos de Tarifa

Através da rota `/rate_plans_api/rate_plans` e suas derivadas, você pode retornar os dados básicos dos tipos de tarifas existentes.

## Objeto `rate_plans_api/rate_plans`

> Objeto rate_plans_api/rate_plans

```json
{
  "RatePlansRS": {
    "0": "Rates",
    "Rates": {
      "Success": "",
      "HotelCode": "1",
      "0": {
        "ID": "150",
        "Name": "Tarifa Padrão"
      },
      "1": {
        "ID": "151",
        "Name": "Tarifa Corporativa"
      }
    }
  }
}
```

Ao consultar este método, este será o objeto que você irá receber como resposta.

Propriedade | Descrição
-----------: | ---------
**HotelCode** <br>string | Código do hotel no Brasil Bookings
**ID** <br>integer | ID do Quarto no Brasil Bookings
**Name** <br>string | Nome do tipo de tarifa

# Atulizar Disponibilidade

Através da rota `/rate_plans_api/update_availability`, você pode atualizar a disponibilidade dos quartos por período.

## Atualizando disponibilidades

> POST

```json
{
  "AvailabilityRQ": {
    "AvailStatusMessages": {
      "HotelCode": "1",
      "AvailStatusMessage": [
        {
          "BookingLimit": "14",
          "StatusApplicationControl": {
            "Start": "2016-11-20",
            "End": "2016-11-21",
            "InvCode": "1"
          }
        },
        {
          "BookingLimit": "14",
          "StatusApplicationControl": {
            "Start": "2016-11-21",
            "End": "2016-11-22",
            "InvCode": "1"
          }
        },
        {
          "BookingLimit": "14",
          "StatusApplicationControl": {
            "Start": "2016-11-22",
            "End": "2016-11-23",
            "InvCode": "2"
          }
        }
      ]
    }
  }
}
```

> RESPONSE

```json
{
  "AvailabilityRS": {
    "Success": ""
  }
}
```

Propriedade | Descrição
-----------: | ---------
**HotelCode** <br>string | Código do hotel no Brasil Bookings
**BookingLimit** <br>integer | Quantidade de unidades disponíveis para o quarto
**Start** <br>string | Data de início da disponibilidade
**End** <br>string | Data final da disponibilidade
**InvCode** <br>string | ID do tipo de quarto no Brasil Bookings

# Atualizar Tarifas

Através da rota `/rates_api/update_rates`, você pode atualizar as tarifas dos quartos por período e número de ocupantes.

## Atualizando tarifas

> POST

```json
{
  "RatesRQ": {
    "RatePlans": {
      "HotelCode": "1",
      "RatePlan": {
        "RatePlanCode": "1",
        "Rates": {
          "Rate": [
            {
              "Start": "2016-11-28",
              "End": "2016-11-29",
              "InvCode": "1",
              "BaseByGuestAmts": {
                "BaseByGuestAmt": [
                  {
                    "Amount": "150.00",
                    "NumberOfGuests": "1"
                  },
                  {
                    "Amount": "20.00",
                    "NumberOfGuests": "2"
                  },
                  {
                    "Amount": "30.00",
                    "NumberOfGuests": "3"
                  }
                ]
              }
            }
          ]
        }
      }
    }
  }
}
```

> RESPONSE

```json
{
  "RatesRS": {
    "Success": ""
  }
}
```

Propriedade | Descrição
-----------: | ---------
**HotelCode** <br>string | Código do hotel no Brasil Bookings
**RatePlanCode** <br>integer | ID do tipo de tarifa
**Start** <br>string | Data de início da disponibilidade
**End** <br>string | Data final da disponibilidade
**InvCode** <br>string | ID do tipo de quarto no Brasil Bookings
**Amount** <br>float | Valor da tarifa
**NumberOfGuests** <br>integer | Número de ocupantes da tarifa

# Atualizar Restrições e Estadia Mínima

Através da rota `/restrictions_api/update_restrictions`, você pode atualizar as restrições e estadias mínimas dos quartos por período e tipo de tarifa.

> POST

```json
{
  "RestrictionsRQ": {
    "AvailStatusMessages": {
      "HotelCode": "1",
      "AvailStatusMessage": [
        {
          "StatusApplicationControl": {
            "Start": "2016-11-02",
            "End": "2016-11-03",
            "RatePlanCode": "1",
            "InvCode": "1"
          },
          "LengthsOfStay": {
            "LengthOfStay": {
              "Time": "3",
              "TimeUnit": "Day",
              "MinMaxMessageType": "MinLOS"
            }
          }
        },
        {
          "StatusApplicationControl": {
            "Start": "2016-11-02",
            "End": "2016-11-03",
            "RatePlanCode": "1",
            "InvCode": "1"
          },
          "RestrictionStatus": [{
              "Restriction": "Arrival",
              "Status": "Open"
            },
            {
              "Restriction": "Departure",
              "Status": "Open"
            }
		   ]
        }
      ]
    }
  }
}
```

> RESPONSE

```json
{
  "RestrictionsRS": {
    "Success": ""
  }
}
```

# Bloqueio/Desbloqueio de Tarifas

Através da rota `/block_rates_api/block_rates`, você pode bloquear ou desbloquear tarifas por período.

> POST

```json
{
  "BlockRateRQ": {
    "AvailStatusMessages": {
      "HotelCode": "1",
      "AvailStatusMessage": [
        {
          "StatusApplicationControl": {
            "Start": "2016-11-20",
            "End": "2016-11-21",
            "RatePlanCode": "1",
            "InvCode": "1"
          },
          "RestrictionStatus": { "Status": "Close" }
        },
        {
          "StatusApplicationControl": {
            "Start": "2016-11-21",
            "End": "2016-11-22",
            "RatePlanCode": "1",
            "InvCode": "1"
          },
          "RestrictionStatus": { "Status": "Open" }
        }
      ]
    }
  }
}
```

> RESPONSE

```json
{
  "BlockRateRS": {
    "Success": ""
  }
}
```

<!--
# Reservas

Através da rota `/reservations_api/reservations`, retornar as reservas feitas no Brasil Bookings.

> POST

```json

```

> RESPONSE

```json

```

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

-->

