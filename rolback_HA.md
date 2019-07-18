

hassio

'$ hassio ha update --version 0.XX.X'


venv 

``` 
$ sudo systemctl stop home-assistant@homeassistant.service
$ sudo -u homeassistant -H -s
$ source /srv/homeassistant/bin/activate
$ pip3 install homeassistant==0.XX.X
             # just updating  $ pip3 install --upgrade homeassistant
$ exit
$ sudo systemctl start home-assistant@homeassistant.service

``` 
