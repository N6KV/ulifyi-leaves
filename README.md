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
