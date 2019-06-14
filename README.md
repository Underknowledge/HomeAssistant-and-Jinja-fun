# HomeAssistant Jinja fun
Snipets I use all the time, or look for...
<br>
<br>
VERY deam useful rebo by Skalavala <br>
https://github.com/skalavala/mysmarthome/blob/master/jinja_helpers/readme.md

</br>
</br>
</br>

# Attributes

<p>
For the Home Assistant Template editor or Automations/Scripts. 
</p>

## Basics attributes 
`{{states("input_number.cctime")}}` </br>
or</br>
`{{ (states("input_number.cctime"))  }}`  all returns `4.0`</br>

same as above but without error handeling:</br>
`{{ states.input_number.cctime.state}}` </br>

## Basic Manipulation

`{{ (states("input_number.cctime") | int *60 )  }}` returns `240`


`{{ relative_time(states.binary_sensor.mija_door.last_changed) }}` returns `1 hour`

</br>
</br>
</br>


# Template editor

### Replace
```yaml
{% set my_test_json = {
  "gps": "7.8199286,-122.4782551"
} %}
 {{ my_test_json.temperature }} 
{{ my_test_json.gps | replace(",",":")  }} 
```
 returns `7.8199286:-122.4782551`
</br>

### Split
```yaml
{% set my_test_json = {
  "gps": "7.8199286,-122.4782551"
} %}
 {{ my_test_json.temperature }} 
{{ my_test_json.gps.split(',')[0] }} 
```
 returns `7.8199286`

`(',')[1] }} ` 
 would return `-122.4782551` 
</br>

### cut out the name and capitalize the first letter

` {{ 'binary_sensor.waschmaschine_running' | regex_replace('.*\.', '') | regex_replace('_.*', '') | title }} `

 returns `Waschmaschine` 
</br>
### get all attrbutes of a given entity_id  

```yaml
% set entity_id = "automation.tasker_hook" %}

{{ entity_id }}

{{ states[entity_id.split('.')[0]] }}
{{ states[entity_id.split('.')[0]] | list }}
{{ states[entity_id.split('.')[0]][entity_id.split('.')[1]] }}

{{ (states[entity_id.split('.')[0]][entity_id.split('.')[1]]).attributes.friendly_name }}
{{ (states[entity_id.split('.')[0]][entity_id.split('.')[1]]).attributes["friendly_name"] }}
``` 



 
 
</br>
</br>
</br>
 
 # messages
</br>

## random
```yaml
      - service: notify.xyz
        data_template:
          message: >-
                 {{ [
                 " message 0",
                 " message 1",
                 " message 2",
                 " message 3",
                 " message 4",
                 " message 5",
                 " message 6",
                 " message 7",
                 " message 8",
                 " message 9",
                 " message 10"
                 ] | random }}
```

### output:  </br>  
</br>
 returns `message 10`, then  `message 4`, then `message 3`, then `message 10`, then...  </br>  

could be used with `{{now().strftime('%d')}} ` for things like open contests. </br>  
</br></br>
### Keep the style


```yaml
notify:
- name: logfile
  platform: file
  filename: /config/notify.log
  timestamp: true
automation:
  trigger:
  action:
    - service: notify.logfile
      data_template:
        message: |-
          ///
          ///////////////////////////////////
          /                                 /
          ///////////////////////////////////

           #     # ####### #     # 
           #  #  # #     # #  #  # 
           #  #  # #     # #  #  # 
           #  #  # #     # #  #  # 
           #  #  # #     # #  #  # 
           #  #  # #     # #  #  # 
            ## ##  #######  ## ##

          /////////////////////////////////////////////////////////////////
          ////////////////////////////////////////////////////////////////

```
all done by ` |- ` 
 renders to 
 
 
 ``` 
 ///
///////////////////////////////////
/                                 /
///////////////////////////////////
 
 #     # ####### #     # 
 #  #  # #     # #  #  # 
 #  #  # #     # #  #  # 
 #  #  # #     # #  #  # 
 #  #  # #     # #  #  # 
 #  #  # #     # #  #  # 
  ## ##  #######  ## ##
 
/////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////// ``` 
``` 
 </br>
 </br>
 
 # Timestamps 


### string to stamp


string from sensor.ups_transfer_to_battery.state:
`2019-06-12 09:44:56 +0700`
</br>
 Convert it to an timestamp and get the relative time:
`{{ relative_time(strptime(states.sensor.ups_transfer_to_battery.state, '%Y-%m-%d %H:%M:%S %z')) }}` 
</br>
</br>
### timezones
`| timestamp_custom('%Y-%m-%dT%H:%M:%S', False) }}Z` 
timestamp_custom also lets you flag if the time is local time or not

you can add `}}Z` for the Zulo Timezone `}}L` for Local and so on  
or 
`{{ | timestamp_local }}`
`{{ | timestamp_utc }}`
`{{ | timestamp_custom('%Y' True) }}`
</br>
### Create a timestamp with an automation 


```yaml
automation:
  - alias: timestamp
    trigger:
    action:
      - service: notify.timestamp
        data_template:
          message: "{{ { 'timestamp': now()|string } |tojson }}"
 
sensor:
  - platform: file
    name: vit_d3_timestamp
    file_path: /config/imestamp.txt
    value_template: '{{value_json.timestamp}}'
  - platform: template
    sensors:
      timestamp_processed:
        friendly_name: "timestamp"
        unit_of_measurement: 'days'
        value_template: "{{ (( as_timestamp(now()) - as_timestamp(strptime(states('sensor.timestamp'), '%d.%m.%Y')) ) / 86400 ) | round(2) }}"
```
          
