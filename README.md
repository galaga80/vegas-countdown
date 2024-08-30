# vegas-countdown
 Countdown clock (days) in the style of the Las Vegas sign for Home Assistant

 First up I don't know what I am doing and there are probable better ways to do this but thought I would share this for everyone.

# 1 Add a sensor to your sensors.yaml
```
      holiday_countdown:
        friendly_name: "Holiday Countdown"
        value_template: >
         {% set target_date = '2024-12-24' %}
         {% set today = (now()).strftime('%Y-%m-%d') %}
         {% set days_left = (as_timestamp(target_date) - as_timestamp(today)) / 86400 %}
         {{ days_left | int }}

      holiday_countdown_hundreds:
        friendly_name: "Holiday Countdown Hundreds"
        value_template: "{{ (states('sensor.holiday_countdown') | int // 100) % 10 }}"
      holiday_countdown_tens:
        friendly_name: "Holiday Countdown Tens"
        value_template: "{{ (states('sensor.holiday_countdown') | int % 100) // 10 }}"
      holiday_countdown_units:
        friendly_name: "Holiday Countdown Units"
        value_template: "{{ states('sensor.holiday_countdown') | int % 10 }}"
```
and restart

# 2 Create a folder
Create a folder in your **www** called **Las_veags_sign** add the files to it.

# 3 Create 2 automations

```
alias: Las Vegas image switch
description: ""
trigger:
  - platform: time_pattern
    seconds: /01
condition: []
action:
  - action: input_boolean.toggle
    metadata: {}
    data: {}
    target:
      entity_id: input_boolean.image_toggle
mode: single
```

```
alias: Las Vegas image switch
description: ""
trigger:
  - platform: time_pattern
    seconds: /01
condition: []
action:
  - action: input_boolean.toggle
    metadata: {}
    data: {}
    target:
      entity_id: input_boolean.image_toggle
mode: single

```

# 4 Create a picture-elements card
	
```
`type: picture-elements
image: /local/Las_veags_sign/Las_vegas_sign.png
elements:
  - type: image
    entity: input_boolean.image_toggle
    state_image:
      'on': /local/Las_veags_sign/Las_veags_signA.png
      'off': /local/Las_veags_sign/Las_veags_signB.png
    style:
      top: 50%
      left: 50%
      width: 100%
      height: 100%
  - type: image
    entity: input_boolean.image_toggle_5sec
    state_image:
      'on': /local/Las_veags_sign/Las_vegas_signDaysA.png
      'off': /local/Las_veags_sign/Las_vegas_signHoliday.png
    style:
      top: 50%
      left: 50%
      width: 100%
      height: 100%
  - type: conditional
    conditions:
      - entity: input_boolean.image_toggle_5sec
        state: 'on'
    elements:
      - type: image
        entity: sensor.holiday_countdown_hundreds
        state_image:
          '0': /local/Las_veags_sign/Las_vegas_sign0.png
          '1': /local/Las_veags_sign/Las_vegas_sign1.png
          '2': /local/Las_veags_sign/Las_vegas_sign2.png
          '3': /local/Las_veags_sign/Las_vegas_sign3.png
          '4': /local/Las_veags_sign/Las_vegas_sign4.png
          '5': /local/Las_veags_sign/Las_vegas_sign5.png
          '6': /local/Las_veags_sign/Las_vegas_sign6.png
          '7': /local/Las_veags_sign/Las_vegas_sign7.png
          '8': /local/Las_veags_sign/Las_vegas_sign8.png
          '9': /local/Las_veags_sign/Las_vegas_sign9.png
        style:
          top: 35.5%
          left: 59.5%
          width: 9%
  - type: conditional
    conditions:
      - entity: input_boolean.image_toggle_5sec
        state: 'on'
    elements:
      - type: image
        entity: sensor.holiday_countdown_tens
        state_image:
          '0': /local/Las_veags_sign/Las_vegas_sign0.png
          '1': /local/Las_veags_sign/Las_vegas_sign1.png
          '2': /local/Las_veags_sign/Las_vegas_sign2.png
          '3': /local/Las_veags_sign/Las_vegas_sign3.png
          '4': /local/Las_veags_sign/Las_vegas_sign4.png
          '5': /local/Las_veags_sign/Las_vegas_sign5.png
          '6': /local/Las_veags_sign/Las_vegas_sign6.png
          '7': /local/Las_veags_sign/Las_vegas_sign7.png
          '8': /local/Las_veags_sign/Las_vegas_sign8.png
          '9': /local/Las_veags_sign/Las_vegas_sign9.png
        style:
          top: 36%
          left: 64%
          width: 9%
  - type: conditional
    conditions:
      - entity: input_boolean.image_toggle_5sec
        state: 'on'
    elements:
      - type: image
        entity: sensor.holiday_countdown_units
        state_image:
          '0': /local/Las_veags_sign/Las_vegas_sign0.png
          '1': /local/Las_veags_sign/Las_vegas_sign1.png
          '2': /local/Las_veags_sign/Las_vegas_sign2.png
          '3': /local/Las_veags_sign/Las_vegas_sign3.png
          '4': /local/Las_veags_sign/Las_vegas_sign4.png
          '5': /local/Las_veags_sign/Las_vegas_sign5.png
          '6': /local/Las_veags_sign/Las_vegas_sign6.png
          '7': /local/Las_veags_sign/Las_vegas_sign7.png
          '8': /local/Las_veags_sign/Las_vegas_sign8.png
          '9': /local/Las_veags_sign/Las_vegas_sign9.png
        style:
          top: 36.5%
          left: 68.6%
          width: 9%
````
