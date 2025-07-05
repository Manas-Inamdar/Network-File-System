# Network File System (NFS)

This project delivers a custom **Network File System (NFS)** featuring a modular design with Clients, a Naming Server, and multiple Storage Servers. It enables distributed file management and access across several machines, supporting operations like file creation, deletion, reading, writing, audio streaming, and data replication.

## Architecture

### 1. Naming Server (NM)
The Naming Server serves as the central coordinator. Its responsibilities include:
- Managing metadata about file locations.
- Keeping records of all registered Storage Servers (including IPs, ports, and accessible paths).
- Answering client queries by supplying the appropriate Storage Server details.
- Handling logging, feedback, error messages, and monitoring failures.

### 2. Storage Servers (SS)
Each Storage Server:
- Registers itself with the Naming Server at startup.
- Shares its IP address, client port, NM port, and accessible paths.
- Handles file and folder creation, deletion, reading, writing, audio streaming, and copying.
- Can be added to the system dynamically at runtime.
- Supports both asynchronous and synchronous write operations.
- Replicates data to two additional servers for increased fault tolerance.

### 3. Clients
Clients communicate with the Naming Server to execute file operations. Supported actions include:
- Reading and writing files.
- Creating and deleting files or directories.
- Copying files between servers.
- Streaming audio files.
- Listing all available paths.
- Fetching file metadata.

## Features

### Initialization
- The Naming Server is started first and waits for Storage Servers to register.
- Storage Servers register dynamically (no hardcoded configuration).
- The Naming Server begins handling client requests only after all initial Storage Servers are registered.

### File Operations
- Complete support for reading, writing (sync and async), creating, deleting, copying, and streaming files.
- Metadata retrieval (such as size and permissions).

### Asynchronous Writes
- Large file writes are handled asynchronously, providing immediate acknowledgment to clients.
- Final confirmation is sent once data is safely written to disk.
- Any failures during async writes are reported to clients via the Naming Server.

### Multi-client Support
- Allows multiple clients to connect and operate concurrently.
- Ensures exclusive write access to a file, while permitting multiple simultaneous reads.

### Error Handling
- Utilizes specific error codes to indicate problems like missing files or files currently being modified.

### Search Optimization
- Fast file lookup using a Trie or HashMap.
- LRU cache boosts performance for repeated queries.

### Replication and Fault Tolerance
- Files and directories are asynchronously replicated to two backup servers.
- If a Storage Server fails, the Naming Server redirects reads to backup servers.
- Upon recovery, previously replicated files are reconciled to avoid duplication.

### Bookkeeping
- Logs every request, acknowledgment, IP, and port.
- Displays messages to indicate the status and outcome of operations.

