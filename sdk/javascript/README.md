
## The Open Oracle Reporter

The Open Oracle Reporter makes it easy to add a price feed to your application web server.

## Installation

To add the Open Oracle Reporter to your application, run:

```
yarn add open-oracle-reporter
```

## Running Stand-alone

You can run this reporter as a stand-alone by providing a simple JavaScript function that will pull the data for the reporter.

```bash
yarn global add open-oracle-reporter

open-oracle-reporter \
	--port 3000 \
	--private_key file:./private_key \
	--script ./fetchPrices.js \
	--path /prices.json \
	--key_type string \
	--value_type decimal
```

## Usage

Once you've installed the Open Oracle SDK, you can sign a Open Oracle feed as follows:

```typescript
import {encode, sign} from 'open-oracle-reporter';

let encoded = Reporter.encode('string', 'decimal', +new Date(), {'eth': 260.0, 'zrx': 0.58});
let signature = Reporter.sign(encoded, '0x...');
```

For example, in an express app:

```typescript
// See above for signing data

express.get('/prices.json', async (req, res) => {
  res.json({
	encoded: encoded,
	signature: signature
  });
});
```

You may also use the open oracle express adapter:

```typescript
import {endpoint} from 'open-oracle-reporter';

async function fetchPrices() {
	return {'eth': 260.0, 'zrx': 0.58};
}

app.use(endpoint('/prices.json', '0x...', 'prices', 'string', 'decimal', fetchPrices));
```