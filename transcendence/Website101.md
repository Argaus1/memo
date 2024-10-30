# Website 101


## What is a website

Web pages located on a server. The server is linked to an IP adress.

1. DNS query. Request sent by the browser. Enter a **domain name** -> Domain Name System (DNS) translates it into the **IP address of your server**.

2. Connection to the server. IP is located by DNS server, the browser sends a http request to the server.

3. The server delivers HTML, CSS, Javascript and all resources necessary to make up the webpage.

Beware! Physical computer that hosts the website (= stores the resources) **is also alled a server**. It ensures that the web server can find the resource when a user enters a webaddress.

**Web pages** are documents. Written in HTML code, styled thanks to CSS files. Interactivity comes from JavaScript.:w
These documents are **loaded by the web server from the server, and displayed in the browser.**

4. Rendering web pages. Browser processes the HTML code to create the webpage structure, applies the CSS to style it, and execute the JavaScript files.


Dynamic vs static. A **dynamic website**'s content changes based on real time data or user interaction. **The server generates content**, often pulling data of a db.

## Difference between back end and front end

## How does a dynamic website work + SPA

Interacts with the user by dynamically rewriting the current webpage with new data from the server. No new page is loaded. Client renders the HTML from the new data retrieved. In other words, the frontend is located in the client side.

Traditional vs Single Page Web Applications

![Traditional](https://elmprogramming.com/images/chapter-7/7.2-what-is-a-single-page-app/server-side-rendering.svg)

![SinglePage](https://elmprogramming.com/images/chapter-7/7.2-what-is-a-single-page-app/client-side-rendering.svg)

With a SPA, the client is not just able to make request for new web pages. On its side is loaded the entire front end (CSS, java, html). It then only need to send and receive the pieces of data it needs to edit those pages (in JSON format typically).

![divs](https://elmprogramming.com/images/chapter-7/7.2-what-is-a-single-page-app/spa-shell.svg)

The application is divided into **divs**, the application can then decide which divs need to be refreshed.

## What is an API

Interface used to pull data from an external source. One system can communicate with another. This ensures that dynamic content is always up-to-date and relevant to the user.

## Basics of website architecture

## HTPP, HTTPS, user data and security
