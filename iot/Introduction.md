
### Internet of Things (IoT)

Internet of **Things**.

### Things

Hardware devices that can have the ability to: 
    **sense -> process -> take action**

### Communication

Connected  **Things**  collaborate to achieve their goals.
=> Each **thing** must be able to **communicate** with other **things**.

#### Peer-to-Peer (P2P)
![[p2p.png|300]]
Each device is connected to every other device.
Not very scalable.

#### Server-Client
![[client-server.png|300]]
Central device (server) is connected to every other device
Fairly scalable.

#### Cloud-Fog-Edge

Tree-like structure.
Easily scalable.

### Example: Uber

**Thing:** Mobile Phone
**Sensor:** GPS

**Processing:**
    -> Server: Nearest ride, Pricing, Ride allocation, ...
    -> Edge: Getting coordinates of user, app rendering, ...

**Action:** Book the ride
**Communication:** Regular mobile phone network.

### Example: Smart Road Traffic Monitoring System

**Objective:** Estimate Traffic on a road.

**Thing:** Mobile Phones
**Sensors:** GPS

**Processing:**
    -> Calculate number of signals in a specific area (corresponding to the road)

**Action:** Calculate traffic on a road.
**Communication:** Regular mobile phone network.





