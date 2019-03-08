Host a HTTP service so that only a devices on a certain VLAN can access it

----
#### SchoolBox
- AM2 - CiscoDevNet - SIH19 - Team68
- Increasing BYOD productivity in classrooms

**The Cisco DevNet Ideas are submitted in:**

#### Primary parts of the project
- WiFi Access Points
- A L3 Captive Portal with AAA
- A RADIUS Authenticating Server
- User database
- An API Server
- Cisco Firepower
- Network Webapps for teachers
- Network Webapps for students

#### List of Cisco Firepower APIs Used for policy making
- skjdas
- sakjd 

#### Aspects of SchoolBox
- Combination of non-cisco products with cisco
- VLAN Separation of Teacher and Student Apps
- User management is centralized and limited by design

```
------------       |-----------------|   tx/rx  ------------------
| internet |<----->| cisco firepower |<-------->|   CoocaChilli  |
------------       -------------------          -------^----------
                                ^                      |        ^
                           api  |        |--------------        | tx/rx
        ------------------------|--------|                      |
        |                       |                               |
--------v--------       api  ---v--------------------           |
| RADIUS Server |<---------->| SchoolBox API Server |           |
-^---------------   api      ----^-------------------           v
 |  ^   -------------------------|      ^                      -----------
 |  |   |       AAA                     |                      | WiFi AP |
 |  ----{-------------------------------{--------------------->|   NAS   |
 |      |               api             |                      --^------^-
-v------v-     --------------------------       tx/rx            |      |
| SQLDB  |     |      --------------------------------------------      |
----------     |      |                                                 |
               v      v                                                 v
        -------------------                                    -------------------
        |                 |                                    |ssid1|ssid2      |
        |                 |                                    |Teachers|Students|
        |                 |                                    |WPA2-En|WPA2-En  |
        v                 v                                    -------------------
    ----------------      ----------------
    | Teacher Apps |      | Student Apps |
    | VLAN 200     |      | VLAN 100     |
    ----------------      ----------------
```

#### List of Teacher Apps
- MainDashboard
- TestMode
- RulesDashboard

#### List of Student Apps
- TestMode 

#### How the device will be detected
- When the students submits the token, the user agent will be detected by the server and will be stored


#### Functions of CoovaChilli
Once it's authenticated, send radius auth

- CoovaChilli will run in a docker container
- Hostapd and dnsmasq runs in a docker container
- Freeradius runs in  a docker container

#### Flow
HTTP services will listen only on the IP of the interface on that vlan


#### Contribute
