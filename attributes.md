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


## in the template editor

### Replace
```yaml
{% set my_test_json = {
  "gps": "7.8199286,-122.4782551"
} %}
 {{ my_test_json.temperature }} 
{{ my_test_json.gps | replace(",",":")  }} 
```
### Split
```yaml
{% set my_test_json = {
  "gps": "7.8199286,-122.4782551"
} %}
 {{ my_test_json.temperature }} 
{{ my_test_json.gps.split(',')[0] }} 
```
split(',')[0]

## get all attrbutes of a given entity_id  

```yaml
% set entity_id = "automation.tasker_hook" %}

{{ entity_id }}

{{ states[entity_id.split('.')[0]] }}
{{ states[entity_id.split('.')[0]] | list }}
{{ states[entity_id.split('.')[0]][entity_id.split('.')[1]] }}

{{ (states[entity_id.split('.')[0]][entity_id.split('.')[1]]).attributes.friendly_name }}
{{ (states[entity_id.split('.')[0]][entity_id.split('.')[1]]).attributes["friendly_name"] }}
``` 
