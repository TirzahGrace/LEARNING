# Mail Client-Server System

## Setup

### Server Machine

1. Download the code zip file and extract it.

2. SERVER  {folder}
     - smtpmail.c
     - popserver.c
     - user.txt
     - for user{i} in user.txt: create a folder named with username { eg: user1, user2 } in the SERVER directory.
     - **Server Directory Structure:**
         ```
         SERVER
         ├── smtpmail.c
         ├── popserver.c
         ├── user.txt
         ├── user1
         ├── user2
         ├── user3
         └── ...
         ```     
3. Compile the SMTP server:

    ```bash
    cd path/to/server/folder
    gcc smtpmail.c -o smtp
    ```

    Run the SMTP server:

    ```bash
    ./smtp <smtp_port>
    ```

4. In another terminal, compile the POP3 server:

    ```bash
    cd path/to/server/folder
    gcc popserver.c -o pop3
    ```

    Run the POP3 server:

    ```bash
    ./pop3 <pop3_port>
    ```

5. Ensure that both SMTP and POP3 servers are running.

### Client Machine

1. Download the code zip file and extract it.

2. CLIENT  {folder}
   - mailclient.c

4. Compile the mail client:

    ```bash
    cd path/to/client/folder
    gcc mailclient.c -o client
    ```

    Run the client:

    ```bash
    ./client <server_IP> <smtp_port> <pop3_port>
    ```

5. The client will prompt for a username and password. Enter the login details.

6. The client will show three options:
   - 1. Manage Mail
   - 2. Send Mail
   - 3. Quit

## Usage

- **Manage Mail:**
    - Authentication check (password check) occurs here.
    - Lists received mails, allows viewing and deleting mails.
    - If no mails received, an error message is displayed.
    
- **Send Mail:**
    - Sends mail to another user.
    - Both the sender and receiver must exist in `user.txt`.
    - Both login user_name and from user_name should match.
    - No authentication check (password check) occurs here.

- **Quit:**
    - Exits the mail client.

## Notes

- Both "from" and "to" mail formats are `<user_name>@<domain_name>`.
- For sending mail, users must exist in `user.txt`.
- Authentication is not checked for sending mail.
- Authentication is checked for managing mails.
- Mails are listed with the following information:
    -  Sl. No. <Sender’s email id> <When received, in date : hour : minute> <Subject>
- **Handled Multiple Users:**
    - Multiple users can connect to the same mail server to access their mailboxes.
    - Multiple mail servers can send mail to the single mail server at the same  time.
    - Even if multiple users write to the same user at the same time, the system handles it seamlessly.

## Function Descriptions

### popserver.c

1. **`sendCommand(int Socket, const char *command)`**

   - **Parameters:**
      - `Socket`: The socket descriptor.
      - `command`: The command to be sent to the client.

   - **Description:**
      - Sends a command to the connected client using the specified socket.

2. **`receiveResponse(int Socket, char *buffer)`**

   - **Parameters:**
      - `Socket`: The socket descriptor.
      - `buffer`: A character array to store the received response.

   - **Description:**
      - Receives a response from the connected client using the specified socket and stores it in the provided buffer.

3. **`handleClient(int clientSocket, in_addr_t addr)`**

   - **Parameters:**
      - `clientSocket`: The socket descriptor for the connected client.
      - `addr`: The IP address of the connected client.

   - **Description:**
      - Handles the POP3 protocol communication with the client, managing user authentication, processing commands, and handling mailbox operations.
      - The function continuously listens for commands from the client and responds accordingly.
      - Manages user authentication using the provided username and password.
      - Handles the `LIST`, `RETR`, `DELE`, and `QUIT` commands.
      - Manages a lock file to ensure that only one process can access the mailbox at a time.
      - Performs mailbox operations such as listing mails, retrieving mails, and deleting mails based on client commands.
      - The function ensures proper error handling and responds to the client accordingly.

4. **`main(int argc, char *argv[])`**

   - **Parameters:**
      - `argc`: The number of command-line arguments.
      - `argv`: An array of command-line arguments.

   - **Description:**
      - The main function responsible for setting up the POP3 server.
      - Parses the command-line arguments to get the port number for the server.
      - Creates a socket, binds it to the specified port, and starts listening for incoming connections.
      - Accepts incoming client connections and forks a new process to handle each client.
      - Uses the `handleClient` function to manage the communication with each connected client.

   - **Note:**
      - The main function runs in an infinite loop, continuously accepting client connections and forking processes to handle them.

     This `popserver.c` implementation follows the POP3 protocol for a simple mail server, handling commands from the client and managing user mailboxes.
---
