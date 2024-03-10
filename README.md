# Mail Client-Server System

## Setup

### Server Machine

1. Download the code zip file and extract it.

2. SERVER  {folder}
     - smtpmail.c
     - popserver.c
     - user.txt
     - for user{i} in user.txt: create a folder named with username { eg: user1, user2 } in the SERVER directory.
           
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

3. Compile the mail client:

    ```bash
    cd path/to/client/folder
    gcc mailclient.c -o client
    ```

    Run the client:

    ```bash
    ./client <server_IP> <smtp_port> <pop3_port>
    ```

4. The client will prompt for a username and password. Enter the login details.

5. The client will show three options:
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
    - No authentication check (password check) occurs here.

- **Quit:**
    - Exits the mail client.

## Notes

- Both "from" and "to" mail formats are `<user_name>@<domain_name>`.
- For sending mail, users must exist in `user.txt`.
- Authentication is not checked for sending mail.
- Authentication is checked for managing mails.
- Mails are listed with the following information:
    - Serial Number
    - Sender's email id
    - Received timestamp (date : hour : minute)
    - Subject

---
