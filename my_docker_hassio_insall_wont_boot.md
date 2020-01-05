get into the homeassistant container and run a config check manualy.

``` 
docker exec -it homeassistant /bin/bash
python -m homeassistant --script check_config --config /config

``` 

Try running network checks like dig or ping from inside the container
When you still have issues try removing custom_component's
