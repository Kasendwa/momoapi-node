# MoMoApi Node Proposal

MTN MoMo API Client for Node JS.

## Development

Clone this repository and compile

```sh
git clone git@github.com:sparkplug/momoapi-node.git
cd momoapi-node
npm install
npm run compile
npm link # to test and develop cli tooling locally, see below
```

## Proposed usage

## Sandbox Credentials

To get sandbox credentials, install the package globally and run the `momo-sandbox` command

```sh
npm install --global momoapi-node
momo-sandbox --host example.com --primary-key 23e2r2er2342blahblah
```

>  For now, you can only test the cli by running `npm link` after cloning and compiling this app

### Installation

Add the library to your project

```sh
npm install momoapi-node # this isn't published yet
```

### Collections

```js
const momo = require("momoapi-node");

// initialise the collections api
const collections = new momo.Collections({
  userSecret: process.env.USER_SECRET,
  userId: process.env.USER_ID,
  subscriptionKey: process.env.SUBSCRIPTION_KEY,
  environment: process.env.ENVIRONMENT
});

// Request to pay
collections
  .requestToPay({
    amount: "50",
    currency: "EUR",
    externalId: "123456",
    payer: {
      partyIdType: "MSISDN",
      partyId: "256774290781"
    },
    payerMessage: "testing",
    payeeNote: "hello"
  })
  .then(transactionId => {
    console.log({ transactionId });

    // Get transaction status
    return collections.getTransactionStatus(transactionId);
  })
  .then(transactionStatus => {
    console.log({ transactionStatus });

    // Get account balance
    return collections.getAccountBalance();
  })
  .then(accountBalance => console.log({ accountBalance }))
  .catch(error => {
    if (error.response && error.response.data) {
      console.log(error.response.data);
    }

    console.log(error.message);
  });
```
