# Accessing a remote service (solution*)

The problem can be solved using a simple parameter when giving the SSH command, 
mapping a remote port to a local port through the ssh tunnel.

The option to use the ssh tunnel is due to the practicality and ease of implementation in the existing architecture and does not affect the service running on server A, which also uses port 8000 to perform the service.

**considering:**
> SSH is installed and configured.

> <IP_ServerA> = IP or hostname do Server A.

> <IP_ServerB> = IP or hostname from Server B.

> <Remote_PortB> = Port on which server b is running the service.

> <Local_Port> = Desired local port to access the service.

```bash
$ ssh <user>@<IP_ServerA> <Local_Port>:<IP_ServerB>:<Remote_PortB>
```
## Exemple:
Mapping port 8000 from server B to local port 8082:
> <IP_ServerA> = 10.1.0.3

> <IP_ServerB> = 10.1.0.4
```shell
ssh root@10.1.0.3 -L 8082:10.1.0.4:8000
```
With the ssh tunnel open, we can access the http services of the servers through localhost using the mapped port.
***You can access the server A service as well.***
```shell
ssh root@10.1.0.3 -L 8081:10.1.0.3:8000
```
 
![alt text](https://github.com/MTMonteiro/challenge-remote-access/blob/master/solution/Remote-access.png)

## In a more continuous use of the server
We can use autossh, it will be responsible for reestablishing the connection automatically in the event of a reboot or network outages.

Another possibility would be to use the "-g" parameter in ssh, so that other clients have access to the service on the network.

## Exemple

If ClientA use:
```shell
ssh -g root@10.1.0.3 -L 8081:10.1.0.3:8000
```
Other clients will be able to access using:
> http://<ClientA_IP>:8082

![alt text](https://github.com/MTMonteiro/challenge-remote-access/blob/master/solution/Clients.png)


***The use of a key in these cases is recommended if you do not want to keep pressing the password frequently.***

# instalando o autossh:
```shell
$ sudo yum install autossh
```

# Using:
Usage is almost the same as ssh.
```shell
$ autossh root@10.1.0.3 -L 8082:10.1.0.4:8000
```
***To get a little more performance, we have the <a href="https://github.com/sshuttle/sshuttle.git" target="_blank">`sshuttle`</a>***

Code by _Matheus Monteiro_.
