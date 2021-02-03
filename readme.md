# Node

we can build applications that can interact with millions of users without blocking using Node. javascript doesn't allow us to Interact with the OS directly we can't create of modify the file or interact with cpu but we can use C/C++ that has many features to directly interact with OS so need combination of both C/C++ and JS that is the node

using node, javascript easily access the computer feature by labeling. javascript use labels and using libuv that labels use c++ functions that allow us to interact with os. eg ->
1.http
2.fs etc.

steps ->

1. js use label.
2. using libuv label use C++ functionsl
3. C++ interact with the OS directly.
4. js + c++ -> Node.js
   eg -> js label http,fs are in the c++.

in node js does 3 main things by itself ->

1. save data.
2. use that data.
3. use labels to interact with C++.
   eg ->

```javascript
// Step 1 Saving all the data
let num = 3;
function multiply_by_2(input) {
	const result = 2 * input;
	return result;
}

// Step 2 use saved data
const output1 = multiply_by_2(num); // 6
const output2 = multiply_by_2(10); // 20

// Step 3 use label to use c++ feature
const fs = require('fs');
```

# Node APIs

when we create our code than it use lable named server to access C++ feature that enables us to create a server that returns us an object contains method then we uses listen method from that object to interact with the client.

default port is 80.

```javascript
const server = http.createServer();
server.listen(80);
```

![working of code](img/server/step1.jpg)

```javascript
function doOnIncoming(incomingData, functionToSetOutgoingData) {
	functionToSetOutgoingData.end('Welcome');
}
const server = http.createServer(doOnIncoming);
server.listen(80);
```

![working of code](img/server/step2.jpg)

when **createServer** executed it automatically call **doOnIncoming** function that is passed as an argument. because javascript is single threaded synchronous so nodes slow work would done in the backgrosund.

node create two object instant when message get. first object contains url and the second object has bunch of methods.
so when **doOnIncoming** get executed it passes to arguments to that function one for incomingData and second for functionToSetOutgoingData.

![working of code](img/server/step3.jpg)

# Node With HTTP

```javascript
const tweets = ['Hi', ' ', 'Hello', ' ', ' '];
function doOnIncoming(incomingData, functionsToSetOutgoingData) {
	const tweetNeeded = incomingData.url.slice(8) - 1;
	functionsToSetOutgoingData.end(tweets[tweetNeeded]);
}
const server = http.createServer(doOnIncoming);
server.listen(80);
```

![http code working](img/http/step1.jpg)

![http code working](img/http/step2.jpg)

this is the work done by the server.

data is in http format. headers use to metadata and body for the data.

send message -> http -> format

end() -> in the node set out http data

data -> collection of chunks

node must be restart for every change but we can use nodemon to get rid from restart again and again.

ssh used for interfacing with computer.

# Event & Error Handling

events used on server for handling conditions like client side error etc.

http -> in node -> open socket
and return object.

error comes first as parameter in node.

```javascript
function doOnIncoming(incomingData, functionsToSetOutgoingData) {
	functionsToSetOutgoingData.end('Welcome to Twitter');
}
function doOnError(infoOnError) {
	console.error(infoOnError);
}
const server = http.createServer();
server.listen(80);
server.on('request', doOnIncoming);
server.on('clientError', doOnError);
```

![code work](img/event.jpg)

# File System

```javascript
function cleanTweets(tweetsToClean) {
	// code that removes bad tweets
}
function useImportedtweets(errorData, data) {
	const cleanedTweetsJson = cleanTweets(data);
	const tweetsObj = JSON.parse(cleanedTweetsJson);
	console.log(tweetsObj.tweet2);
}
fs.readFile('./tweets.json', useImportedtweets);
```

![code work](img/file_system.jpg)

callstack ->
js way to track code.

json data are partially in buffer.

onClose event take all stringify json and turned into object.

# Streams

data can be get into the batches in the background simultaneously. default batch size is 64k byte.

```javascript
let cleanedTweets = '';
function cleanTweets(tweetsToClean) {
	// algorithm to remove bad tweets from `tweetsToClean`
}
function doOnNewBatch(data) {
	cleanedTweets += cleanTweets(data);
}
const accessTweetsArchive = fs.createReadStream('./file.json');
accessTweetsArchive.on('data', doOnNewBatch);
```

![code work](img/stream.jpg)

# Asynchronicity in Node

Some queues in nodejs ->

1. TimerQueue.
2. IOQueue(Network, File all data almost 95%).
3. CheckQueue.
4. CloseQueue.
5. MicrotaskQueue it has higher priority it's actually is two.

```javascript
function useImportedtweets(errorData, data) {
	const tweets = JSON.parse(data);
	console.log(tweets.tweet1);
}
function immediately() {
	console.log('Run me last');
}
function printHello() {
	console.log('Hello');
}
function blockFor500ms() {
	// Block JS thread DIRECTLY for 500 ms
	// With e.g. a for loop with 5m elements
}
setTimeout(printHello, 0);
fs.readFile('./file.json', useImportedtweets);
blockFor500ms();
console.log('Me first');
setImmediate(immediately);
```

![code work](img/asynchronous.jpg)
