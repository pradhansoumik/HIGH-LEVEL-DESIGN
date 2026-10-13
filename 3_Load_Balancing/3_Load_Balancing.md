

# Load Balancing

- Helps to spread the traffic (incoming requests) across a cluster of servers to improve responsiveness and availability of a system.
- LB also keeps track of the status of all resources (servers) while distributing requests.
- If a Server is Not Available, LB will stop sending requests/traffic to that server.

## Example

```text
100K
Client -------- LB -------- Web Server-1
                            Web Server-2
```

Total Request: 100K

LB:

- Web Server-1: 50K
- Web Server-2: 50K

> LB keeps track of which request is going to which server and the current status of that server.

### HashMap Example

- req-1: WS-1: Good
- req-2: WS-2: Bad
- req-3: WS-1
- req-4: WS-1: Because WS-2 is down

### Real Life Examples

- Nginx Plus
- AWS ELB (Elastic Load Balancer)
- Citrix ADC (Prev: NetApp)
- Google Cloud Load Balancer

---

## Where to Put Load Balancers?

To utilize full Scalability and Redundancy, We can try to balance each layer of the system.

Ideally, we can add LB at 3 places:

1. Between User (Client) and Web Server
2. Between Web Server and an Internal Platform Layer (Application Server or Cache Server)
3. Between Internal Layer Platform and Database

```text
Client -------- LB --------> Web Server ------- LB --------> Application/Cache Server (Internal Layer) --------- LB --------> DB --------> Downstream Services (3rd party)
```

## Downstream Service Example

```text
System-1 ------> System-2
```

### Banking Example

Submit/Withdraw a Cheque:

Chase Bank

```text
Client ----> Chase Bank
```

- Chase Bank: Check the funds, Customer Details, Account Status --- ACTIVE or NOT
- Validations on Cheque: Cheque is genuine or not --- MICR Code etc, Funds Request, dates etc
- ------> 3rd Party Service: DCS - Deposit Cheque Service

```text
req: submit_cheque
Client ----------------------> Chase bank ----------------->  Deposit Cheque Service
       <---------------------             <----------------   (Downstream Services)
```

Downstream Services: I am passing the data/requests/responses from my own organization (Chase Bank) to them (3rd party).

- Deposit Cheque Service: Downstream for Chase Bank
- Chase Bank: Upstream for Deposit Cheque Service

---

## Advantages / Benefits of Load Balancing

1. Users experience faster, uninterrupted services. Requests are moved to a more readily available server, hence no wait time.

2. Less Downtime and Higher Throughput (Lower Latency): Even a Full Server Failure will not affect the end user experience, as LB will route to a healthy server.

```text
100K
Client -------- LB -------- Web Server-1
                            Web Server-2
```

LB:

- req-1 ------> WS-1: Failed
- All New Req along with req-1:
- req-2 ------> WS-2

3. Keeping Track of heartbeat of All Servers

4. LB can also cache some data for faster retrieval

5. Smart Predictive Load Balancing:

   Predictive Analysis:

   - Determine the Traffic Bottlenecks before they happen

   As a result, Smart Load Balancers will give actionable insights:

   - Automation and drive Business Decisions

### Example

E-Commerce:

- Black Friday Sale
- Scale: 100x

---

## Load Balancing Algorithms

### 1) Least Connection Method

- Incoming traffic routes to servers with the fewest active connections.

### 2) Least Response Time Method

- Incoming traffic routes to servers with the least response time.

### 3) Round Robin Method

- Iterate through all list of servers and sends each new request to the next available server.
- At the end, start iterating from beginning.

```text
10 Servers

req-1: 1st
..........
..........
..........
req-9: 9th
req-10: 10th
..........
req-11: 1st
```

### 4) IP Hashing

- Hash of IP Address is redirected to the Server.

IP: 102.204.55.98
N: Number of Servers

98 % N : Server_ID

**Example:**

- 98 % 5 = 3rd Server

### 5) Least Bandwidth Method

- Incoming traffic routes to server with the least Bandwidth (Mbps)


