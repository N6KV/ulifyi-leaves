# Ulifyi Leaves

[![npm version](https://img.shields.io/npm/v/ulifyi-leaves.svg)](https://www.npmjs.com/package/ulifyi-leaves)
[![npm downloads](https://img.shields.io/npm/dm/ulifyi-leaves.svg)](https://www.npmjs.com/package/ulifyi-leaves)
[![GitHub license](https://img.shields.io/github/license/Ulifyi/ulifyi-leaves)](https://github.com/Ulifyi/ulifyi-leaves/blob/main/LICENSE)

Ulifyi Leaves is a JavaScript library for generating unique Leaf IDs similar to Twitter's Snowflake IDs. It allows you to easily integrate globally unique identifier generation into your applications.

## Features

- Generate unique Leaf IDs.
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

### Generate Leaf ID

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
- `generateId()`: string: Generates a unique Leaf ID.

## Contributing
Contributions are welcome! Please feel free to submit a pull request.

## License
This project is licensed under the MIT License - see the [LICENSE](https://github.com/Ulifyi/ulifyi-leaves/tree/main?tab=MIT-1-ov-file#readme) file for details.
   node index.js
   ```
