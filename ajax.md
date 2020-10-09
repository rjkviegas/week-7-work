# AJAX

## Asynchronous JavaScript And XML

The use of ```XMLHttpRequest``` objects to communicate with servers. Sending and receiving information in various formats, including JSON, XML, HTML, and text files.

Asynchronous : communicating with the server, exchanging data and updating the page without refreshing

### Major Features of AJAX

- Makes requests to the server without reloading the page
- Receive and work with data from the server

### Making a HTTP Request

To make an HTTP request to the desired server (with JavaScript), you need an instance of an ```XMLHTTPRequest``` object:

```httpRequest = new XMLHttpRequest();```

### Preparing for the server response

After making the request, a response will be received. To handle the response, set the ```onreadystatechange``` property of the object and name it after the function to call when the request changes state
```httpRequest.onreadystatechange = nameOfTheFunction;```

1. The function needs to check the request's state to see if the state has the value of ```XMLHttpRequest.DONE``` (corresponding to 4), that means that the full server response was received and it's OK to proceed.
```if (httpRequest.readyState === XMLHttpRequest.DONE) {```

2. Check the HTTP response status codes of the HTTP response.
```if (httpRequest.status === 200) {```

3. Access the data with either:
- ```httpRequest.responseText``` – returns the server response as a string of text
- ```httpRequest.responseXML``` – returns the response as an ```XMLDocument``` object you can traverse with JavaScript DOM functions

### Making the request

Make the request, by calling the ```open()``` and ```send()``` methods of the HTTP request object:
```
httpRequest.open('GET', 'http://www.example.org/some.file', true);
httpRequest.send();
```
The parameter to the ```send()``` method can be any data you want to send to the server if ```POST```-ing the request. Form data should be sent in a format that the server can parse, like a query string:

```"name=value&anothername="+encodeURIComponent(myVar)+"&so=on"```
or other formats, like multipart/form-data, JSON, XML, and so on.

