---
layout: post
title:  "How to setup High Availability Cluster of HAProxy using Keepalived"
author: sma
categories: [ HAProxy,Keepalived,HighAvailability ]
image: assets/images/posts/art-board-game-challenge-163064.jpg
description: "How to setup High Availability Cluster of HAProxy using Keepalived?"
tags: [featured]
---

How to setup High Availability Cluster of HAProxy using Keepalived?

HAProxy (High Availability Proxy) serves it's job well as a *Reverse Proxy* and *TCP / HTTP load balancer*.


![HAProxy Load Balancer]({{ site.baseurl }}/assets/images/posts/ha-proxy-load-balancer.jpg)

In *HAProxy load balancing setup* shown in above diagram the *HAProxy* is the single points of failure,which may cause downtime / service unavailability. To make this a true *High Availability setup*, we need to make *HAProxy load balancer* redundant with two or more *HAProxy* nodes using *Keepalived and VRRP*.

![HAProxy Load Balancer]({{ site.baseurl }}/assets/images/posts/ha-proxy-load-balancer-high-availability-setup.jpg)

The example shown in above diagram uses a *virtual ip address* which is currently assigned to *Active HAProxy node* and in case of failure it can be reassigned to the *HAProxy Backup node*. We we explore *How to build redundant with High Availability cluster of multiple HAProxy servers using Keepalived, LVS and VRRP* in following sections of this article.

## Installing HAProxy

Please follow my article [How To Install HAProxy on Ubuntu Linux Server]({{ site.baseurl }}{% post_url 2019-03-27-how-to-install-haproxy-on-ubuntu-linux-server %}) to get *HAProxy* installed on participant servers *(that you want to make redundant)*.

## Install Keepalived

As mentioned above we will create the *High Availability CLuster of HAProxy* using *Keepalived* so you need to install *Keepalived* on all the participant servers with *HAProxy* installed. You can follow my article [Setup Highly Available Private Networking using Keepalived (VRRP)]({{ site.baseurl }}{% post_url 2019-03-27-highly-available-private-networking-using-keepalived %}) to get it done.

## Our Test Environment

We will be using following network / IP Address configurations in this article.

{:class="table table-responsive "}
| Server Name | IP Address    | Role                    |
|-------------|---------------| -------------------------
| srv-1       | 192.168.0.101 | Primary / Master Node   |
| srv-2       | 192.168.0.102 | Backup / Slave Node     |
| srv-3       | 192.168.0.103 | Backup / Slave Node     |



## HAProxy Health Check with Keepalived Check Script

*Keepalived Check Script* is used as a trigger to change current state of *Keepalived virtual router*. *Keepalived* changes the current state of *virtual router* on the basis of value returned by *Check Script* as following.

{:class="table table-responsive "}
| Check Script Return Value     | Description                                   |
|-------------------------------|-----------------------------------------------|
| 0                             | Everything is working fine / Healthy State    |
| 1 or any other value than 0   | Something went wrong / Faulty State           |

We will create *HAProxy service / process status check* script to make sure if *HAproxy service* is up and running, this script will work as a trigger to switchover to *Keepalived BACKUP node with next highest priority*. 

Create `haproxy-service-check.sh` shell script file on each participant node with *Keepalived and HAProxy* installed.

1. Create `haproxy-service-check.sh` file in `/usr/local/bin/` directory using nano text editor.
    ```
    sudo nano /usr/local/bin/haproxy-service-check.sh
    ```
2. Copy following contents into newly created file, save file `(Ctrl + O)` and exit the editor `(Ctrl + X)`.
    ```
    #!/bin/sh
    # HAProxy Service status check
    # Author: SMA (write@sma.im)
    SERVICE=haproxy
    STATUS="$(pidof $SERVICE | wc -w)"

    if [ $STATUS -eq 0 ]
    then
        exit 1
    else
        exit 0
    fi    
    ```
Please make sure you have created the `haproxy-service-check.sh` shell script on all three nodes (in context of this article).

## Configuring Keepalived for Primary / Master Server

We will configure the *srv-1 (192.168.0.101)* as Primary or *Master Keepalived node*. Connect to the *srv-1 (192.168.0.101)* machine to configure it as *Keepalived master node* as following.

