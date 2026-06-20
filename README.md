# Offline Payment Mesh

A Spring Boot simulation of an offline UPI-style payment network where encrypted payment packets travel through a Bluetooth mesh of nearby devices until a connected bridge node uploads them to the backend for settlement.

## Overview

Traditional UPI payments require an active internet connection. This project explores how a payment instruction could be securely transported through a mesh network when the sender has no connectivity.

The system simulates:

* Offline payment creation
* End-to-end encrypted payment packets
* Bluetooth mesh-style packet propagation
* Bridge nodes with internet connectivity
* Replay protection
* Idempotent transaction processing
* Transaction settlement and ledger updates

---

## Architecture

```text
Sender Device
     │
     ▼
Create Payment Instruction
     │
     ▼
Hybrid Encryption
(RSA-OAEP + AES-256-GCM)
     │
     ▼
Mesh Packet
     │
     ▼
Bluetooth Mesh Simulation
(Device → Device Gossip)
     │
     ▼
Bridge Node
(Internet Available)
     │
     ▼
Backend Ingestion Service
     │
     ├── Idempotency Check
     ├── Decryption
     ├── Freshness Validation
     └── Settlement
     │
     ▼
Transaction Ledger
```

---

## Key Features

### Hybrid Encryption

Payment instructions are encrypted using:

* RSA-2048 OAEP
* AES-256 GCM

The server publishes a public key and only the backend can decrypt incoming payment packets.

### Mesh Network Simulation

Packets are propagated through a network of virtual devices using a gossip protocol.

Features:

* Device-to-device forwarding
* Packet deduplication
* Time-to-live (TTL)
* Bridge node delivery

### Idempotent Processing

Duplicate packet deliveries are prevented using:

* SHA-256 packet hashing
* Atomic packet claims
* ConcurrentHashMap-based idempotency cache

This ensures that even if multiple bridge nodes upload the same packet simultaneously, only one transaction is settled.

### Replay Protection

The system validates:

* Packet age
* Freshness window
* Future-dated packets

to prevent replay attacks.

### Transactional Settlement

Settlement uses:

* Spring Transactions
* JPA
* Optimistic Locking

to guarantee safe balance updates.

---

## Technology Stack

### Backend

* Java 17
* Spring Boot 3.3.5
* Spring Data JPA
* Thymeleaf

### Database

* H2 In-Memory Database

### Security

* RSA-2048 OAEP
* AES-256 GCM
* SHA-256

### Build Tool

* Maven

---

## Core Components

### BridgeIngestionService

Responsible for:

* Packet validation
* Idempotency checks
* Decryption
* Replay protection
* Settlement orchestration

### SettlementService

Responsible for:

* Account debits
* Account credits
* Transaction persistence

### HybridCryptoService

Implements:

* AES encryption
* RSA key exchange
* Hybrid encryption workflow

### MeshSimulatorService

Simulates:

* Offline devices
* Packet gossip
* Bridge uploads
* Network propagation

---

## Dashboard

The project includes a Thymeleaf dashboard that allows users to:

* Create payment packets
* Inject packets into the mesh
* Run gossip rounds
* Upload packets through bridge nodes
* View account balances
* View transaction history
* Observe idempotent processing

---

## Running Locally

Clone the repository:

```bash
git clone https://github.com/shivamjaveri0010/offline-payment-mesh.git
cd offline-payment-mesh
```

Run the application:

```bash
./mvnw spring-boot:run
```

Windows:

```bash
mvnw.cmd spring-boot:run
```

Open:

```text
http://localhost:8080
```

---

## Sample Demo Flow

1. Create a payment from Alice to Bob.
2. Encrypt the payment instruction.
3. Inject the packet into the mesh.
4. Execute gossip rounds.
5. Allow a bridge node to upload packets.
6. Backend performs:

    * Idempotency validation
    * Decryption
    * Replay protection
    * Settlement
7. Observe updated balances and ledger entries.

---

## Learning Outcomes

This project demonstrates concepts commonly found in distributed payment systems:

* Distributed Systems
* Hybrid Cryptography
* Exactly-Once Processing
* Idempotency
* Replay Protection
* Optimistic Locking
* Transaction Management
* Spring Boot Architecture

---

## Future Improvements

* Real Bluetooth Low Energy communication
* Redis-backed distributed idempotency
* PostgreSQL persistence
* JWT Authentication
* Digital Signatures
* Device Certificates
* Docker Deployment
* Kubernetes Support

---

## Author

Shivam Javeri

GitHub:
https://github.com/shivamjaveri0010
