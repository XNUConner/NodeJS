# NodeJS
Notes taken and code written while learning NodeJS <br />

## Installation Process (linux-amd64)
```
wget https://nodejs.org/dist/v14.16.0/node-v14.16.0-linux-x64.tar.xz
sudo mkdir /usr/local/lib/node
tar -xvf node-v14.16.0-linux-x64.tar.xz
mv node-v14.16.0-linux-x64/ nodejs/
sudo mv nodejs/ /usr/local/lib/node/

echo NODEJS_HOME=/usr/local/lib/node/nodejs >> ~/.bashrc
echo PATH=$NODEJS_HOME/bin:$PATH

source ~/.bashrc 
```

## REPL
We have access to node's REPL mode, Read-Eval-Print-Loop <br />
It is similar to Python IDLE. <br />
```
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

## Variable definitions
```
                          // Strings are immutable
const name = "XNUConner"  // Cannot be reassigned, scope: function
let name = "XNUConner"    // Can be reassigned, scope: containing code block
var name = "XNUConner"    // Can be reassigned, scope: function
```

### String syntax
```
// Template literals
`text`

`multi line
text string`

`my text ${expression} my text`

myfunc`my text ${expression} my text`
```

# Async promise chaining

