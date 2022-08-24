# SSAM - Garbage collection days in Home assistant

Instructions:
1) Download the ssam.yaml file and place in your home assistant configuration folder or in a subfolder.

2) Search using this page to get your full address and code, then copy in to the configuration.
https://edpfuture.ssam.se/FutureWeb/SimpleWastePickup/Simplewastepickup  

3) Update the row for the rest sensor resource template:
Replace `Streetname 1, City (12345)` with your address and code (don't remove surrounding quotes).  



4) Add in configuration.yaml:

```
homeassistant:
  packages:
    ssam: !include packages/ssam.yaml
```


5) Example automations to remind you about the garbage collection on the evening before: 
```
- id: ac22a80d775d4cbba83e174f0fc75dd4
  alias: Påminn om sophämtning 1
  trigger:
  - platform: time
    at: '18:00:00'
  condition:
  - condition: template
    value_template: '{{ (float(as_timestamp(states("sensor.sophamtning_karl_1"),"%Y-%m-%d")-86400)|
      timestamp_custom("%Y-%m-%d")) == (as_timestamp(now()) | timestamp_custom("%Y-%m-%d"))
      }}'
  action:
  - data:
      message: Sophämtning Soptunna 1 imorgon!
      title: Sophämtning kärl 1
    service: notify.notify
    
- id: 068a13b37bac4eb1afbec7b9d5fce444
  alias: Påminn om sophämtning 2
  trigger:
  - platform: time
    at: '18:00:00'
  condition:
  - condition: template
    value_template: '{{ (float(as_timestamp(states("sensor.sophamtning_karl_2"),"%Y-%m-%d")-86400)|
      timestamp_custom("%Y-%m-%d")) == (as_timestamp(now()) | timestamp_custom("%Y-%m-%d"))
      }}'
  action:
  - data:
      message: Sophämtning Soptunna 2 imorgon!
      title: Sophämtning kärl 2
    service: notify.notify
