# Attributes

<p>
For the Home Assistant Template editor or Automations/Scripts. 
</p>

## Basics attribute 
`{{states("input_number.cctime")}}` </br>
or</br>
`{{ (states("input_number.cctime"))  }}`  all returns `4.0`</br>

same as above but without error handeling:</br>
`{{ states.input_number.cctime.state}}` </br>

### Basic Manipulation

`{{ (states("input_number.cctime") | int *60 )  }}` returns `240`


`{{ relative_time(states.binary_sensor.mija_door.last_changed) }}` returns `1 hour`

## Setting Variables
