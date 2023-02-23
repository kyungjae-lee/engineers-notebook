<a href="../">Notebook</a> > <a href="./">Computer/Software Security</a> > Firewalls

# Firewalls



## Network Organization



<img src="./img/network-organization.png" alt="network-organization" width="700">



* No internal system directly communicates with the Internet. None of the internal network directly communicate with the section between the **inner firewall** and the **outer firewall** even.



## Firewalls

* A firewall is a host that mediates access to a network.
  * Allows, disallows accesses based on configuration and type of access.
* Example: Block botnet agents
  * Agent allows external users to control systems
    * Requires commands to be sent to a particular port (say, 25345)
  * Firewall can block all traffic to or from that port
    * So even if agent installed, outsiders can't use it

### Outer Firewall Configuration

* Goals - restrict public access to corporate network; restrict corporate access to Internet
* Required - public needs to send, receive email; access web services
  * So outer firewall allows SMTP, HTTP, HTTPS
  * Outer firewall uses its address for those of mail, web servers
* To do it safe, make DNS server, mail server, web server run on different systems from each other and from the firewall.

### Inner Firewall Configuration

* Goals - restrict access to corporate internal network (Has to be more strict than that of outer firewall's.)
* Rule - block ALL traffic except for that specifically authorized to enter 
  * Principle of fail-safe defaults
* Example - Drib uses NFS on some internal systems
  * Outer firewall disallows NFS packets crossing
  * Inner firewall disallows NFS packets crossing, too.
    * DMZ does not need access to this information (least privilege)
    * If inner firewall fails, outer one will stop leaks, and vice versa (separation of privilege)



## DMZ

* In DMZ, external customers can access it without going onto internal network.



## Basic Principles of Computer Security

* Principle of Least privilege
  * A subject should be given only those privileges that it needs in order to complete its task.
* Principle of complete mediation
  * All accesses to objects be checked to ensure that they are allowed. (Everything should be looked at.)
  * Inner firewall mediates every access to DMZ
* Principle of separation of privilege
  * A system should not grant permission based on a single condition. (More than one entity has to make the decision)
  * Going to Internet must pass through inner, outer firewalls and DMZ servers.
* Principle of least common mechanism
  * Mechanisms used to access resources should not be shared.
  * Inner, outer firewalls are distinct; DMZ servers separate from inner servers.
  * DMZ DNS violates this principle
    * If it fails, multiple systems affected
    * Inner, outer firewall addresses fixed, so they do not depend on DMZ DNS



## Bastion Host

* A bastion host is **a server whose purpose is to provide access to a private network from an external network, such as the Internet**. Because of its exposure to potential attack, a bastion host must minimize the chances of penetration.
