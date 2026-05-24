# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm
1 Start the program.

2.Create socket in server and client.

3.Bind server with host and port number.

4.Server waits for client connection.

5.Client connects to the server.

6. Client selects:

Download (GET)
Upload (POST)
7.If GET request:

Server reads index.html
Sends HTML content to client
8.If POST request:

Client sends data
Server stores data in upload.txt
9.Display response message.

10.Close client and server connection.

11.Stop the program.

## Program 
# client
```
import socket

client = socket.socket()

client.connect(("localhost", 3024))

print("===== MENU =====")
print("1. Download File")
print("2. Upload Data")
print("3. Exit")

choice = input("Enter choice: ")

# DOWNLOAD
if choice == "1":

    request = "GET / HTTP/1.1\nHost: localhost\n\n"

    client.send(request.encode())

    response = client.recv(4096).decode()

    print("\nServer Response:\n")
    print(response)

# UPLOAD
elif choice == "2":

    msg = input("Enter data to upload: ")

    request = "POST / HTTP/1.1\nHost: localhost\n\n" + msg

    client.send(request.encode())

    response = client.recv(1024).decode()

    print("\nServer Response:\n")
    print(response)

# EXIT
elif choice == "3":

    print("Client Closed")

else:

    print("Invalid Choice")

client.close()
```

# server
```
import socket

server = socket.socket()

server.bind(("localhost", 3024))

server.listen(5)

print("Server Running on Port 3024...")

while True:

    client, addr = server.accept()

    print("Connected with", addr)

    try:

        request = client.recv(4096).decode()

        print("Request Received")

        # DOWNLOAD
        if request.startswith("GET"):

            try:
                file = open("index.html", "r")

                data = file.read()

                response = "HTTP/1.1 200 OK\n\n" + data

                client.send(response.encode())

                file.close()

                print("HTML File Sent")

            except:
                client.send("HTTP/1.1 404 NOT FOUND\n\nFile Not Found".encode())

        # UPLOAD
        elif request.startswith("POST"):

            msg = request.split("\n\n")[1]

            file = open("upload.txt", "w")

            file.write(msg)

            file.close()

            response = "HTTP/1.1 200 OK\n\nData Uploaded Successfully"

            client.send(response.encode())

            print("Data Uploaded")

        else:

            client.send("HTTP/1.1 400 BAD REQUEST\n\nInvalid Request".encode())

    except Exception as e:

        print("Error:", e)

    client.close()
```
## OUTPUT
<img width="1363" height="767" alt="image" src="https://github.com/user-attachments/assets/c0c00447-f8a0-4a12-bb95-9c6c7ed84b9c" />
<img width="1364" height="766" alt="image" src="https://github.com/user-attachments/assets/fa5eeac0-d137-48f8-a6b5-db8f318f2249" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
