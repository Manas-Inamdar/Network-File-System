# Network File System (NFS)

This project implements a custom **Network File System (NFS)** with a modular architecture involving Clients, a Naming Server, and multiple Storage Servers. It enables distributed file access and management across multiple machines with features such as file creation, deletion, reading, writing, audio streaming, and replication.

## Architecture

### 1. Naming Server (NM)
The Naming Server acts as the central coordination point. It:
- Maintains metadata about file locations.
- Stores information about all registered Storage Servers (IP, ports, accessible paths).
- Responds to client requests by providing the correct Storage Server information.
- Handles logging, feedback, error reporting, and failure detection.

### 2. Storage Servers (SS)
Each Storage Server:
- Registers itself with the Naming Server at runtime.
- Provides its IP address, client port, NM port, and list of accessible paths.
- Supports operations such as file/folder creation, deletion, read/write, audio streaming, and file copying.
- Can be added dynamically during runtime.
- Supports asynchronous and synchronous writes.
- Replicates data to two other servers for fault tolerance.

### 3. Clients
Clients interact with the Naming Server to perform file operations. Supported functionalities include:
- Reading and writing files.
- Creating and deleting files/folders.
- Copying files between servers.
- Streaming audio files.
- Listing all accessible paths.
- Retrieving file metadata.

## Features

### Initialization
- Naming Server starts first and awaits registration from Storage Servers.
- Storage Servers send registration details dynamically (not hardcoded).
- Naming Server starts accepting client requests only after all initial SS are registered.

### File Operations
- Full support for read/write (both sync and async), create/delete, copy, stream.
- Metadata retrieval (size, permissions, etc.)

### Asynchronous Writes
- Writes large files asynchronously with immediate ACK to clients.
- Final confirmation is sent after data is flushed to disk.
- Failures during async writes are reported to clients via NM.

### Multi-client Support
- Supports multiple clients concurrently.
- Only one client can write to a file at a time.
- Multiple clients can read the same file simultaneously.

### Error Handling
- Uses distinct error codes to describe issues like file not found or file under modification.

### Search Optimization
- Efficient file lookup using Trie or HashMap.
- LRU caching to improve repeated query performance.

### Replication and Fault Tolerance
- Files/folders are asynchronously replicated to two backup servers.
- On SS failure, NM serves reads from backups.
- On SS recovery, previously replicated files are matched without creating duplicates.

### Bookkeeping
- Logs every request, acknowledgment, IP address, and port.
- Displays messages indicating operation status and results.

