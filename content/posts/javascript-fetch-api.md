+++
title = "Javascript Fetch API "
date = "2020-02-03"
draft = false
pinned = false
image = "img/fetchAPI.png"
description = "The Fetch API is a promise-based JavaScript API for making asynchronous HTTP requests in the browser similar to XMLHttpRequest.\n\nFetch API is pretty much standardized now and is supported by all modern browsers except IE. If you need to support all browsers including IE, just add a polyfill released by GitHub to your project."
+++
\### Basic API Usage

Using Fetch API is really simple. Just pass the URL, the path to the resource you want to fetch, to\`fetch()\`method:

\`\``js

fetch('/js/users.json')

.then(response=>{

// handle response data

})

.catch(err=>{

// handle errors

});

\`\``

We pass the path for the resource we want to retrieve as a parameter to\`fetch()\`. It returns a promise that passes the response to\`then()\`when it is fulfilled. The\`catch()\`method intercepts errors if the request fails to complete due to network failure or any other reason.



\### GET Request

By default, the Fetch API uses GET method for asynchronous requests. Let's use the jsonplaceholder API to retrieve a list of posts using GET request:



\`\``js

fetch('https://jsonplaceholder.typicode.com/posts')

.then(responese=>responese.json())

.then(data=>{

data.map(user=>{

document.write(`<h2>${user.id}: ${user.title}</h2> <h1> » ${user.body}</h1>`);

});

});

\`\``

Calling fetch() method returns a promise. The response returned by the promise is a stream object which means that when we call\`json()\`method, it returns another promise. Call to json() method indicates that we are expecting a JSON response. If you are expecting an XML response, you should use\`text()\`method.



\### POST Request

Just like Axios, Fetch also allows to use any other HTTP method in the request:\`POST, PUT, DELETE, HEAD and OPTIONS\`. All you need to do is set the method and body parameters in the fetch() options:

\`\``js

constuser={

firstName:'Adem',

lastName:'Hussein',

jobTitle:'Frontend Developer'

};



constoptions={

method:'POST',

body:JSON.stringify(user),

headers:{

'Content-Type':'application/json'

}

}



fetch('https://you can put backend url here/users',options)

.then(res=>res.json())

.then(data=>console.log(data));



\`\``

The backend url API sends us the body data back with an ID and created timestamp attached:

\`\``json

{

"firstName":"Adem",

"lastName":"Hussein",

"jobTitle":"Frontend Developer",

"id":"some id",

"createdAt":"some date time and timezone"

}

\`\``

\### DELETE Request

The DELETE request looks very similar to the POST request except body is not required:

\`\``js

constoptions={

method:'DELETE',

headers:{

'Content-Type':'application/json'

}

}



fetch('https://you can put backend url here/users/2',options)

.then(responese=>{

if(responese.ok) {

returnPromise.resolve('User deleted.');

}else{

returnPromise.reject('An error occurred.');

}

})

.then(data=>console.log(data));



\`\``

\### Error Handling



Since\`fetch()\`method returns a promise, error handling is easy. We can use the catch() method of the promise to intercept any error that is thrown during the execution of the request. However, no error will be thrown if the request hits the server and comes back, regardless of what response was returned by the server. The promise returned by the fetch() doesn't reject on HTTP errors even if the HTTP response code is 404 or 500.



Fortunately, you can use the ok property of response object to check whether the request was successful or not:

\`\``js

fetch('https://reqres.in/api/users/22')// 404 Error

.then(response=>{

if(response.ok) {

returnresponse.json();

}else{

returnPromise.reject(response.status);

}

})

.then(data=>console.log(data))

