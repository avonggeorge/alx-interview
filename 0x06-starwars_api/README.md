# Star Wars API

The “0. Star Wars Characters” project requires you to interact with an external API to fetch and display information about Star Wars characters based on the movie ID provided as an argument. To successfully complete this project, you need to be familiar with several key concepts related to web programming, API interaction, and asynchronous programming in JavaScript.
### Concepts Needed:

##  **HTTP Requests in JavaScript**:
    
    -   Understanding how to make HTTP requests to external services using the  `request`  module or alternatives like  `fetch`  in Node.js.

Requests are a way to exchange information between a client and a server. The client and server has to be present before we can call it a request. The client has to be the one to initiate a request by which the server in return, send a response in accordance to the request.
 HTTP requests contains three elements which includes:

-  The request method which I called the components earlier.
-  The target, where the request is going to. This is usually the URL.
-  The protocol name and its version.
**Components to consider before making a HTTP request**
*Method (GET, POST, PUT, DELETE, etc)*
*Data:*
*Headers*
*Authentication*
### **Ways to make HTTP requests in Node**

 We will discuss 5 ways to make HTTP GET and POST request. 

#### **1. HTTP Module**

This is a built in standard library or module that is basically used to build a HTTP client and server. For example, we can create a Web server that listens to HTTP requests and we can use the same module to make HTTP requests.

**GET request**
```

const https = require("https");

https
  .get(`https://reqres.in/api/users`, resp => {
    let data = "";

    // A chunk of data has been recieved.
    resp.on("data", chunk => {
      data += chunk;
    });

    // The whole response has been received. Print out the result.
    resp.on("end", () => {
      let url = JSON.parse(data).message;
      console.log(url);      
    });
  })
  .on("error", err => {
    console.log("Error: " + err.message);
  });
```
In the above code, we make a GET request to the Dogs API, the resp is an object and inside that is two events that we are to listen to; the on data and on end. With the on data, we are streaming the data to us in chunks(pieces by pieces) and with the on end, we listen and parse our data when it is completed. We also have the error handler event on error that listens for errors.
**POST request**
```

const https = require("https");

const options = {
  hostname: 'yourapi.com',
  port: 443,
  path: '/todos',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': data.length
  }
}

https
  .request(options, resp => {
    // log the data
    resp.on("data", d => {
      process.stdout.write(d);
    });
  })
  .on("error", err => {
    console.log("Error: " + err.message);
  });
```
#### **2. Axios**
According to Axios, it is a promised-based HTTP client for the browser and Node as well. This means that it is a third party library that can be used on any JavaScript project
**Install Axios**
```

$ npm install axios
```
**simple GET request**
```

const axios = require('axios').default;

