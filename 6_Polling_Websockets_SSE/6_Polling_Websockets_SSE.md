# Polling (Long / Short) vs WebSockets vs SSE

```text
Client ------- HTTP Req --------> Server
       <------ Response
```

Basic Web Application with the Client-Server Model Flow will be:

- Client makes an HTTP Request requesting a webpage/resource from server
- Server calculates the response
- Server sends the response (JSON/XML) (REST/SOAP) to Client

> Ways of getting responses based on input requests

---

## Polling

Polling is a technique by which client asks the server for new data regularly.

### 1) Short Polling / AJAX Based Polling

- Makes request after a fixed delay (time/regular intervals)

AJAX: Asynchronous Javascript and XML

```text
Client -------- HTTP Req (Every 30 sec) --------> Server
       <------- Response
```

In Short Polling:

- Client makes a request to the server.
- Server can respond in 2 ways:
  - If a new data is Available: Sends response Immediately
  - If no new data is Available: Sends "Empty" Response
- As soon as Client Receives a response (Either Empty or Response), it will repeat the same process again

Diagram:

```text
req1 ------ resp1 ------ 30 sec ------- req2 ------- resp2 ------ 30 sec ------ req3 ------ resp3
```

resp-1/2/3: empty or actual response

Advantages:

- Message Ordering is Guaranteed.
  - If I first get the request, I will first get the response, then I can trigger the new request.
  - Until & unless I don't have the response from the first request, I will not trigger a new request.

Challenges:

- Making repeated requests to the server, wastage of resources as:
  - with each new request, HTTP headers must be passed
  - a query for data must be performed
- Connection must be closed and resources cleaned up
- Majority of responses are going to be null or empty from server end

Real Life Example:

1. Get the location of your Uber in every 10 seconds
2. GPS Navigation
3. Brokerage Firm to fetch Buy and Sell Orders: Stocks/Options

NVIDIA:

```text
fetchPrice(stock):
    10:14:15: response()
    10:14:45: response()
```

---

### 2) HTTP Long Polling

Long Polling works differently than Short Polling.

In Long Polling:

- Client makes a request to the server.
- Server can respond in 2 ways:
  - If a new data is Available: Sends response Immediately
  - If no new data is Available: Keep the connection open for a period of time and when it receives a new data, it will respond back with the updated data
- Every request will have a certain timeout.
- If the response is not received within the timeout, the connection would be closed.

Example: Connection Timeout Error

Real Life Examples:

```text
Server ----> DB

>> fetchPrice(stock, timestamp1): Result
>> fetchPrice(stock, timestamp2): No Result
```

NVIDIA:

```text
fetchPrice(stock):
    10:14:15: response()
    10:14:45: response()
```

1st Way (Short Polling):

- Ask every 30 sec the updated price of stock.

2nd Way (Long Polling):

- Rather than asking price every 30 sec,
- Keep the connection open with Server,
- When the price changes, Server informs the client.

Conclusion: Long Polling is better here.

### Challenges

Server:

- Req Timeout: 10 sec

```text
req-1: 10:00:00 -----> resp-1: 10:00:09
req-2: 10:00:05 -----> resp-2: 10:00:06
```

Chat Applications:

Sender:

- "Hey"
- "How are you"

Receiver:

- "How are you"
- "Hey"

Challenges:

- Message Ordering and delivery is NOT Guaranteed.
  - Message Ordering cannot be Guaranteed if the same client opens multiple connections to the Server
- Client was not able to receive the message, then it would be possible data loss

Short Polling:

```text
Client ---------> Server
   req-1
```

Long Polling:

```text
Client ------------------------------> Server
   req-1, req-2, req-3......
```

---

## WebSockets

HTTP: HyperText Transfer Protocol: 80
HTTPS: HyperText Transfer Protocol Secure = HTTP + TLS/SSL: 443

TCP: Transmission Control Protocol
IP: Internet Protocol

UDP: User Datagram Protocol

### TCP

TCP: Transmission Control Protocol - Reliable

- 3 Way Handshake