1. Open *Keepalived configuration* file `keepalived.conf` for editing.
    ```
    sudo nano /etc/keepalived/keepalived.conf
    ```
2.  Copy following contents to `keepalived.conf` file, save file `(Ctrl + O)` and exit the nano editor `(Ctrl + X)`.
    ```
    global_defs {
    notification_email {
        name@example.com
    }
        notification_email_from sender@example.com
        smtp_server localhost
        smtp_connect_timeout 30
    }

    vrrp_script chk_haproxy_service_status {
        script      "sh /usr/local/bin/haproxy-service-check.sh"
        interval 2
        fall 2
        rise 2
        timeout 2
        weight 5
    }

    vrrp_instance VRRP1 {
        state MASTER
        interface ens160
        virtual_router_id 66
        priority 100
        advert_int 1
        authentication {
            auth_type PASS
            auth_pass pass1234
        }
        virtual_ipaddress {
            192.168.0.200  
        }
        track_script {
            chk_haproxy_service_status
    }
    }
    ```
### Understanding the Keepalived Configurations

Before moving to the configurations for backup nodes, let's understand the *Keepalived configuration* in detail.

#### global_defs::notification_email
Email address of recipient who will receive the notification emails. You can specify one or more email addresses as following.

```
global_defs {
    notification_email {
        person1@example.com
        person2@example.com
        person3@example.com
    }
...    
}
```

#### global_defs::notification_email_from
Specified the sender's email address, this will be used for “MAIL FROM:” SMTP command.


#### global_defs::smtp_server remote

SMTP server that will be used to send notification emails.


#### global_defs::smtp_connection_timeout

Specified the SMTP timeout value.

#### global_defs::smtp_connection_timeout

#### vrrp_script
Section to define *Keepalived Tracking Script*.

#### vrrp_script::script

Specifies the command along with any arguments to be executed as *Check script*.

#### vrrp_script::interval

Time in seconds to repeat execution of *script*, in following example the *script* will be executed every 2 seconds.

```
vrrp_script script_name {
  script       "command ..."
  interval 2
  ...
}
```

#### vrrp_script::fall

Specified the number of attempts (with **non-zero** exit code returned by script) before changing the *Router* to *FAULT* state. In following example the router will enter *FAULT* state, if *script* returns *non-zero value* as exit code *continuously* 2 times.

vrrp_script script_name {
  script       "command ..."
  interval 2
  fall 2
  ...
}

#### vrrp_script::rise

Specified the number of attempts (with **zero exit** code returned by script) before exiting the *ROUTER* from *FAULT* state. In following example the router will exit from *FAULT* state, if *script* returns *0* as exit code *continuously* 2 times.

vrrp_script script_name {
  script       "command ..."
  interval 2
  rise 2
  ...
}

#### vrrp_script::timeout

Router will wait for number of seconds specified as *timeout* value before considering the *script exit code* as non-zero. In following example the router will wait for *2 seconds* even if *exit code* of *script* is *non-zero*.

vrrp_script script_name {
  script       "command ..."
  interval 2
  rise 2
  timeout 2
  ...
}


#### vrrp_script::weight

Specified the number by which current priority of router will be reduced after entering *FAULT* state. In following example current priority of router will be reduced by 5 as soon as the *router* enters to *FAULT* state.

vrrp_script script_name {
  script       "command ..."
  interval 2
  rise 2
  timeout 2
  weight 5
}

#### vrrp_instance::state

Defines the default state of *ROUTER* as *MASTER* or *BACKUP*.

#### vrrp_instance::interface
*Virtual IP Address* will be assigned to *network interface* defined against *interface* property.

#### vrrp_instance::virtual_router_id
The virtual router ID must be unique to each VRRP instance that you define
Defines the *Virtual Router Id* with following constraints.
- Unique *virtual_router_id* needs to assigned to each *Virtual Router* on a single machine.
- Same *virtual_router_id* shall be used for *VRRP instances* for different machines belonging to same cluster.

#### vrrp_instance::priority

Defines the *priority* of *Virtual Router instance*. The priority defined for *MASTER* node shall be higher than all *BACKUP* nodes. In case of a failure on *MASTER*, the *BACKUP* node with *highest* priority will become new *MASTER*.
 
