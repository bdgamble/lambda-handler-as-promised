# lambda-handler-as-promised

[![Build Status][ci-image]][ci-url]
[![Coverage Status][coverage-image]][coverage-url]
[![NPM version][npm-image]][npm-url]
[![Dependency Status][dependencies-image]][dependencies-url]

Tiny wrapper that ensures that lambda's callback is always called. In other words, from your handler you can return value, promise, throw exception, and this library will wrap your code into a promise while calling appropriate lambda-required callback. If you’d like, you can call callback by yourself, `lambda-handler-as-promised` will still behave in the same way.

## Installation

```bash
$ npm install lambda-handler-as-promised
```

## Usage

If you do things in a usual way, you'll construct your lambda handlers similar to this:

```js
const handler = function(event, context, callback) {
	try {
		const result = doSomething(event);

		if (result) {
			callback(null, result);
		} else {
			callback(new Error('Winter is coming!'));
		}
	} catch (err) {
		callback(err);
	}
}
```

With `lambda-handler-as-promised` you should not worry about top-level error handling, so you can write your handlers just like this:

```js
const lambdaHandler = require('lambda-handler-as-promised');

const handler = lambdaHandler(function(event) {
	const result = doSomething(event);
	if (result) ? return result : throw new Error('Winter is coming!');
});
```

Or like this:

```js
const lambdaHandler = require('lambda-handler-as-promised');

const handler = lambdaHandler(function(event) {
	if (/*something wrong*/) {
		throw new Error('Winter is coming!');
	}

	return doSomethingPromise(event);
});
```

Or this (if you really want to):

```js
const lambdaHandler = require('lambda-handler-as-promised');

const handler = lambdaHandler(function(event, context, callback) {
	const result = doSomething(event);

	if (result) {
		callback(null, result);
	} else {
		callback(new Error('Winter is coming!'));
	}
});
```

> Note: handler, created by `lambda-handler-as-promised`, returns Promise, so you can use it, for example, to simplify your testing.

## License

The MIT License (MIT)

Copyright (c) 2016 Anton Bazhal

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

[ci-image]: https://circleci.com/gh/AntonBazhal/lambda-handler-as-promised.svg?style=shield&circle-token=fc9c3e6f415d2d338800c8a08d6155708ad260ce
[ci-url]: https://circleci.com/gh/AntonBazhal/lambda-handler-as-promised
[coverage-image]: https://coveralls.io/repos/github/AntonBazhal/lambda-handler-as-promised/badge.svg?branch=master
[coverage-url]: https://coveralls.io/github/AntonBazhal/lambda-handler-as-promised?branch=master
[dependencies-url]: https://david-dm.org/antonbazhal/lambda-handler-as-promised
[dependencies-image]: https://img.shields.io/david/AntonBazhal/lambda-handler-as-promised.svg
[npm-url]: https://www.npmjs.org/package/lambda-handler-as-promised
[npm-image]: https://img.shields.io/npm/v/lambda-handler-as-promised.svg