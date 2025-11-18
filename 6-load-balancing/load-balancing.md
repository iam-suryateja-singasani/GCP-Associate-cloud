# Cloud Load Balancer

Distributed user traffic across instances of an application in single region or multiple regions.
    1) fully distruted, software defined managed service
    2) important feature:
        a) health check -route to healthy instances, recover for failure
        b) auto scaling
        c) global load balancing with single anycast IP(can be used to receive traffic from multiple regions around the world) and also supports internal load balancing.
High availability : even if instances go down and come up cloud loadbalancing ensures that users does not effected.
it distributr traffic between healthy instances


# protocols(http vs https vs tcp vs tls vs udp)

when ever two systems talk to each other means communication happen over multiple layers the imp layesr are application layer(http, https, smtp), transport layer(tcp,tls,udp) and network layer(ip)

computers use protocols to communicate
**network layer 3**: responsable for transfer bits and bytes like 0's and 1's which is unreliable (possiability of data loss)

**transport layer 4**: responsable to ensure the bits and bytes transferred properly

TCP(Transmission control): more relaible than perfoemance
TLS(Transport layer security): secure TCP
UDP(USer Datagram Protocal): more performance than reliable
video streming. gameing

**application layer 7**: to make rest api calls and send emails
http(hypertext yransfer protocal): stateless request response cycle
https: secure http by using certificate
smpt: for emails

each layer makes use of the layers beneath it means for eg if we are using layer 7 which calls layer 4 and that calls layer 3 

## create load balancer and distribute the load between all the managed instances 

Network services --> load balancing --> https(single and multi region), tcp(single and multi region), udp(single region only)

**backend configuration**: where you want to redirect the incomming traffic to, for us it is managet instance group, so it a endpoint that receive traffic from a google cloud load balancer

**host and path rules**: deside how to redirect traffic to your backends, define rules redericting the traffic to different backend
    based on path: flipkart.com/cart vs flipkart.com/orders
    based on host: a.flipkart.com vs b.flipkart.com
    based on http headers(authorization header) and methods(post, get, etc)

**frontend configuration**: where you would receive the traffic. user hit a ip adress wit port and protocal. this ip address is the frontend ip for your client requests


### load balancing - SSL/TLS Termination/Offloading

traffic from user to load balancer through interneet is using https request

the traffic between load balancer to vm instance is through google internal network so it is ok to use htto but priffered https

ssl/tls termination/offloading: so the secure part of communication is beeing terminated at loadbalancer
    client to loadbalancer: https/tls
    loadbalancer to vm instance: http/tcp

    bec of offloading you are reducing load on the instances

### choose a loadbalance in GCP
    where the traffic traffic comming from
       1) internal traffic 
       2) external traffic form internet

    what kind of traffic are you getting : http, tcp,udp   

    let consider first internal traffic so you have tcp or udp kind and http or https kind of traffic
    for tcp/udp you can use internal tcp/udp loadbalancer
    for http or https traffic youc an use internal https loadbalancer

    external traffic from internet and the traffic kind is http or https so check the resources(vm) are in regional or global use loadbalancer accordangely

    in the tcp king of reaffic and there is ssl offloading then use ssl proxy

    if you dont want ssl offloading and you want global loadbalancer or ipv6 adress then use tcp proxy if you want preserve client ip's then go for external network loadbalancer which is recomended for udp and icmp traffic as well


features of load balancing    

**external LB** --> http(s) --> global, external, http or https --> proxy LB --> http on 80 or 8080, https on 443

**proxy LB**: get the request from a client then make changes in it and then send the different request to the backend

**pass-through**: what ever request come from client directly sends to the backend so the client will see all the details of request which is send by the client as it is
and the backend will see the ip adress of the client

**internal https** --> regional, internal, http, https --> proxy--> http on 80 or 8080, https on 443

**ssl proxy** ---> global, external, tcp with ssl offload --> proxy 

**TCP proxy** ---> global, external, tcp without ssl offload --> proxy 

**external network TCP/UDP** --> regional, external, tcp or udp ---> pass-through

**internal TCP/UDP** regional, internal,tcp or udp ---> pass-through  --> any