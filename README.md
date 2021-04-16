## Webrelay Model: X-WR-1R12-1H
```  
Implement a easy way to read and set the webrelay through js, websockets and php.
unfortunately this does not work with their latest models in the range of X401 > X4xx 
After some testing several webrelays, this is the one we picked because it registered over
50 inputs per second flawlessly.

How dow we use the webrelay.
We have to count the number of people passing a rotating gate. 
This gate has two switches, switch A and Switch B. When first switch A and then B is triggererd, 
one person is entering, when switch B is triggered and then A, one person is leaving.

The time gap between A-B(in) of B-A(out) (switch-gap) is very short, the time gap between the sequence 
of in out is larger then the switch-gap

So its important that we have the triggers reported fast and reliable.
For now we only connect to switch A, switch A can be a simple make contact

```  

![alt text](https://www.controlbyweb.com/webrelay/images/webrelay_170.png)


## Connect the Webrelay
```
Provide 12V on the Vin+ and Vin-
Connect ethernet cable
Connect +5V out to one side of the switch/contact A
Connect Input+ to the other side of the switch/contact A
Connect Vin- to Input- ( just make a litle bridge with a wire

Find the ip-address of the webrelays, default its http://192.168.1.2/, for now we keep using that
Goto http://192.168.1.2/setup.html
Goto tab Relay/Input
When asked for a username / password, default is admin/webrelay
    Relay mode: standard
    Pulse duration: 0.1 default
    Remote Relay options: remote command equals input
    Remote Relay Ip Address: Here, enter the ip number of your web server, for us its 192.168.1.152
    Remote tcp port: 80 default
    Relay#: 1, when connecting more Webrelay units, change this number accordingly, 
            its the relay numbner whats reported to your server.
    Password: default
    Keep alive: No, default
    
The hardware setup is done. Now everytime the input on the webrelay is triggered it will be send to
http://192.168.1.152 This behaviour is not documented as far as we know, but we are not sure, 
we found out by using wireshark to analyze the web traffic.

The call to the server is like http://192.168.1.152/state.xml?relay1State=1 or 0

On the server we have implemented .htaccess with the following rule

<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule ^state.xml$ /webrelays/reroute.php
</IfModule>


Code of reroute.php
<?php
error_log(print_r($_REQUEST, true));
?>

From here its up to you. In your error log each event will be logged 

To set the relay on or off call
http://192.168.1.2/state.xml?relayState1=1 for ON or
http://192.168.1.2/state.xml?relayState1=0 for Off


    

```
