---
tags:
  - "#Networks"
  - "#Machine_Learning"
  - "#Vehicular"
  - RSA
---
# Type of Messages

**LAKs** -> awareness
**CAM** -> contains info about the station such as speed and position
**SPATEM** -> semáforos
**DENM** -> contains info about the event which generated the message
**VAM'S** ->awareness between VRUs (Vunerable Road Users)
**MAP** -> road Topogoly
**MCM** -> has the maneuvers and one or more trajectories

## LAK 

## CAM
**Cooperative Awareness Messages** are periodic (1Hz to 10 Hz) and contain the information about the station such as the position and angle. This messages create and maintain the awareness of vehicles using the road network or RSUs. 
<img src="CAM.png" style="display: block; margin: auto;" />
The **HF (High Frequency Container)** is in every CAM message, while the **LF (Low Frequency Container)** can be updated with a maximum of 5Hz frequency. 
 
## SPAT

## DENM
**Decentralized Environmental Notification Messages** are asynchronous and contain information about the event and the staion that the message was generated for.
<img src="DENM.png" style="display: block; margin: auto;" />


## VAM

## MAP

## MCM

**Manoeuvre Coordinaton Messages** include the intented or planned maneuvers and one or more desired or alternative trajectories. They are generated at a rate between 1 Hz and 10 Hz.
<img src="MCM.png" style="display: block; margin: auto;" />

