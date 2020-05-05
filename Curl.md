* [Perform an HTTP GET request](#perform-an-http-get-request)
* [Get the HTTP response headers](#get-the-http-response-headers)
* [Only get the HTTP response headers](#only-get-the-http-response-headers)
* [Perform an HTTP POST request](#perform-an-http-post-request)
* [Perform an HTTP POST request sending JSON](#perform-an-http-post-request-sending-json)
* [Perform an HTTP PUT request](#perform-an-http-put-request)
* [Follow a redirect](#follow-a-redirect)
* [Store the response to a file](#store-the-response-to-a-file)
* [Using HTTP authentication](#using-http-authentication)
* [Set a different User Agent](#set-a-different-user-agent)
* [Inspecting all the details of the request and the response](#inspecting-all-the-details-of-the-request-and-the-response)
* [Copying any browser network request to a curl command](#copying-any-browser-network-request-to-a-curl-command)

Refs
* <https://flaviocopes.com/http-curl/>

# Perform an HTTP GET request
When you perform a request, curl will return the body of the response:
```sh
curl https://flaviocopes.com/
```
# Get the HTTP response headers
By default the response headers are hidden in the output of curl. To show them, use the `i` option:
```sh
curl -i https://flaviocopes.com/
```
# Only get the HTTP response headers
Using the `I` option, you can get only the headers, and not the response body:
```sh
curl -I https://flaviocopes.com/
```
# Perform an HTTP POST request
The `X` option lets you change the `HTTP` method used. By default, `GET` is used, and it’s the same as writing
```sh
curl -X GET https://flaviocopes.com/
```
Using `-X POST` will perform a `POST` request. You can perform a POST request passing data URL encoded:
```sh
curl -d "option=value&something=anothervalue" -X POST https://flaviocopes.com/
```
In this case, the `application/x-www-form-urlencoded Content-Type` is sent.

# Perform an HTTP POST request sending JSON
Instead of posting data URL-encoded, like in the example above, you might want to send JSON. In this case you need to explicitly set the Content-Type header, by using the `H` option:
```sh
curl -d '{"option": "value", "something": "anothervalue"}' -H "Content-Type: application/json" -X POST https://flaviocopes.com/
```
You can also send a JSON file from your disk:
```sh
curl -d "@my-file.json" -X POST https://flaviocopes.com/
```
# Perform an HTTP PUT request
The concept is the same as for `POST` requests, just change the `HTTP method` using `-X PUT`

# Follow a redirect
A redirect response like 301, which specifies the Location response header, can be automatically followed by specifying the `L` option:
```sh
curl http://flaviocopes.com/
```
will not follow automatically to the HTTPS version which I set up to redirect to, but this will:
```sh
curl -L http://flaviocopes.com/
```
# Store the response to a file
Using the `o` option you can tell curl to save the response to a file:
```sh
curl -o file.html https://flaviocopes.com/
```
You can also just save a file by its name on the server, using the O option:
```sh
curl -O https://flaviocopes.com/index.html
```
# Using HTTP authentication
If a resource requires Basic HTTP Authentication, you can use the `u` option to pass the `user:password values`:
```sh
curl -u user:pass https://flaviocopes.com/
```
# Set a different User Agent
The user agent tells the server which client is performing the request. By default curl sends the `curl/<version> user agent, like: curl/7.54.0.`

You can specify a different user agent using the `--user-agent` option:
```sh
curl --user-agent "my-user-agent" https://flaviocopes.com
```
# Inspecting all the details of the request and the response
Use the `--verbose` option to make curl output all the details of the request, and the response:
```sh
curl --verbose -I https://flaviocopes.com/
*   Trying 178.128.202.129...
* TCP_NODELAY set
* Connected to flaviocopes.com (178.128.202.129) port 443 (#0)
* TLS 1.2 connection using TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* Server certificate: flaviocopes.com
* Server certificate: Let's Encrypt Authority X3
* Server certificate: DST Root CA X3
> HEAD / HTTP/1.1
> Host: flaviocopes.com
> User-Agent: curl/7.54.0
> Accept: */*
>
< HTTP/1.1 200 OK
HTTP/1.1 200 OK
< Cache-Control: public, max-age=0, must-revalidate
Cache-Control: public, max-age=0, must-revalidate
< Content-Type: text/html; charset=UTF-8
Content-Type: text/html; charset=UTF-8
< Date: Mon, 30 Jul 2018 08:08:41 GMT
Date: Mon, 30 Jul 2018 08:08:41 GMT
```




## HTTP Login
```sh
# HTTP Basic Authentication
curl -u bob:qwerty http://192.165.34.3/dir/

# HTTP Digest Authentication
curl --digest -u alice:password1 http://192.165.34.3/poc/

# HTTP Token Authentication
# -H, --header <header/@file> Pass custom header(s) to server
curl -H 'Authorization: Token 123123123' 192.186.248.3
```
## Fetch Headers
```sh
# curl -I to fetch headers and read the protection type from those headers
curl -I http://192.165.34.3/dir
```
