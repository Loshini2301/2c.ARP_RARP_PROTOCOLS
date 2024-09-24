# 2c.SIMULATING ARP /RARP PROTOCOLS.

### Developed by: Loshini.G
### Register no: 212223220051

## AIM:
To write a python program for simulating ARP protocols using TCP.

## ALGORITHM:

### Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
   
### Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.

## PROGRAM:

### Server:
```py
import socket
import os

def get_mac_address(ip_address):
    # Execute the arp command to get the ARP table entry for the given IP address
    try:
        arp_output = os.popen(f"arp -a {ip_address}").read()
        # Find the MAC address from the output (typically next to the IP address)
        for line in arp_output.splitlines():
            if ip_address in line:
                mac_address = line.split()[1]  # Assuming MAC address is second in output
                return mac_address
    except:
        return None
    return None

def server_program():
    # Create a socket to listen for connections
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Bind the socket to localhost on port 12345
    server_socket.bind(('localhost', 12345))
    server_socket.listen(1)
    
    print("Server is listening for connections...")
    
    while True:
        # Accept a connection
        client_socket, client_address = server_socket.accept()
        print(f"Connection established with {client_address}")
        
        try:
            # Receive the IP address from the client
            ip_address = client_socket.recv(1024).decode()
            print(f"Received IP address: {ip_address}")
            
            # Get the MAC address corresponding to the IP address
            mac_address = get_mac_address(ip_address)
            
            # Send the MAC address back to the client
            if mac_address:
                client_socket.send(mac_address.encode())
            else:
                client_socket.send(b'')
        finally:
            # Close the connection
            client_socket.close()

if __name__ == "__main__":
    server_program()
```

### Client:
```py
import socket

def client_program():
    # Create a socket to connect to the server
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # Connect to the server at localhost on port 12345
    server_address = ('localhost', 12345)
    client_socket.connect(server_address)
    
    # Get the IP address to be converted to MAC address
    ip_address = input("Enter the IP address to be converted into MAC address: ")
    
    try:
        # Send the IP address to the server
        client_socket.send(ip_address.encode())
        
        # Receive the MAC address from the server
        mac_address = client_socket.recv(1024).decode()
        if mac_address:
            print(f"MAC Address for IP {ip_address} is: {mac_address}")
        else:
            print(f"MAC Address for IP {ip_address} could not be found.")
    
    finally:
        # Close the connection
        client_socket.close()

if __name__ == "__main__":
    client_program()
```

## OUTPUT:

### Server:
![Screenshot 2024-09-24 193939](https://github.com/user-attachments/assets/d05a920a-26c4-4ee1-a717-7e6bef2fba96)

### Client:
![Screenshot 2024-09-24 193917](https://github.com/user-attachments/assets/7bd671ad-c8cf-4998-828b-142a9ba1862b)

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed..
