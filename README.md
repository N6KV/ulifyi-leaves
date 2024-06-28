<div align="center">

# Ulifyi Leaves

[![npm version](https://img.shields.io/npm/v/ulifyi-sdk.svg)](https://www.npmjs.com/package/ulifyi-sdk)
[![npm downloads](https://img.shields.io/npm/dm/ulifyi-sdk.svg)](https://www.npmjs.com/package/ulifyi-sdk)
[![GitHub license](https://img.shields.io/github/license/Ulifyi/ulifyi-sdk)](https://github.com/Ulifyi/ulifyi-sdk/blob/main/LICENSE)

Ulifyi SDK is a JavaScript library that provides a convenient interface for interacting with the Ulifyi API. It allows you to easily integrate Ulifyi authentication and user data retrieval into your applications.

## Features

- Authenticate users with Ulifyi using OAuth2.
- Retrieve user data, including self-information and information about other users.

## Installation

```bash
npm install ulifyi-sdk
````
</div>

## Usage

### Initialize the SDK

```javascript
const UlifyiSdk = require('ulifyi-sdk');

const ulifyi = new UlifyiSdk();
ulifyi.init('YOUR_ACCESS_TOKEN');
```

## Examples

### Get Self Information

```javascript
try {
    const selfUserData = await ulifyi.getUserSelf();
    console.log('Self User Data:', selfUserData);
} catch (error) {
    console.error('Error fetching self user data:', error.message);
}
```

### Get User Information by ID

```javascript
const userId = 'TARGET_USER_ID';

try {
    const targetUserData = await ulifyi.getUserById(userId);
    console.log(`User Data for User ID ${userId}:`, targetUserData);
} catch (error) {
    console.error(`Error fetching user data for User ID ${userId}:`, error.message);
}
```

### Generate OAuth2 URL

```javascript
const clientId = 'YOUR_CLIENT_ID';
const redirectUri = 'YOUR_REDIRECT_URI';
const scopes = 'openid profile email'; // Example scopes
const responseType = 'code';
const state = 'YOUR_STATE';

try {
    const authorizationUrl = ulifyi.generateUrl(clientId, redirectUri, scopes, responseType, state);
    console.log('OAuth2 Authorization URL:', authorizationUrl);
} catch (error) {
    console.error('Error generating OAuth2 authorization URL:', error.message);
}
```

## API Reference
- `init(accessToken: string)`: Initializes the SDK with the provided access token.
- `getUserSelf(): Promise<UserData>`: Retrieves the user's self information.
- `getUserById(userId: string): Promise<UserData>`: Retrieves user information by user ID.
- `generateUrl(clientId: string, redirectUri: string, scopes: string, responseType: string, state: string): string`: Generates an OAuth2 authorization URL.

## Contributing
Contributions are welcome! Please feel free to submit a pull request.

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/Ulifyi/ulifyi-sdk/tree/main?tab=MIT-1-ov-file#readme) file for details.

--

<div align="center">

# Ulifyi Leaves

[![npm version](https://img.shields.io/npm/v/ulifyi-sdk.svg)](https://www.npmjs.com/package/ulifyi-sdk)
[![npm downloads](https://img.shields.io/npm/dm/ulifyi-sdk.svg)](https://www.npmjs.com/package/ulifyi-sdk)
[![GitHub license](https://img.shields.io/github/license/Ulifyi/ulifyi-sdk)](https://github.com/Ulifyi/ulifyi-sdk/blob/main/LICENSE)

Ulifyi Leaves is a JavaScript library for generating unique Snowflake IDs similar to Twitter's Snowflake IDs. It allows you to easily integrate globally unique identifier generation into your applications.

## Features

- Generate unique Leave IDs.
- Customizable machine ID.
- Thread-safe ID generation.

## Installation

```bash
npm install ulifyi-leaves
```
</div>

## Usage

### Initialize the Leaves
```javascript
const Leaves = require('ulifyi-leaves');

const machineId = 1; // Your unique machine ID
const leaves = new Leaves(machineId);
```

## Examples

### Generate Leave ID

```javascript
const Leaves = require('./leaves');

const machineId = 1;
const leaves = new Leaves(machineId);

for (let i = 0; i < 10; i++) {
  console.log(leaves.generateId());
}
```

## API Reference
- `constructor(machineId: number)`: Initializes the Leaves instance with the provided machine ID.
- `generateId()`: string: Generates a unique Leave ID.

## Contributing
Contributions are welcome! Please feel free to submit a pull request.

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/Ulifyi/ulifyi-leaves/tree/main?tab=MIT-1-ov-file#readme) file for details.

## leaves.js

```javascript
class Leaves {
  constructor(machineId) {
    this.EPOCH = 1288834974657n;
    this.MACHINE_ID_BITS = 10n;
    this.SEQUENCE_BITS = 12n;

    this.MAX_MACHINE_ID = -1n ^ (-1n << this.MACHINE_ID_BITS);
    this.MAX_SEQUENCE = -1n ^ (-1n << this.SEQUENCE_BITS);

    this.MACHINE_ID_SHIFT = this.SEQUENCE_BITS;
    this.TIMESTAMP_SHIFT = this.SEQUENCE_BITS + this.MACHINE_ID_BITS;

    if (machineId > this.MAX_MACHINE_ID || machineId < 0) {
      throw new Error(`Machine ID must be between 0 and ${this.MAX_MACHINE_ID}`);
    }

    this.machineId = BigInt(machineId);
    this.sequence = 0n;
    this.lastTimestamp = -1n;
  }

  _timestamp() {
    return BigInt(Date.now());
  }

  _waitForNextMillis(lastTimestamp) {
    let timestamp = this._timestamp();
    while (timestamp <= lastTimestamp) {
      timestamp = this._timestamp();
    }
    return timestamp;
  }

  generateId() {
    let timestamp = this._timestamp();

    if (timestamp < this.lastTimestamp) {
      throw new Error("Clock moved backwards. Refusing to generate id");
    }

    if (timestamp === this.lastTimestamp) {
      this.sequence = (this.sequence + 1n) & this.MAX_SEQUENCE;
      if (this.sequence === 0n) {
        timestamp = this._waitForNextMillis(this.lastTimestamp);
      }
    } else {
      this.sequence = 0n;
    }

    this.lastTimestamp = timestamp;

    let id = ((timestamp - this.EPOCH) << this.TIMESTAMP_SHIFT) |
             (this.machineId << this.MACHINE_ID_SHIFT) |
             this.sequence;

    return id.toString();
  }
}

// Example usage
const machineId = 1;
const leaves = new Leaves(machineId);

for (let i = 0; i < 10; i++) {
  console.log(leaves.generateId());
}
```

## Installation as npm Package
To use this library as an npm package, follow these steps:

1. Create a new file leaves.js and paste the above code into it.
2. In your project folder, run the following command to initialize a new npm package:
   ```bash
   npm init -y
   ```
3. Create a file index.js in your project folder and use the library:
   ```javascript
   const Leaves = require('./leaves');

   const machineId = 1;
   const leaves = new Leaves(machineId);

   for (let i = 0; i < 10; i++) {
     console.log(leaves.generateId());
   }
   ```
4. Run the script:
   ```bash
   node index.js
   ```
