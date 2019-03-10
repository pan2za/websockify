This is not only a clone from the offical websockify
The original idea is:

novnc client --------------->websockify---------------------------->vnc server

The idea here is:

novnc client -------------->websockify--------------------------->vnc server

                              +                                     +
                              
                              rctl-server---------(rctl-socket)----rctl-client
                              

                              =rwctl-server                         =rvctl-client

where,
rwctl-server= websockify+rctl-server
rvctl-client = vncserver+rctl-client

where, rctl can refer to the other project pan2za/rctl

Why?

I need a server as a anchor, so that I can remote control a NAT-ed internal network.

How?

1 rvctl-client will always connect to rwctl-server, and rwctl-server can list all its clients.
2 when novnc client want to connect the remote internal vnc server, it first connect to rwctl-server
3 then rwctl-server selects a rvctl-client, which can be selected by the novnc client using other control method
4 and rwctl-server connect to rvctl-client( or the vnc server, they are the same object),
5 and the rvctl-client( or the vnc server) will response as a vnc server always does.
6 then the novnc client works

为了解决内网穿透问题，采用了反向代理，由vnc server所在位置建立一个永久在线连接，连接到服务器（服务器端拥有公网IP），由此所有向内网的vnc连接，都经由服务器代理。
To resolve the internal network tranversial issue, a reverse proxy is applied. First a always-online connection is established in the vnc server, and the connection will be reversely connected to the server where there is a public IP. The whole internal network vnc connections, will always be proxied by the server.