#### vrrp_instance::advert_int

Specifies the time interval in seconds for *Virtual Router* to *advertise* it's current state to other member nodes of the same cluster *(with same virtual_router_id on a network)*.

#### vrrp_instance::authentication:auth_type
Specifies the type of authentication to be used by VRRP. Following example shows, how we can specify authentication type as *password authentication* for current *virtual router*.

```
authentication {
    auth_type PASS
    ...
}
```

#### vrrp_instance::authentication:auth_pass
Specifies the password for *password authentication* of current VRRP router. Following example shows, how to setup password value for  *password authentication* of current VRRP router.

```
authentication {
    auth_type PASS
    auth_pass pass1234
}
```

#### vrrp_instance::virtual_ipaddress

Defines the *virtual IP address* of a *VRRP router*.

#### vrrp_instance::track_script

Specifies the *track script* for *VRRP router*.


## Configuring Keepalived for Backup Server

We are done with setup of primary server, and now have better understanding of *Keepalived configurations* so let's start with configuring our first backup server. In this section we will configure the *srv-2 (192.168.0.102)* as our *backup Keepalived server*.

1. Open *Keepalived configuration* file `keepalived.conf` for editing.
    ```
    sudo nano /etc/keepalived/keepalived.conf
    ```
2.  Copy following contents to `keepalived.conf` file, save file `(Ctrl + O)` and exit the nano editor `(Ctrl + X)`.
    ```
    global_defs {
    notification_email {
        name@example.com
    }
        notification_email_from sender@example.com
        smtp_server localhost
        smtp_connect_timeout 30
    }

    vrrp_script chk_haproxy_service_status {
        script      "sh /usr/local/bin/haproxy-service-check.sh"
        interval 2
        fall 2
        rise 2
        timeout 2
        weight 5
    }

    vrrp_instance VRRP1 {
        state BACKUP
        interface ens160
        virtual_router_id 66
        priority 99
        advert_int 1
        authentication {
            auth_type PASS
            auth_pass pass1234
        }
        virtual_ipaddress {
            192.168.0.200  
        }
        track_script {
            chk_haproxy_service_status
    }
    }
    ```

## Configuring Keepalived for Additional Backup Server

At this point our *High Availability cluster* of two nodes with *HAproxy* and *Keepalived* on each node, is up and running. In case of failure on *Primary* node the *Backup* node will take over to serve the connected clients. In this section I we will explore how to configure *srv-3 (192.168.0.103)* as an additional backup node for our High Availability cluster. Please proceed with following steps on *srv-3*.

1. Open *Keepalived configuration* file `keepalived.conf` for editing.
    ```
    sudo nano /etc/keepalived/keepalived.conf
    ```
2.  Copy following contents to `keepalived.conf` file, save file `(Ctrl + O)` and exit the nano editor `(Ctrl + X)`.
    ```
    global_defs {
    notification_email {
        name@example.com
    }
        notification_email_from sender@example.com
        smtp_server localhost
        smtp_connect_timeout 30
    }

    vrrp_script chk_haproxy_service_status {
        script      "sh /usr/local/bin/haproxy-service-check.sh"
        interval 2
        fall 2
        rise 2
        timeout 2
        weight 5
    }

    vrrp_instance VRRP1 {
        state BACKUP
        interface ens160
        virtual_router_id 66
        priority 98
        advert_int 1
        authentication {
            auth_type PASS
            auth_pass pass1234
        }
        virtual_ipaddress {
            192.168.0.200  
        }
        track_script {
            chk_haproxy_service_status
    }
    }
    ```

## Testing our High Availability cluster of HAProxy and Keepalived

You can test this high availability setup as following.

1. With all three nodes up and running, connect to *Virtual IP (192.168.0.200)*, it will connect to *primary* node by default.
2. Now stop *HAProxy* on *primary node* using `sudo service haproxy stop` command on *primary* node.
3. Switchover to *backup* node shall happen, to confirm this refresh the browser to confirm if the connection is still healthy. 


That's it, hope you enjoyed it. You like this article, have any questions or suggestions please let us know in the comments section.

Thanks and Happy Learning!
