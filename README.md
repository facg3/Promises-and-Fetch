# Promises-and-Fetch

![pic](https://www.tutomena.com/wp-content/uploads/2015/10/javascript-promises.png)

# What are JavaScript Promises ?
A promise represents the eventual result of an asynchronous operation. It is a placeholder into which the successful result value or reason for failure will materialize.  

 A promise may be in one of 3 possible states: 

- fulfilled: meaning that the operation completed successfully.
- rejected: meaning that the operation failed.
- pending: initial state, neither fulfilled nor rejected.



-- Syntax

If you’ve done any app in JavaScript, you have probably had to face callbacks, nested inside of callbacks, nested inside of callbacks.
You may end up with code that looks something like this:


```js
function isUserTooYoung(id, callback) {
    openDatabase(function(db) {
        getCollection(db, 'users', function(col) {
            find(col, {'id': id},function(result) {
                result.filter(function(user) {
                    callback(user.age < cutoffAge)
                })
            })
        })
    })
}
```



Pretty difficult to follow. And it can get much worse. In our current node.js codebase we sometimes do as many as ten sequential, asynchronous calls. That would be a lot of nesting. Thankfully, there’s a much better way: promises.

The following is the above code rewritten using promises:


```js
function isUserTooYoung(id) {
    return openDatabase()
        .then(getCollection)
        .then(find.bind(null, {'id': id}))
        .then(function(user) {
            return user.age < cutoffAge;
        });
}
```

## What are fetches
A fetch is a request -Similar to the XHTTPRequest- that takes in:
- path: path to file wishing to be read
- init: an argument that allows you to control a bunch of things *(optional)*

and returns a promise with the response

### Syntax eg.
```js
fetch('./public/index.html')
.then( (response) => {
  return response;
})
.catch( (err) => {
  throw new Error('Ops! Something wrong happened!');
}
```
In this case, the fetch brings the file ``` index.html ``` and returns it's contents in the response. The second init argument could is an object containing the options that could be set for the requrest.

### Syntax eg.
```js
const init = { method: 'GET',
               headers: myHeaders,
               mode: 'cors',
               cache: 'default' 
              };
const path = './public/index.html';

fetch(path , init)
.then( (response) => {
  return response;
});
```
A descritpive explaination for the options is found [here](https://developer.mozilla.org/en-US/docs/Web/API/Request/Request)

## fetch error handling
responses in fetch have a property ```ok ``` that describe the state of the response, wheather if it was successful or not. 

### Syntax eg.
```js
const init = { method: 'GET',
               headers: myHeaders,
               mode: 'cors',
               cache: 'default' 
              };
const path = './public/index.html';

const request = new Request(path, init);

fetch(request)
.then( (response) => {
  if(response.ok) return response;
  throw new Error('Ops! Something went wrong!');
})
.catch( (err) => {
  console.log(err);   // imagine this as error handling
});
```
## what are the downsides of using Fetch?
- doesn’t send cookies by default if the server uses cookie based authentication
- the server needs to know that the client will be able to handle a JSON encoded response
- the server in on a different sub-domain and CROS* is disabled by default in fetch
- fetch doesn’t give any mechanism to override defaults
- ```response.json()``` will throw an exception if the server responded with 204 (sucsess, no content)

#### Advantages of Promises

1. Flat structure
2. Terse error handling
3. Point-free syntax (where possible)
4. Compositional pipeline of functions
5. Can use throw
If you’re observant, you’ll also notice these disadvantages:

#### Disadvantages of Promises

1. Can only operate on a single value at a time (Arrays and/or Promise.all helps you work around this, though)
2. Execution begins immediately when the Promise is created.
3. Possible performance issues. Due to the runtime overhead of monitoring the state of a Promise, they are slower than callbacks (although this might improve as more JS engines start supporting them natively).
4. Not available on all JS engines. In older engines, they have to be polyfilled.
![pic](http://i.imgur.com/MByWioX.png)
