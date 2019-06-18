# address-validator-net

A library providing a wrapper for [api.address-validator.net](https://www.address-validator.net/de/api.html)

## Installation

```shell
npm install address-validator-net
```

## Usage

```js
import AddressValidator from 'address-validator-net'

// YOUR_API_KEY is a string
const { autocomplete, retrieve, validate } = AddressValidator(YOUR_API_KEY)

const responseObject1 = await autocomplete({ Query: 'Mühlenstraße 26, Ocholt' }) // searches for autocompletions

/*
  responseObject1 looks like this:
  {
    status: UNVERIFIED or error: DELAYED, RATE_LIMIT_EXCEEDED, API_KEY_INVALID_OR_DEPLETED
    results: [
      {
        description: 'Große Mühlenstraße 26, 26655 Westerstede, Niedersachsen',
        id: 'WVR7sXOZ8BbhnDYygzULO6Efzk-ax359UQ',
      },
      {
        description: 'Mühlenstraße 26, 26655 Westerstede, Niedersachsen',
        id: 'WFR7sEMtAp2323YTbCGdecjpUk-ax35tEA',
      },
    ],
  }
*/

// Now we can retrieve the resultId the user selected:
const responseObject2 = await retrieve({
  ID: 'WFR7sEMtAp2323YTbCGdecjpUk-ax35tEA',
}) // retrieves the suggested address

/*
  responseObject2 looks like this:
  {
    status: UNVERIFIED or error: DELAYED, RATE_LIMIT_EXCEEDED, API_KEY_INVALID_OR_DEPLETED
    addressline1: 'Mühlenstraße 26',
    addressline3: '26655 Westerstede',
    city: 'Westerstede',
    country: 'DE',
    formattedaddress: 'Mühlenstraße 26,26655 Westerstede',
    postalcode: '26655',
    state: 'Niedersachsen',
    street: 'Mühlenstraße',
    streetnumber: '26',
  }
*/

// Now we can validate the result we retrieved:
// !! We can also use the validation decoupled from the autocomplete with classic address forms.
const responseObject3 = await validate({
  City: 'Ocholt',
  CountryCode: 'de',
  PostalCode: '26655',
  StreetAddress: 'Mühlenstraße 26',
}) // validates the provided address

/*
  responseObject3 looks like this:
  {
    status: VALID, SUSPECT, INVALID or error: DELAYED, RATE_LIMIT_EXCEEDED, API_KEY_INVALID_OR_DEPLETED
    addressline1: 'Mühlenstr. 26',
    addressline3: '26655 Westerstede',
    city: 'Westerstede',
    country: 'DE',
    formattedaddress: 'Mühlenstr. 26,26655 Westerstede',
    postalcode: '26655',
    state: 'Niedersachsen',
    street: 'Mühlenstr.',
    streetnumber: '26',
  }

  => https://www.address-validator.net/de/api.html
*/
```

## Tests

```shell
npm test
```

## Release History

- 2.0.0 Rewrite in Typescript and using Promises
- 0.1.0 Initial release
