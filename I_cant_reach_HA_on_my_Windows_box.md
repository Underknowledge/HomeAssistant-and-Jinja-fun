
Edit hosts file `c:\windows\system32\drivers\etc\hosts` 

add your HA box like this 

``` 
external.dns.name    1.2.3.4

``` 

Press the “Win” button, type “cmd“.
Right-click “Command Prompt“, choose “Run as Administrator“.
Type `ipconfig /flushdns`  press “Enter“.
