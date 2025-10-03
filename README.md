# Building-A-Simple-TCP-Port-Scanner-In-C

# Building A Simple TCP Port Scanner in C

## Overview

This project demonstrates how to build a basic TCP port scanner in C. Port scanning is a technique used to determine which ports on a target host are open. This knowledge is essential for network administration and security.

## Features

- Scans ports 1–1024 (well-known ports) on the target (default: localhost)
- Reports open ports to the console
- Easy to extend for remote targets and custom port ranges

## How It Works

- Uses TCP sockets to attempt connections to each port.
- If a connection succeeds, the port is open.
- Each port is scanned sequentially.

## Getting Started

### Prerequisites

- GCC or any C compiler
- Unix-like OS (Linux, macOS)

### Build

```sh
gcc -o port_scanner port_scanner.c
```

### Run

```sh
./port_scanner
```

## Code Explanation

```c
#include <stdio.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>

int main() {
    int sockfd;
    struct sockaddr_in serv_addr;

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_addr.s_addr = inet_addr("127.0.0.1"); // Scan localhost

    for (int port = 1; port <= 1024; port++) {
        sockfd = socket(AF_INET, SOCK_STREAM, 0);
        if (sockfd < 0) {
            perror("Socket creation failed");
            continue;
        }
        serv_addr.sin_port = htons(port);

        if (connect(sockfd, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) == 0) {
            printf("Port %d is open\n", port);
        }
        close(sockfd);
    }
    return 0;
}
```

1. **Set up the target address** for scanning (`127.0.0.1`).
2. **Iterate over the desired port range**.
3. **Create a new socket** for each port.
4. **Attempt to connect** to each port.
5. **Print open ports**.
6. **Close the socket** to avoid resource leaks.

## Tips and Tricks

- Use multithreading for faster scanning.
- Set timeouts to avoid hanging on closed ports.
- Accept IP/port range as input for flexibility.

## Extensions

- Scan remote hosts by changing the IP address.
- Scan custom port ranges.
- Output results to a file.

## Resources

- [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/)
- [Nmap Documentation](https://nmap.org/book/)
- [Linux Socket Programming](https://www.geeksforgeeks.org/socket-programming-cc/)

## Recommended Repositories

- [nmap/nmap](https://github.com/nmap/nmap)
- [robertdavidgraham/masscan](https://github.com/robertdavidgraham/masscan)
- [secdev/scapy](https://github.com/secdev/scapy)

## Learning Path

- Learn C and systems programming basics.
- Study networking fundamentals.
- Explore advanced topics like multithreading and asynchronous I/O.

---

**Happy Scanning!**
