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
[chaining eap authentication with webauth](https://security.stackexchange.com/questions/140965/wpa2-eap-and-captive-portal)

This can be achieved with cisco wlc but as I don't have access to one, we will try achieving it with coovachilli
browser-based Universal Access Method (UAM) captive portal.

HTTP services will listen only on the IP of the interface on that vlan

----
- what is wpa_guests

https://code.google.com/archive/p/fabfi/wikis/IntroductionToRadius.wiki


## Bugs
- Students cannot connect to the network untill and unless the network is connected to the internet, because the local DNS servers are not being used
    - possible solution can be to configure coovachilli to use something like dnsmasq for dhcp relaying or using a more proper captive portal solution

## Takeaways
- Setting up coovachilli is a PITA with **BAD** docs.
    - Use OpenWRT with Packetfence if don't want to spend anything on a proper solution
- FreeRadius logging can be improved with coloring

## Freeradius
There are two general kinds of attributes : Check Attributes and Reply Attributes
Attribute Value Pairs (AVPs)

How user files work: https://stackoverflow.com/questions/13658080/freeradius-users-operators/17968867#17968867


#### Docker magic
docker pull phpmyadmin/phpmyadmin
docker network create schoolbox-network

```

docker run --name phpmyadmin-sb \
           --net=schoolbox-network \
           -e MYSQL_ROOT_PASSWORD=password \
           -e PMA_HOST="mysql-sb" \
           -e PMA_PORT=3306 \
           -p 8080:80 \
           -d phpmyadmin/phpmyadmin
```

#### Contribute