```text
          req (0)
client  ----------------------->   Server

          resp(1)
      <-----------------------

         Acknowledgement(2)
      ----------------------->

        confirms the Ack.(3)
      <-----------------------
```

TCP:

- Send (req) --------> Receive (resp) -------> Acknowledgement (sent by the Client after receiving the response from server)
- Stateful Connection
  - Connection established between Client and Server
  - Maintains a continuous communication channel until the communication is broken/terminated

### WebSocket Definition

Web Socket is a communication protocol that provides a full-duplex communication channel over a single TCP Connection.

#### Half-Duplex

```text
Client -----------> Server (HTTP)
          resp
```

#### Full-Duplex

```text
Client --------> Server (TCP)
        AND
Server --------> Client (TCP)
```

```text
Client: Packet-1: Received => ACK-1
Server Received ACK-1
Server will send Packet-2
Client: Packet-2: Received => ACK-2
```

WebSockets:

- Enable interaction between a client and a server with less overheads
- Provides real-time data transfer between client and server
- Keep the connection/channel open, allowing messages to be send to and from client and server
- Bi-Directional Active communication happens between client and server

Real Life Use Case:

1. Chat Application: WhatsApp, Messenger
2. Video Calls: Zoom, WhatsApp - TCP

Advantages:

- Websockets keeps a unique connection open while eliminating the latency problem in Long Polling.
- Long Polling is more resource-intensive because multiple connections are open and HTTP headers are set and the connection is closed.
- Websockets are very lightweight.
- Used in Real Time Applications like WhatsApp, Video Calls etc.
- Maintains the Message Delivery Order / Messaging Order

---

## Server Side Events (SSE)

- Under the SSE, the client will establish a persistent and long term connection with the server
- Server uses this connection to send data to client
- Client wants to send data to the server, it would require another protocol (Eg: TCP)

> If there is an update in Server, Server will send the updated data which is not requested by the client

Real Life Examples:

1. In Social Media when there is a new content/update, Server informs client and push it to the Client
   - Eg: LinkedIn, Instagram
2. Server Side Notifications / Push Notifications
   - Eg: Zomato

Flow:

1. Client requests the data from a server using HTTP
2. Requested webpage/resource opens a connection to the server
3. Server sends the data to the client whenever new event/information is Available

Example:

SSE are best where the server is generating data (feed) and multiple events (notifications) and send to client.

---

## How to Choose SSE vs WebSockets vs Polling?

To Ensure, Best Data Delivery : Web Sockets

| Option | Best Use Case | Key Characteristic | Notes |
|---|---|---|---|
| Short Polling | Simple periodic refresh needs | Client asks server at regular intervals | More frequent requests and empty responses |
| Long Polling | Real-time updates with fewer requests | Server holds the request open until new data arrives | Better than short polling, but multiple connections can be open |
| WebSockets | Chat, live dashboards, gaming, real-time messaging | Full-duplex, persistent connection | Best for bidirectional fast communication |
| SSE | Server pushes updates to client | One-way push from server to client | Good for feeds, notifications, live updates |

Applications for SSE:

- Real-Time Stock Market
- Real-Time Coverage of News/Links/Feeds/Tweets
- Live Dashboard (Scorecard)
- Live Dashboard for your System (Eg: Monitoring Systems and Alerts)

However,

- SSE is NOT a viable option for faster communication for client-server

Examples:

- MMO (Massive Multiplayer Online) Games that needs huge amount of messaging and connection from both client and server end
- PUBG, NFS

Websockets > SSE

> Use cases for SSE : it deals with server side updates only; no clients involved here means client can't be considered as producer here, rather clients will be considered as consumer.
>
> That's why for real time communication/messaging involved in game - follows websockets.

### Long Polling vs Short Polling

| Type | Behavior | Trade-off |
|---|---|---|
| Long Polling | Keeps connection open until data arrives | Feasibility to keep multiple connections open; no message delivery guarantee for non-ordering responses |
| Short Polling | Fetches the latest update at regular intervals | Need to fetch latest update in every few intervals; with message delivery |

#### TLS

TLS : Transport Layer Security
