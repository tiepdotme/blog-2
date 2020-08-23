---
layout: post
title:  "What are Cookies? Document.cookie explained."
date:   2020-08-22 12:00:00 -0500
---

A cookie or HTTP cookies is a small amount of data that is stored by a website in your web browser when you visit said site. Cookies help websites to remember who you are or maintain state for the website (like saving region, language, preference selections, cart).

Cookies are also used for authentication. Websites can save secure cookies that are not directly readable by any script running in the browser. These are usually set by a web server and contains a non-sensitive encoded value (JWT token, session ID) to validate a user or set a time limit on a user session.

Last but not least, there is tracking cookies. These cookies build a long term profile about your browsing patterns, and they can be used to serve ads. There are potential security concerns about such practices.

### Reading Cookies

We can use the `Document` object in the browser to read cookies.

```javascript
const cookies = document.cookie;
cookies;
// dp1=bu1p/QEBfX0BAX19AQA**6304611c^pbf/%23e000e0000000000000000061232d9c^bl/US6304611c^;
// ebay=%5Esbf%3D%23100000%5E;
// nonsession=BAQAAAXIlFHjQAAaAADMABWEjLZw2MDEzOQDKACBjBGEcMTliOGU0NDYxNzQwYTE2ZDE0NGUzOTY3ZmZmZmVjYTAAywABX0IBJDWmn0jHK5v4TpcC5dR/rhnVGsB6nw**
```

I went to eBay in an incognito window and ran the above code to see the cookies that eBay sets. As you can see, there are three cookies set by default `dp1`, `ebay` and `nonsession` with various values.

### Writing Cookies

We can set a cookie using the syntax:

```javascript
document.cookie="key=value";
```

For example:

```javascript
document.cookie="region=en_US";
```

When setting a cookie, we can specify multiple options separated by a semicolon. These options are:

- `path` (whether root / or a specific path in the site where the cookie is set)
- `domain` (either the root domain or a particular subdomain)
- `max-age` (how long a cookie is valid for. This is passed in seconds)
- `expires` (date in GMT)
- `secure` (to set a secure cookie that could only be transmitted over HTTPS)
- `samesite` (disables sending the cookie in a cross-site request)

### Reading a Specific Cookie

In the example above we set a cookie `region` and set its value to `en_US`. We can read all cookies using `document.cookie` but to read a specific cookie, we need to write a bit of code.

```javascript
document.cookie="region=en_US";

const region = document.cookie
    .split('; ') // split cookies into an array
    .find(row => row.startsWith("region")) // find the array element with cookie we want
    .split('=')[1]; // get the value of the cookie

region; // en_US
```

I usually prefer to save this as a utility method to read cookies multiple time. Folks also use small libraries to do it. One such common library is [js-cookie](https://github.com/js-cookie/js-cookie).

### Disadvantages

- Cannot save sensitive information. No one is stopping you from doing it, but it creates a massive security issue.
- Most browsers implement a size limitation of 4096 bytes.
- Browsers also impose a limit on the number of cookies per domain
- Clients could have cookies disabled so you can't always be dependent on them.
- Cookies only work with strings so storing any other data type is tricky.


Even though there are disadvantages, there are a ton of options and customization you can do with cookies. Go exploring!

Happy coding 👋🏼

