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

### cut out the new line 
`"{{something['somethingRece ived'].replace('\n', ''}}" `


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


Of an domain:
```yaml
{% for state in states.zone -%}
  {{ state.entity_id }}:

 {{state.attributes}}
{% endfor %}

``` 
templating out of these propertys
```yaml
{% for zone in states.zone -%}
- name: {{ state_attr(zone.entity_id, 'friendly_name') }}
  latitude: {{ state_attr(zone.entity_id, 'latitude') }}
  longitude: {{ state_attr(zone.entity_id, 'longitude') }}
  radius: {{ state_attr(zone.entity_id, 'radius') }}
{{ '\n' -}} 
{%- endfor %}

#by ludeeus:
{%- for state in states.zone -%}
- name: {{ state_attr(state.entity_id, 'friendly_name') }}
 {%- for attr in ["latitude", "longitude", "radius", "icon"] %}
  {{attr}}: {{ state_attr(state.entity_id, attr) }}
 {%- endfor %}
{% endfor -%}

``` 


### Wich user changed the light?  

```yaml
# by Ludeeus https://discordapp.com/channels/330944238910963714/330944238910963714/622811187964280832
{{ states.light.nachttisch.context }}

{%- set changed_by_id = states.light.nachttisch.context.user_id -%}
{%- for user in states.person -%}
 {%- if changed_by_id == state_attr(user.entity_id, 'user_id') -%}
  State changed by: {{ state_attr(user.entity_id, 'friendly_name') }}
  {%- endif -%}
{%- endfor -%}
``` 
 returns `State changed by: XY` 

### loop through a list, and break out of the loop

```yaml
# by Ludeeus https://discordapp.com/channels/330944238910963714/330944238910963714/630423692635013141
{% set break = namespace(active=False) %}
{% for state in states.binary_sensor if not break.active %}
  {% if state.state == "on" %}
    {{ state.entity_id}}
    {% set break.active = True %}
  {% endif%}
{% endfor %}

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



```yaml
# https://community.home-assistant.io/t/template-time/131509/2?u=underknowledge
---
sensor:
  - platform: time_date
    display_options:
      - 'date'

  - platform: template
    sensors:
      turni_data:
        friendly_name: "Data calendario turni"
        entity_id: sensor.date
        value_template: 
          # Gets your timestamp string attribute and sets it equal to tstring
          {% set tstring = state_attr('calendar.turni', 'start_time') %}
          # Gets the timezone you are currently in.  Makes it a string.
          {% set tz = now().timestamp() | timestamp_custom('%z') %}
          # Converts string+timezone into a datetime object.
          {% set date = strptime(tstring+tz, '%Y-%m-%d %H:%M:%S%z') %}
          # subtracts the current date from the timestamp date.  Then gets the number of days.
          {% set change = (date - now()).days %}
          # If the number of days are zero...
          {% if change == 0 %}
            # show today.
            Today
          # If the number of days are 1
          {% elif change == 1 %}
            # show tomorrow
            Tomorrow
          {% else %}
            # otherwise show date.
            {{ date.timestamp() | timestamp_custom('%A %d') }}
          {% endif %}
``` 

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

you can add `{{ | timestamp_local }}` for Local or 
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
When you got already a timestamp you can use it like ` state_attr('input_datetime.date_time','timestamp'))` ` 

 # Time II

```yaml
---
sensor:
  - platform: time_date
    display_options:
      - 'time'

  - platform: template
    sensors:
      turni_data:
        friendly_name: ""
        entity_id: sensor.time
        value_template: 
          {% if (states('sensor.time') > '00:01') and (states('sensor.time') < '05:00') %}
            night
          {% elif (states('sensor.time') > '05:01') and (states('sensor.time') < '06:00') %}
            morning
          {% elif (states('sensor.time') > '06:01') and (states('sensor.time') < '17:00') %}
            day
          {% elif (states('sensor.time') > '17:01') and (states('sensor.time') < '22:00') %}
            evening
          {% elif (states('sensor.time') > '22:01') and (states('sensor.time') < '23:59') %}
            night
          {% else %}
            day
          {% endif %}

``` 
