# Profiles

  - Each Profile seems to be broken down into (Device, Profile) pairs, named after an all-caps, hypen-delimited, [8,4,4,4,12] uuid, ending in `.sdProfile`: e.g. `FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF.sdProfile`
  - There appears to be a separate directory for each button on a (0,0) .. (4,2) grid.
  - Each Profile has a `manifest.json` at the top-level

### manifest.json

```
cat manifest.json | jq 'keys'
[
  "Actions",
  "DeviceModel",
  "DeviceUUID",
  "Name",
  "Version"
]
```

#### Version
Not sure the version of what, I assume it's the profiles. I've only seen `1.0`

#### Name
The disploy name of the profile in the per-device section of the Preferences dialog in the Stream Deck application

#### DeviceUUID
  - Physical Stream Deck: `@(1)[4057/109/AL43J2C22044]`
    - I assume `4057/109` have something to do with the model revision? and `@(1)` is a physical device? maybe?
    - The Serial Number on the label on back of the device: `AL43J2C22044`
  - iPhone iOS App: `@(8)[ffffffff-ffff-ffff-ffff-ffffffffffff]`

#### DeviceModel
This seems to just be what it says on the tin. Looks like the Stream Deck model on the sticker on the back of the device: `20GAA9902` or `VSD/Wifi` for the iOS app.

#### Actions
# One JSON key for each physical key on the grid that has assets defined

```
for manifest in $(find * -name manifest.json); do cat ${manifest} | jq -rc '.Actions | keys'; done
["0,2","1,1"]
["2,0","2,1","2,2","3,0","3,1","3,2","4,0","4,1","4,2"]
["0,0","2,0","2,1","2,2","3,0","3,1","3,2","4,0","4,1","4,2"]
```

```
for manifest in $(find * -name manifest.json); do for key in $(cat ${manifest} | jq -rc '.Actions | keys[]'); do cat ${manifest} | jq -rc --arg grid "$key" '.Actions | .[$grid] | keys'; done ; done
["Name","Settings","State","States","UUID"]
..
["Name","Settings","State","States","UUID"]
```

#### Name
Seems to be the name of the specific control...

```
for manifest in $(find * -name manifest.json); do for key in $(cat ${manifest} | jq -rc '.Actions | keys[]'); do cat ${manifest} | jq -rc --arg grid "$key" '.Actions | .[$grid] | .Name'; done ; done
Website
Multi Action
Adjust Brightness
Adjust Temperature
Leave Meeting
Adjust Brightness
Adjust Temperature
Video Toggle
On / Off
Website
Mute Toggle
Switch Profile
Adjust Brightness
Adjust Temperature
Leave Meeting
Adjust Brightness
Adjust Temperature
Video Toggle
On / Off
Website
Mute Toggle
```

#### Settings
Seems to be the specific-control settings.

```
{"openInBrowser":true,"path":"https://us02web.zoom.us/j/8948405401?pwd=Y3hvbndrRVd4OXpWcXRuL1JZay9Tdz09"}
{"Routine":[{"Name":"Open","OverrideState":0,"Settings":{"openInBrowser":true,"path":"/Applications/zoom.us.app"},"State":0,"States":[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"zoom","TitleAlignment":"","TitleColor":"","TitleShow":""}],"UUID":"com.elgato.streamdeck.system.open"},{"Name":"Switch Profile","OverrideState":0,"Settings":{"DeviceUUID":"","ProfileUUID":"E73F927C-D0D3-43D9-A449-75E24847536A"},"State":0,"States":[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"switch to zoom","TitleAlignment":"","TitleColor":"","TitleShow":""}],"UUID":"com.elgato.streamdeck.profile.rotate"}],"RoutineAlt":[]}
{"deviceID":"BW33J1A04772","lights":{"brightness":10},"name":"Elgato Key Light Right Side","step":"9"}
{"deviceID":"BW33J1A04772","lights":{"temperature":4700},"name":"Elgato Key Light Right Side","step":"2"}
null
{"deviceID":"BW33J1A04772","lights":{"brightness":10},"name":"Elgato Key Light Right Side","step":"4"}
{"deviceID":"BW33J1A04772","lights":{"temperature":4700},"name":"Elgato Key Light Right Side","step":"7"}
null
{"deviceID":"BW33J1A04772","name":"Elgato Key Light Right Side"}
{"openInBrowser":true,"path":"https://git.io/JTghY"}
null
{"DeviceUUID":"","ProfileUUID":"414E1EC3-E38B-4DD8-8E96-E6C62242515C"}
{"deviceID":"BW33J1A04772","lights":{"brightness":10},"name":"Elgato Key Light Right Side","step":"9"}
{"deviceID":"BW33J1A04772","lights":{"temperature":4700},"name":"Elgato Key Light Right Side","step":"2"}
null
{"deviceID":"BW33J1A04772","lights":{"brightness":10},"name":"Elgato Key Light Right Side","step":"4"}
{"deviceID":"BW33J1A04772","lights":{"temperature":4700},"name":"Elgato Key Light Right Side","step":"7"}
null
{"deviceID":"BW33J1A04772","name":"Elgato Key Light Right Side"}
{"openInBrowser":true,"path":"https://git.io/JTghY"}
null
```

### State
Seems to float between 0-2

```
0
0
0
0
1
0
0
2
1
0
2
0
0
0
1
0
0
2
0
0
2
```

### States
Some kinds of metadata, if needed
```
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"state0.png","Title":"drunk zoom","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"state0.png","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"e-queue","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"home","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"e-queue","TitleAlignment":"","TitleColor":"","TitleShow":""}]
[{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""},{"FFamily":"","FSize":"","FStyle":"","FUnderline":"","Image":"","Title":"","TitleAlignment":"","TitleColor":"","TitleShow":""}]
```

### UUID
Appears to be a namespaced name for the specific control
```
for manifest in $(find * -name manifest.json); do for key in $(cat ${manifest} | jq -rc '.Actions | keys[]'); do cat ${manifest} | jq -rc --arg grid "$key" '.Actions | .[$grid] | .UUID'; done ; done
com.elgato.streamdeck.system.website
com.elgato.streamdeck.multiactions.routine
com.elgato.controlcenter.brightness-stepper
com.elgato.controlcenter.temperature-stepper
com.lostdomain.zoom.leave
com.elgato.controlcenter.brightness-stepper
com.elgato.controlcenter.temperature-stepper
com.lostdomain.zoom.videotoggle
com.elgato.controlcenter.lights-on-off
com.elgato.streamdeck.system.website
com.lostdomain.zoom.mutetoggle
com.elgato.streamdeck.profile.rotate
com.elgato.controlcenter.brightness-stepper
com.elgato.controlcenter.temperature-stepper
com.lostdomain.zoom.leave
com.elgato.controlcenter.brightness-stepper
com.elgato.controlcenter.temperature-stepper
com.lostdomain.zoom.videotoggle
com.elgato.controlcenter.lights-on-off
com.elgato.streamdeck.system.website
com.lostdomain.zoom.mutetoggle
```
