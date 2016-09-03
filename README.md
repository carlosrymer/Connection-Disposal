```javascript
// Let's assume that we're using the mysql module for Node.js in this example.
const mysql = require('mysql');

// We'll use Bluebird.js instead of the native Promise
const Promise = require('bluebird');

// Now, let's create a function that returns a promise that resolves to a database connection
function getConnection() {
  return Promise.try(() => Promise.promisifyAll(mysql.createConnection({}))) // We're assuming default options here
    .disposer(connection => connection.destroy()); // Disposer will be called once getConnection resolves
}

// Now, let's use our function defined above with Promise.using
return Promise.using(getConnection(), connection => connection.queryAsync()); // Here we make our query, and once it's resolved, our disposer's callback will be executed, destroying the connection
```
