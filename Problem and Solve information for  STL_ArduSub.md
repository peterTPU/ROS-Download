# SITL with Gazebo Connection Issue

## Problem_1
When running QGroundControl (QGC) **before** starting SITL with Gazebo, you may encounter this error:

```
[Err] [ArduPilotPlugin.cc:889] [submarine_6motor] failed to bind with 127.0.0.1:9002 aborting plugin.
```
Note!!: you can in this photo
<img width="974" height="236" alt="Screenshot from 2025-12-09 19-09-57" src="https://github.com/user-attachments/assets/55488cff-14b1-414c-b31e-9cde7d60751a" />


## Cause
The error occurs because QGC automatically starts a MAVLink server on port 9002. When Gazebo's ArduPilotPlugin tries to bind to the same port (127.0.0.1:9002) for the SITL connection, it fails due to the port already being in use by QGC.

## Solution
**Close QGroundControl first**, then start your SITL simulation. After SITL and Gazebo are running successfully, you can launch QGroundControl to connect to the simulation.

### Correct Order:
1. **Close QGroundControl** (if running)
2. **Start SITL simulation** with Gazebo
3. **Launch QGroundControl** to connect to the running simulation

### Wrong Order (causes error):
1. Start QGroundControl (occupies port 9002)
2. Try to start SITL with Gazebo (fails to bind to port 9002)

## for run my sitl ArduSub i used this code:
in file ./launch_sitl.sh
```bash
#!/bin/bash
# Source ArduPilot environment (adjust path to your ArduPilot directory)
#source ~/.profile
cd ~/ardupilot/
#./Tools/autotest/sim_vehicle.py -v ArduSub -f gazebo-bluerov2 # IT IS WORK
./Tools/autotest/sim_vehicle.py -v ArduSub -N -f gazebo-bluerov2 -I0 -l 56.476730,85.084460,120.0,273.88 --no-mavproxy --udp --use-dir=submarine
```

If you see QGC using this port, close QGC before starting your SITL simulation.
