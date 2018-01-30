# Honeydew 🍈

[![Build Status](https://travis-ci.org/nosleepnotever/honeydew.svg?branch=master)](https://travis-ci.org/nosleepnotever/honeydew)
[![Coverage Status](https://coveralls.io/repos/github/nosleepnotever/honeydew/badge.svg?branch=master)](https://coveralls.io/github/nosleepnotever/honeydew?branch=master)  
[![Known Vulnerabilities](https://snyk.io/test/github/nosleepnotever/honeydew/badge.svg?targetFile=package.json)](https://snyk.io/test/github/nosleepnotever/honeydew?targetFile=package.json)

A lightweight worker class to automate promise-returning tasks with NO dependencies.

## Installation

`npm i honeydew`

## Usage

```
    const { Worker } = require('honeydew');

    const findTask = () => {
        // this will generally be a database query
        // to find relevant work to be done
        return db.Requests.findOne({status:'Queued'}).then(request=>{
             // this promise must return an object with an execute function
             // that itself returns a promise of the work to be done
            const task = {
                execute: () => {  
                    return VendorAPI.post(request).then(res => {
                        const response = Object.assign({}, res, { request: request._id});
                        return db.Responses.create(response);
                    });
                }
            };
            return task;
        })
    }

    // on initialization, the worker will begin to find and run tasks
    const worker = new Worker(findTask);
```

## Tests

`npm test`

## Contributing

In lieu of a formal style guide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code.
