# HTTP Analysis - Findings

## Objective

Analyze HTTP request and response packets and observe how a webpage is transferred in plain text.

---

## Traffic Generated

Started a local HTTP server:

```bash
python3 -m http.server 8000
```

Accessed:

```text
http://127.0.0.1:8000/index.html
```

Display Filter:

```text
http
```

---

## Packet 1 - HTTP GET Request

Source IP:
127.0.0.1

Destination IP:
127.0.0.1

Protocol:
HTTP

Method:
GET

Requested Resource:
/index.html

HTTP Version:
HTTP/1.1

Purpose:

The client requests the **index.html** file from the web server.

---

## Packet 2 - HTTP Response

Source IP:
127.0.0.1

Destination IP:
127.0.0.1

Protocol:
HTTP

Status Code:
200 OK

HTTP Version:
HTTP/1.0

Content-Type:
text/html

Content-Length:
57 Bytes

Server:
SimpleHTTP/0.6 Python/3.13.12

Purpose:

The web server successfully returns the requested HTML file.

---

## HTTP Communication Flow

Client
    │
    │ GET /index.html
    ▼
Python HTTP Server
    │
    │ HTTP/1.0 200 OK
    │ HTML File
    ▼
Client

---

## HTTP Payload

The HTML page is transferred in plain text.

```html
<h1>Hello Wireshark Tue Jul 21 12:54:48 PM EDT 2026</h1>
```

This can be viewed by selecting the HTTP response packet and choosing:

```
Follow
    ↓
TCP Stream
```

---

## Observations

- HTTP uses TCP as its transport protocol.
- HTTP communicates over port **80** by default.
- The browser sends an HTTP GET request.
- The server replies with **HTTP 200 OK**.
- HTTP headers and webpage content are transmitted in plain text.
- Wireshark can reconstruct the complete HTTP conversation using **Follow TCP Stream**.

---

## Browser Cache Note

If the requested file is already stored in the browser cache, the server may respond with:

```
HTTP/1.0 304 Not Modified
```

instead of:

```
HTTP/1.0 200 OK
```

In this case, the HTML content is **not sent again**, because the browser already has a local copy. As a result, the packet will contain only HTTP headers and **no webpage data**.

To capture the HTML content again, you can:

- Use `curl` instead of a browser.
- Disable the browser cache.
- Modify the file so the server sends a fresh copy.

---

## What I Learned

- HTTP is a request-response protocol.
- HTTP transfers data in plain text without encryption.
- The webpage content can be viewed directly in Wireshark.
- HTTP headers provide information such as the request method, server software, content type, and content length.
- Browser caching can change the server response from **200 OK** to **304 Not Modified**, preventing the HTML data from appearing in the capture.
