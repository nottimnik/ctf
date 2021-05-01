# Web Fundamentals [ [link](https://tryhackme.com/room/webfundamentals) ]

## Task 1 - Introduction and objectives

No Answers Needed.

## Task 2 - How do we load websites?

### What request verb is used to retrieve page content?

To get the content of a website you use a HTTP GET request, the server will respond to the GET request with the web page content.

The answer is `GET`.

### What port do web servers normally listen on?

Most of the web servers normally listen on the port `80`.

### What's responsible for making websites look fancy?

CSS is used to style html and make the websites look fancy.

The answer is `CSS`.

## Task 3 - More HTTP - Verbs and request formats

### What verb would be used for a login?

`POST` request is used to send data (login credentials) to the web server 

### What verb would be used to see your bank balance once you're logged in?

`GET` - requests the data (balance) from the web server (the bank).

### Does the body of a GET request matter? Yea/Nay

The body of a `GET` request doesn't matter.

The answer is `Nay`.

### What's the status code for "I'm a teapot?"

I found the status code [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses).

The answer is `418`.

### What status code will you get if you need to authenticate to access some content, and you're unauthenticated?

The status code that you will get when you are unauthenticated is `401`, meaning `Unauthorized`.

## Task 4 - Cookies, tasty!

No Answers Needed.

## Task 5 - Mini CTF

Navigated to http://10.10.149.108:8081.

### What's the GET flag?

We need to do a `GET` request to the web server with the path `/ctf/post`

`curl http://10.10.149.108:8081/ctf/get`

And we get the flag:
`thm{162520bec925bd7979e9ae65a725f99f}`

### What's the POST flag?

We need to make a POST request with the body "flag_please" to the web server with the path `/ctf/post`

`curl -X POST http://10.10.149.108:8081/ctf/post --data "flag_please"`

And we get the flag:
`thm{3517c902e22def9c6e09b99a9040ba09}`

### What's the "Get a cookie" flag?

We need to navigate to `http://10.10.149.108:8081/ctf/getcookie` in a browser. We inspect the page and check the storage for cookies.

The only cookie is this and it contains the flag:
```
flag:"thm{91b1ac2606f36b935f465558213d7ebd}"
```

### What's the "Set a cookie" flag?

Navigate to `http://10.10.149.108:8081/ctf/sendcookie` in a web browser. We need change the name of the cookie with the name `flag` to `flagpls` and the value to `flagpls`, then refresh the page. Then the page will show the flag:

`thm{c10b5cb7546f359d19c747db2d0f47b3}`
