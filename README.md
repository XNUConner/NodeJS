# NodeJS
Notes taken and code written while learning NodeJS <br />

## What is NodeJS?
- NodeJS is a server-side javascript interpreter. <br />
- Javascript is typically run on the browser, facilitated by an engine, there are multiple engines, and Chrome uses V8. <br />
- NodeJS was created by essentially ripping out Chrome's V8 engine and modifying it to run on an OS. <br />
- Many NodeJS function calls become C++ function calls through NodeJS's [process binding loader](https://github.com/nodejs/node/blob/17a527ec07c69e063a2479a5a87df445a23e43ac/lib/internal/bootstrap/loaders.js). <br />
- NodeJS processes' run on single threads, there is no same-process multithreading due to how the V8 engine was built. <br />
- *V8 does not precompile JS code into C++ binaries, it uses JIT compilation.* <br />

## Installation Process (linux-amd64)
```shell
wget https://nodejs.org/dist/v14.16.0/node-v14.16.0-linux-x64.tar.xz
sudo mkdir /usr/local/lib/node
tar -xvf node-v14.16.0-linux-x64.tar.xz
mv node-v14.16.0-linux-x64/ nodejs/
sudo mv nodejs/ /usr/local/lib/node/

echo NODEJS_HOME=/usr/local/lib/node/nodejs >> ~/.bashrc
echo PATH=$NODEJS_HOME/bin:$PATH

source ~/.bashrc 
```

## Starting a NodeJS process
Enter this in shell, typically `entrypoint.js` will be `app.js` <br />
```
$ node entrypoint.js
```

## REPL
We have access to node's REPL mode, Read-Eval-Print-Loop <br />
It is similar to Python IDLE. <br />
```javascript
$ node
> const name = "XNUConner"
undefined
> name
'XNUConner'
>
```

## Package installation
We can use `npm` to install node packages, much like Python's `pip` <br />
```
npm install axios // install axios into ./node_modules

(or)

npm i axios       // Same as above

(or)

npm add axios     // Same as above
```
package.json will automatically be updated to include `axios` as a dependency. <br />
Options for `npm install` can be viewed with `npm help install` <br />
Common options are `[-P|--save-prod|-D|--save-dev|-O|--save-optional] [-E|--save-exact] [-B|--save-bundle] [--no-save] [--dry-run]` <br />
They can be used to change the dependency location in `package.json` <br />
Packages can have their modules used in code, see below. <br />

## Module use
To use a module in code, we can initialize it like so: <br />
```javascript
const axios = require('axios');
const fs = require('fs');
```

## Variable definitions
```javascript
                          // Strings are immutable
const name = "XNUConner"  // Type cannot change but contents actually can, scope: function
let name = "XNUConner"    // Can be reassigned, scope: containing code block
var name = "XNUConner"    // Can be reassigned, scope: function
```

### String syntax
```javascript
// Template literals
`text`

`multi line
text string`

`my text ${expression} my text`

mytag`my text ${expression} my text`
```

### Function syntax
**Function** <br />
```javascript
function myFunction(parameter1, parameter2) {
  return val;
}
```

**Arrow Function** <br />

**Anonymous Function** <br />

### Async callback
Callbacks (swift: completion handlers) aren't the preffered method of handling async code in NodeJS: <br />
```javascript
const request = require('request');
const fs = require('fs');

request('https://ghibliapi.herokuapp.com/films', (error, response, body) => {
    if (error) {
        console.error(`Could not send request to API: ${error.message}`);
        return;
    }

    if (response.statusCode != 200) {
        console.error(`Expected status code 200 but received ${response.statusCode}.`);
        return;
    }

    console.log('Processing our list of movies');
    movies = JSON.parse(body);
    let movieList = '';
    movies.forEach(movie => {
        movieList += `${movie['title']}, ${movie['release_date']}\n`;
    });

    fs.writeFile('callbackMovies.csv', movieList, (error) => {
        if (error) {
            console.error(`Could not save the Ghibli movies to a file: ${error}`);
            return;
        }

        console.log('Saved our list of movies to callbackMovies.csv');
    });
});
```
callbacks aren't preferred because if an async function calls other async functions, multiple nested callbacks will be required, leading to messy code. <br />

### Promises
The `Promise()` function is used in async functions in order to have them return as if they were synchronous. <br />
The function returns an immediate `Promise` object to represent an async call which returns `Pending`, `Fulfilled`, or `Rejected` <br />
```javascript
function doRequest(request) {
    return new Promise( (resolve, reject) => {
        // Complete a web request
        let data = responseData
        // Mark as resolved, function will return responseData
        resolve(responseData)
    })
}
```

### Promise chaining (with .then())
Rather than callbacks, promise chaining is preffered in NodeJS: <br />
```javascript
const axios = require('axios');
const fs = require('fs').promises;


axios.get('https://ghibliapi.herokuapp.com/films')
    .then((response) => {
        console.log('Successfully retrieved our list of movies');
        let movieList = '';
        response.data.forEach(movie => {
            movieList += `${movie['title']}, ${movie['release_date']}\n`;
        });

        return fs.writeFile('promiseMovies.csv', movieList);
    })
    .then(() => {
        console.log('Saved our list of movies to promiseMovies.csv');
    })
    .catch((error) => {
        console.error(`Could not save the Ghibli movies to a file: ${error}`);
    });
```
`response` written inside `.then()` represents the return value of the async call above it. <br /> 
If if any promise in the chain is rejected, program execution immediately jumps to the `.catch()` block. <br />

### Async await
Even more concise than `.then()`, we can chain async function calls with the `async` and `await` keywords: <br />
```javascript
const axios = require()
async function saveMovies() {
    try {
        const response = await axios.get('https://ghibliapi.herokuapp.com/films');
        let movieList = '';
        response.data.forEach(movie => {
            movieList += `${movie['title']}, ${movie['release_date']}\n`;
        });
        await fs.writeFile('asyncAwaitMovies.csv', movieList);
    } catch (error) {
        console.error(`Could not save the Ghibli movies to a file: ${error}`);
    }
}
```
This makes our NodeJS code look more like a typical function, much easier to read. <br />

### Ending a NodeJS process
`process.exit(1)` - Exit current process with exit code 1 <br />
Exit process on POSIX signal: <br />
```javascript
process.on('SIGTERM', () => {
  // Do cleanup first 
  process.exit(EXIT_CODE);
})
```
`process.kill(process.pid, 'SIGTERM')` - Kill current process with self-inflicted SIGTERM <br />