.catch(err=>console.log(\`Error with message: ${err}\`));

\`\``

\### Request Headers

Request headers (like Accept, Content-Type, User-Agent, Referer, etc.) are an essential part of any HTTP request. The Fetch API's Headers object allows us to\`set, remove, or retrieve\`HTTP request headers.



We can create a header object using the\`Headers()\`constructor and then use the\`append, has, get, set, and delete\`methods to modify request headers:

\`\``js

// create an empty \`Headers\` object

constheaders=newHeaders();



// add headers

headers.append('Content-Type','text/plain');

headers.append('Accept','application/json');



// add custom headers

headers.append('X-AT-Platform','Desktop');

headers.append('X-AT-Source','Google Search');



// check if header exists

headers.has('Accept');// true



// get headers

headers.get('Accept');// application/json

headers.get('X-AT-Source');// Google Search



// update header value

headers.set('Content-Type','application/json');



// remove headers

headers.delete('Content-Type');

headers.delete('X-AT-Platform');

We can also pass an arrayofarrays or an object literal to the constructor to create a headerobject:



// passing an object literal

constheaders=newHeaders({

'Content-Type':'application/json',

'Accept':'application/json'

});



// OR



// passing an array of arrays

constheaders=newHeaders([

\['Content-Type','application/json'],

\['Accept','application/json']

]);

\`\``

To add headers to the request, simply create a Request instance, and pass it to fetch() method instead of the URL:

\`\``js

constrequest=newRequest('https://some url please/api/users',{

headers:headers

});



fetch(request)

.then(respone=>response.json())

.then(json=>console.log(json))

.catch(err=>console.error('Error:',err));

RequestObject

The Request object represents a resource request,and can be created by calling theRequest()constructor:



constrequest=newRequest('https://some url please/users');

The Request object also accepts a URLobject:



consturl=newURL('https://some url please/api/users');

constrequest=newRequest(url);

\`\``

By passing a Request object to\`fetch()\`, you can easily customize the request properties:



\`method\`— HTTP method like GET, POST, PUT, DELETE, HEAD

\`url\`— The URL to the request, a string or a URL object

\`headers\`— a Headers object for request headers

\`referrer\`— referrer of the request (e.g., client)

\`mode\`— The mode for cross-origin requests (e.g., cors, no-cors, same-origin)

\`credentials\`— Should cookies and HTTP-Authorization headers go with the request? (e.g., include, omit, same-origin)

\`redirect\`— The redirect mode of the request (e.g., follow, error, manual)

\`integrity\`— The subresource integrity value of the request

\`cache\`— The cache mode of the request (e.g, default, reload, no-cache)

Let us create a Request object with some customized properties and body content to make a POST request:

\`\``js

constuser={

first_name:'Sherefudin',

last_name:'Adem',

job_title:'Web developer'

};



constheaders=newHeaders({

'Content-Type':'application/json',

'Accept':'application/json'

});



constrequest=newRequest('https://some url please/api/users',{

method:'POST',

headers:headers,

redirect:'follow',

mode:'cors',

body:JSON.stringify(user)

});



fetch(request)

.then(response=>response.json())

.then(json=>console.log(json))

.catch(err=>console.error('Error:',err));

\`\``

Only the first argument, the URL, is required. All these properties are read-only, which means that you can not change their value once the request object is created. The Fetch API does not strictly require a Request object. The object literal, we pass to\`fetch()\`method, acts like a Request object:

\`\``js

fetch('https://some url please/api/users',{

method:'POST',

headers:headers,

redirect:'follow',

mode:'cors',

body:JSON.stringify(user)

}).then(response=>response.json())

.then(json=>console.log(json))

.catch(err=>console.error('Error:',err));

\`\``

\### Response Object

The Response object returned by the fetch() method contains the information about the request and the response of the network request including headers, status code, and status message:

\`\``js

fetch('https://some url please/api/users')

.then(res=>{

// get response headers

console.log(res.headers.get('content-type'));

console.log(res.headers.get('expires'));



// HTTP response status code

console.log(res.status);



// shorthand for \`status\` between 200 and 299

console.log(res.ok);



// status message of the response e.g. \`OK\`

console.log(res.statusText);



// check if there was a redirect

console.log(res.redirected);



// get the response type (e.g., \`basic\`, \`cors\`)

console.log(res.type);



// the full path of the resource

console.log(res.url);

});

\`\``

The response body is accessible through the following methods:



\`json()\`returns the body as a JSON object

\`text()\`returns the body as s string

\`blob()\`returns the body as a Blob object

\`formData()\`returns the body as a FormData object

\`arrayBuffer()\`returns the body as an ArrayBuffer object

All these methods return a promise. Here is an example of\`text()\`method:



\`\``js

fetch('https://some url please/api/some address/2')

.then(response=>response.text())

.then(data=>console.log(data));

\`\``

The output of the above network call will be a JSON string:

\`\``json



'{"data":{

"id":"some id",

"name":"some name",

"year":2020,

"color":"some color",

"value":"some value"

}

}'

\`\``

\## Fetch & Cookies

By default, when you use Fetch to get a resource, the request does not contain credentials such as cookies. If you want to send cookies, you have to explicitly enable credentials like below:

\`\``js

fetch(url,{

credentials:'include'

})

\`\``

\## Fetch & Async/Await

Since Fetch is a promise-based API, we can go one step further and use the latest ES2017\`async/await\`syntax to make our code even simpler and synchronous-looking:



\`\``js

constfetchUsers=async()=>{

try{

constresponse=awaitfetch('https://some url please/api/users');

if(!response.ok) {

thrownewError(response.status);

}

constdata=awaitresponse.json();

console.log(data);

}catch(error) {

console.log(error);

}

}



fetchUsers();

\`\``

\### Conclusion

It is a huge improvement over XMLHttpRequest with a simple,elegant and easy-to-use interface. Fetch works great for fetching network resources (even across the network inside the service workers). The Fetch API is supported by all modern browsers, so there is no need to use any polyfill unless you want to support IE.



\`\``js

functiongetMoviesFromApiAsync() {

returnfetch('https://facebook.github.io/react-native/movies.json')

.then((response)=>response.json())

.then((responseJson)=>{

returnresponseJson.body;

})

.catch((error)=>{

console.error(error);

});

}



\`\``



You can also use the ES2017 async/await syntax in your project:



\`\``js

asyncfunctiongetMoviesFromApi() {

try{

letresponse=awaitfetch('https://jsonplaceholder.typicode.com/posts',);

letresponseJson=awaitresponse.json();

returnresponseJson.body;

}catch(error) {

console.error(error);

}

}



\`\``

You can also use the XMLHttpRequest API directly if you prefer.



\`\``js

varrequest=newXMLHttpRequest();

request.onreadystatechange=(e)=>{

if(request.readyState!==4) {

return;

}



if(request.status===200) {

console.log('success',request.responseText);

}else{

console.warn('error');

}

};



request.open('GET','https://some url here/');

request.send();



\`\``
