# Promises-and-Fetch

![pic](https://www.tutomena.com/wp-content/uploads/2015/10/javascript-promises.png)

# What is JavaScript Promises ?
A promise represents the eventual result of an asynchronous operation. It is a placeholder into which the successful result value or reason for failure will materialize.  

 A promise may be in one of 3 possible states: 

- fulfilled: meaning that the operation completed successfully.
- rejected: meaning that the operation failed.
- pending: initial state, neither fulfilled nor rejected.



-- Syntax

If you’ve done any app in JavaScript, you have probably had to face callbacks, nested inside of callbacks, nested inside of callbacks.
You may end up with code that looks something like this:


```
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


```
function isUserTooYoung(id) {
    return openDatabase()
        .then(getCollection)
        .then(find.bind(null, {'id': id}))
        .then(function(user) {
            return user.age < cutoffAge;
        });
}
```