async function getUsers() {
  try {
    const response = await axios.get(`https://reqres.in/api/users`);
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
getUsers()
```
**A POST request to the same API**
```

const axios = require('axios').default;

const data = {
  "name": "victor",
  "job": "writer"
}

async function addUser(data) {
  try {
    const response = await axios.post(`https://reqres.in/api/users`, data);
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}

addUser()
```
Our axios.post() takes in two parameters which are the url and the data to be sent to the server
#### **3. Got**
This is another HTTP client library used for making requests in Node applications. It works almost like Axios but there a few differences in how they work. The first is that this library can not be used on the browser side i.e on the client side. It offers a Stream and Pagination API out of the box. The Got library is a native ESM which means that you have to use the import rather than CommonJS. Oh yeah, it is also a Promise-based API too.

GET a request with Got:
```

import got from 'got';

async function getUsers() {
  try {
   const response = await got.get('https://reqres.in/api/users').json();
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
}

getUsers()
```
POST request;
```

import got from 'got';

const data = {
  "name": "victor",
  "job": "writer"
}

async function addUser(data) {
  try {
   const response = await got.get('https://reqres.in/api/users', 
    {
      json: data
    }).json();
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
}

addUser()
```
### **4. Node-Fetch**
This is another lightweight HTTP library that works just like the browser fetch API.
### **5. SuperAgent**
THis library is a VisionMedia project with over 29 million downloads per month. It is a composable and promise-based API that retries on failure, follows redirects, handles gzip, has a JSON mode, and can also cancel requests.
-   [A Complete Guide to Making HTTP Requests in Node.js](https://intranet.alxswe.com/rltoken/iRse23lnV4gAsD9JJTJMMQ "A Complete Guide to Making HTTP Requests in Node.js") for details


## **Working with APIs**:
### What is a RESTful API?

A **RESTful API** (Representational State Transfer API) is a web service that allows interaction between a client (like a browser or mobile app) and a server over HTTP. It provides access to resources—such as data about users, products, or any other entities—via a set of URLs (or endpoints).

REST APIs follow a set of principles to ensure consistent communication between systems:

-   **Statelessness**: Each API call is independent, and the server doesn’t remember previous interactions.
-   **Client-Server Separation**: The client and server are separate entities, allowing flexibility on both ends.
-   **Resource-Based**: API endpoints represent resources (e.g., `/users` for user data, `/products` for product data).
-   **Standard HTTP Methods**: The API uses standard HTTP methods to perform actions on resources.

### Key HTTP Methods in REST APIs

1.  **GET**: Retrieve data from the server. (e.g., get user information)
2.  **POST**: Send new data to the server. (e.g., create a new user)
3.  **PUT**: Update existing data on the server. (e.g., update user details)
4.  **DELETE**: Remove data from the server. (e.g., delete a user)

### Understanding API Endpoints

An **endpoint** is a URL that defines the location of a resource:

-   **Base URL**: The main part of the URL, which points to the server. Example: `https://api.example.com`
-   **Path**: Specific location for the resource. Example: `/users` or `/users/{id}`
-   **Query Parameters**: Additional filters or settings, added to the URL with `?` and `&`. Example: `/users?active=true`

### Sample REST API Example

For an API with a base URL like `https://api.example.com`:

-   **`GET /users`**: Retrieves a list of all users.
-   **`POST /users`**: Creates a new user.
-   **`GET /users/123`**: Retrieves the user with ID `123`.
-   **`PUT /users/123`**: Updates the user with ID `123`.
-   **`DELETE /users/123`**: Deletes the user with ID `123`.

----------

### Interacting with RESTful APIs in JavaScript

You can interact with RESTful APIs using JavaScript with tools like **`fetch`** (in browsers or with Node.js’s `node-fetch`) or **Axios**.

#### 1. Using `fetch` for API Requests

**GET Request Example**:
```
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```
**POST Request Example**:
```
fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({ name: 'New User', email: 'user@example.com' })
})
  .then(response => response.json())
  .then(data => console.log('User created:', data))
  .catch(error => console.error('Error:', error));
```
#### 2. Using Axios for API Requests

Axios is a popular library for handling HTTP requests, and it simplifies the syntax:

**GET Request Example**:
```
const axios = require('axios');

axios.get('https://api.example.com/users')
  .then(response => console.log(response.data))
  .catch(error => console.error('Error:', error));
```
**POST Request Example**:
```
axios.post('https://api.example.com/users', {
    name: 'New User',
    email: 'user@example.com'
  })
  .then(response => console.log('User created:', response.data))
  .catch(error => console.error('Error:', error));
```
### Handling Responses and Errors

Most APIs return JSON data, so after fetching, you’ll often parse it:

-   **Parsing JSON**: With `fetch`, use `.json()` on the response to convert it to JavaScript objects.
-   **Error Handling**: Use `.catch()` for `fetch` and `axios`, or add specific handling within `try-catch` blocks for `async`/`await`.

### Best Practices for Interacting with REST APIs

-   **Use Async/Await**: Makes asynchronous code more readable.
-   **Check Status Codes**: Check the response’s status code to handle errors gracefully (e.g., `404` for not found, `500` for server errors).
-   **Use Environment Variables**: For sensitive data like API keys, use environment variables to keep them secure.

### Managing asynchronous operations with callbacks, promises, and async/await syntax.
Managing asynchronous operations in JavaScript is crucial when working with APIs, as network requests, file handling, and similar tasks take time to complete. Let’s dive into how to handle these operations with **callbacks**, **promises**, and **async/await** syntax.
### 1. Callbacks

A **callback** is a function passed as an argument to another function and is executed after the completion of an asynchronous operation. However, callbacks can lead to "callback hell" if nested too deeply, making the code hard to read and maintain.

**Example: Using Callbacks with `setTimeout`**
```
function fetchData(callback) {
  setTimeout(() => {
    callback('Data fetched!');
  }, 2000);
}

fetchData((data) => {
  console.log(data);  // Output: Data fetched! (after 2 seconds)
});
```
**API Request Example Using Callbacks with `XMLHttpRequest`**:
```
function getData(url, callback) {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.onload = () => {
    if (xhr.status === 200) {
      callback(null, JSON.parse(xhr.responseText));
    } else {
      callback(`Error: ${xhr.status}`);
    }
  };
  xhr.send();
}

getData('https://api.example.com/data', (error, data) => {
  if (error) {
    console.error(error);
  } else {
    console.log(data);
  }
});
```
### 2. Promises

A **promise** is an object that represents a value that may not be available yet, but will be resolved (or rejected) in the future. Promises have three states: **pending**, **fulfilled**, and **rejected**. Promises allow you to avoid deeply nested callbacks by chaining `.then()` and `.catch()` methods.

**Example of a Promise**
```
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const success = true;
      if (success) {
        resolve('Data fetched!');
      } else {
        reject('Failed to fetch data');
      }
    }, 2000);
  });
}

fetchData()
  .then(data => console.log(data))       // Output: Data fetched!
  .catch(error => console.error(error));
```
**API Request Example Using `fetch` and Promises**:
```
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```
### 3. Async/Await

**Async/await** syntax, introduced in ES2017, provides a cleaner, more readable way to handle asynchronous operations, allowing you to write code that looks synchronous but operates asynchronously. It’s essentially syntactic sugar over promises.

-   **`async`**: Declares an asynchronous function. It always returns a promise.
-   **`await`**: Pauses the execution of the function until the promise is resolved.

**Example of Async/Await**
```
async function fetchData() {
  try {
    const data = await new Promise((resolve, reject) => {
      setTimeout(() => resolve('Data fetched!'), 2000);
    });
    console.log(data);  // Output: Data fetched!
  } catch (error) {
    console.error(error);
  }
}

fetchData();
```
**API Request Example Using Async/Await with `fetch`**:
```
async function getData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error:', error);
  }
}

getData('https://api.example.com/data');
```
### Handling API Response Data Asynchronously

Using `async/await` with APIs provides clean handling of response data. Here’s a step-by-step example of handling response data and incorporating error handling:

```
async function fetchStarWarsCharacter(id) {
  const url = `https://swapi.dev/api/people/${id}/`;

  try {
    // Await the fetch operation
    const response = await fetch(url);
    
    // Check for HTTP errors
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`);
    }
    
    // Await and parse JSON data
    const character = await response.json();
    console.log(`Character: ${character.name}`);
  } catch (error) {
    console.error('Failed to fetch character data:', error);
  }
}

// Fetch data for Star Wars character with ID 1
fetchStarWarsCharacter(1);
```
### Summary

-   **Callbacks**: Useful but can lead to "callback hell" with nested functions.
-   **Promises**: Provide better readability and error handling with `.then()` and `.catch()` methods.
-   **Async/Await**: Simplifies handling of promises, making asynchronous code look synchronous and easier to read.

When interacting with APIs, `async/await` is generally preferred due to readability and straightforward error handling, making it ideal for modern JavaScript development.

## Command Line Arguments in Node.js:
In Node.js, you can access command-line arguments passed to your script using the `process.argv` array. This array contains all the command-line arguments provided to the script, with the first two elements being special.

### **1. Understanding `process.argv`**

The `process.argv` array contains the following elements:

-   **Element 0**: The path to the Node.js executable (e.g., `/usr/bin/node`).
-   **Element 1**: The path to the script being executed (e.g., `/path/to/script.js`).
-   **Element 2 and beyond**: Any additional arguments provided on the command line after the script name.

### Example:

If you run a script like this:
```
node script.js arg1 arg2 arg3
```
Then `process.argv` will look like:
```
['/usr/bin/node', '/path/to/script.js', 'arg1', 'arg2', 'arg3']
```
### **2. Accessing Command-Line Arguments**
To access the arguments passed to your script, you can use `process.argv` starting from index 2 (ignoring the first two elements).

```
// script.js
console.log(process.argv);
```
When you run the script:
```
node script.js arg1 arg2 arg3
```
Output:
```
[ '/usr/bin/node', '/path/to/script.js', 'arg1', 'arg2', 'arg3' ]

```
### **3. Parsing Command-Line Arguments**

If you need to handle command-line arguments more dynamically, such as for options and flags (e.g., `--name`), you’ll want to parse them.

You can manually parse arguments or use a library like `minimist` or `yargs` to simplify this process.

#### **Manual Parsing Example**

Let’s manually parse the arguments from `process.argv` and implement some simple logic:
```
// script.js
const args = process.argv.slice(2);  // Get all arguments after script name

console.log('Arguments:', args);

const nameIndex = args.indexOf('--name');
if (nameIndex !== -1 && args[nameIndex + 1]) {
  console.log('Name:', args[nameIndex + 1]);
} else {
  console.log('No name provided');
}

const ageIndex = args.indexOf('--age');
if (ageIndex !== -1 && args[ageIndex + 1]) {
  console.log('Age:', args[ageIndex + 1]);
} else {
  console.log('No age provided');
}
```
If you run the script:
```
node script.js --name John --age 30
```
Output:
```
Arguments: [ '--name', 'John', '--age', '30' ]
Name: John
Age: 30
```
This is a basic way to parse arguments manually. You can search for flags (e.g., `--name`, `--age`) and extract their values.

#### **Using `minimist` for Argument Parsing**

To simplify argument parsing, you can use a library like **`minimist`**. This library automatically parses the command-line arguments into an object, where flags become keys, and their values become the corresponding values.

1.  **Install `minimist`**:
```
npm install minimist
```
2. **Example with `minimist`**:
```
// script.js
const minimist = require('minimist');

const args = minimist(process.argv.slice(2));
console.log(args);
```
3. **Running the script**:
```
node script.js --name John --age 30
```
Otput:
```
{ _: [], name: 'John', age: 30 }
```
In the above example:

-   The `--name` and `--age` flags are parsed into keys (`name`, `age`) with their respective values.
-   The `_: []` is a default property used by `minimist` to store positional arguments (if any).

#### **Using `yargs` for Argument Parsing**

Another popular library for command-line argument parsing is **`yargs`**. It offers more advanced features, such as command-based parsing, argument validation, and built-in help.

1.  **Install `yargs`**:
```
npm install yargs
```
2. **Example with `yargs`**:
```
// script.js
const yargs = require('yargs');

const argv = yargs
  .option('name', {
    alias: 'n',
    description: 'The name of the user',
    type: 'string',
  })
  .option('age', {
    alias: 'a',
    description: 'The age of the user',
    type: 'number',
  })
  .argv;

console.log(argv);
```
3. **Running the script**:
```
node script.js --name John --age 30
```
Output:
```
{
  _: [],
  name: 'John',
  age: 30,
  '$0': 'script.js'
}
```
-   The `name` and `age` arguments are automatically parsed, and you can use aliases like `-n` for `--name`.

### **4. Summary**

-   **`process.argv`**: Use it to access the raw command-line arguments in a Node.js script.
-   **Manual Parsing**: You can manually slice and handle `process.argv` for basic scenarios.
-   **Third-Party Libraries**:
    -   **`minimist`**: A simple and minimalistic library for parsing command-line arguments into an object.
    -   **`yargs`**: A more powerful library that supports features like argument validation, commands, and help generation.

Using a library like `minimist` or `yargs` simplifies the parsing process, especially for complex or large applications.

## Array Manipulation and Iteration:
In JavaScript, array manipulation and iteration are key for tasks like filtering, transforming, and displaying data. Here’s a rundown on essential methods for working with arrays, especially when you need to format and display information like character names from an API response.

### 1. **Iterating Over Arrays**

Several methods allow you to loop through arrays, each with its own benefits:

#### **forEach**

`forEach` is ideal for simple iteration where you need to perform an action on each item but don’t need to create a new array.
```
const characters = ['Luke Skywalker', 'Darth Vader', 'Leia Organa'];

characters.forEach((character, index) => {
  console.log(`Character ${index + 1}: ${character}`);
});
```
#### **map**

`map` is useful for transforming each element in an array and returns a new array of the same length.
```
const characterLengths = characters.map(character => character.length);
console.log(characterLengths); // Output: [13, 11, 11]
```
### 2. **Filtering and Finding Elements**

When you need to filter or search within an array, `filter` and `find` are powerful methods:

#### **filter**

`filter` returns a new array with elements that match a specified condition.
```
const longNames = characters.filter(character => character.length > 10);
console.log(longNames); // Output: ['Luke Skywalker', 'Darth Vader']
```
#### **find**

`find` returns the first element that satisfies a condition, or `undefined` if none match.
```
const foundCharacter = characters.find(character => character.startsWith('Leia'));
console.log(foundCharacter); // Output: 'Leia Organa'
```
### 3. **Reducing Arrays**

The `reduce` method is used to combine all elements of an array into a single value.
```
const nameLengthsSum = characters.reduce((total, character) => total + character.length, 0);
console.log(nameLengthsSum); // Output: 35
```
### 4. **Sorting Arrays**

To sort an array, `sort` can be used with or without a comparison function:
```
const sortedCharacters = [...characters].sort(); // Default alphabetical sort
console.log(sortedCharacters); // Output: ['Darth Vader', 'Leia Organa', 'Luke Skywalker']
```
### 5. **Array Transformation Example: Formatting Character Names**

Suppose you have an array of character objects with name and rank properties, and you need to format them into a readable list.
```
const characterDetails = [
  { name: 'Luke Skywalker', rank: 'Jedi' },
  { name: 'Darth Vader', rank: 'Sith Lord' },
  { name: 'Leia Organa', rank: 'Princess' }
];

const formattedCharacters = characterDetails.map(
  character => `${character.name} - ${character.rank}`
);

console.log(formattedCharacters);
/* Output:
[
  'Luke Skywalker - Jedi',
  'Darth Vader - Sith Lord',
  'Leia Organa - Princess'
]
*/
```
### 6. **Putting It All Together: Fetching, Filtering, and Formatting**

Here’s how you might fetch data, filter characters by rank, and format their names:

```
// Mock data
const starWarsCharacters = [
  { name: 'Luke Skywalker', rank: 'Jedi' },
  { name: 'Darth Vader', rank: 'Sith Lord' },
  { name: 'Yoda', rank: 'Jedi Master' },
  { name: 'Leia Organa', rank: 'Princess' }
];

// Get only Jedi characters, then format their names
const jediCharacters = starWarsCharacters
  .filter(character => character.rank.includes('Jedi'))
  .map(character => `${character.name} - ${character.rank}`);

console.log(jediCharacters);
/* Output:
[
  'Luke Skywalker - Jedi',
  'Yoda - Jedi Master'
]
*/
```
### Summary

-   **forEach** for simple iteration.
-   **map** to create a transformed array.
-   **filter** to select elements based on conditions.
-   **reduce** to combine array elements into a single output.
-   **sort** to arrange elements, often with a custom comparator.

These array methods make it easy to process and format character data, especially in projects that involve fetching and displaying information from APIs.