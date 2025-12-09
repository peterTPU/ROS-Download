
# 📝 ArduSub Setup Guide for Ubuntu 20.04 and ROS1  
*For ROV/Submarine Simulation with SITL*

This guide walks you through downloading, building, and testing **ArduSub** — the ArduPilot firmware for underwater vehicles (ROVs/submarines) — on **Ubuntu 20.04** using **Software-In-The-Loop (SITL)** simulation.

> ✅ Verified for educational use with student teams.

---

## 🛠️ Step-by-Step Installation

### 1. **Clone the ArduPilot Repository for this place link:https://github.com/ArduPilot/ardupilot.git **
```bash
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot/
```

### 2. **Initialize Submodules**
## You First to be in Ardupilot file in Your Linux Ubuntu
```bash
cd ardupilot/
git submodule update --init --recursive
```

### 3. **Install System Dependencies**
> ⚠️ Note: The correct script path is `Tools/scripts/install_prereqs.sh` (not `environment_install`).
> ⚠️ Note: You need to open your VPN if you in Ru
```bash
Tools/environment_install/install-prereqs-ubuntu.sh -y
source ~/.profile
```

### 4. **Install Required Python Package (if needed)**
```bash
pip3 install --user future
```

> 💡 On most Ubuntu 20.04 + Python 3 systems, this is optional — include it only if you encounter import errors.

### 5. **(Recommended) Reboot Your System**
Some dependencies (e.g., udev rules, shell PATH updates) require a restart to take full effect:
```bash
sudo reboot
```

---

## 🔧 Build ArduSub for SITL

After rebooting, return to the `ardupilot` directory:

```bash
cd ~/ardupilot
```

### Configure and Build
### Check the SUB form this : https://www.ardusub.com/developers/developers.html
## Waf is a global build system for ArduPilot repository, it's necessary to be inside the root folder of ArduPilot to use it. You can check how to use waf with:
```bash
./waf --help
```
## To configure waf to build ArduSub for Pixhawk 1:
# for test SITL
```bash
./waf configure --board sitl --notests --disable-tests
./waf sub
```
# for connection with Real Robot (SUB)
```bash
./waf configure --board Pixhawk1
./waf sub
```

# 📝 ArduSub Setup Test SITL QGC
### Terminal 1
```bash
cd ~/ardupilot
./Tools/autotest/sim_vehicle.py -v ArduSub -L RATBeach
```
## Note here you need to see black window for ArduSub and check that have not Error!

### Terminal 2
```bash
roslaunch mavros apm.launch fcu_url:=udp://0.0.0.0:14550@
```
### Terminal 3
# you need to run QGC by commed
```bash
# where will be you file you need to make
chmod +x ./QGroundControl.AppImage
ls # you need to see the name of ./QGroundControl.AppImage green
./QGroundControl.AppImage 
```
> ✅ This compiles the **ArduSub** firmware for simulation to test the SITL without Gazebo.
---
### Some information About SITL 
``` bash
peter@peter-VirtualBox:~/ardupilot$ ./Tools/autotest/sim_vehicle.py -h
Usage: sim_vehicle.py

Options:
  -h, --help            show this help message and exit
  -v VEHICLE, --vehicle=VEHICLE
                        vehicle type (ArduCopter|Helicopter|Blimp|ArduPlane|Ro
                        ver|ArduSub|AntennaTracker|sitl_periph_universal)
  -f FRAME, --frame=FRAME
                        set vehicle frame type
                        ArduCopter: +|Callisto|IrisRos|X|airsim-
                            copter|bfx|calibration|coaxcopter|cwx|deca|deca-
                            cwx|djix|dodeca-
                            hexa|dotriaconta_octaquad_x|freestyle|gazebo-
                            iris|heli|heli-blade360|heli-dual|heli-
                            gas|hexa|hexa-cwx|hexa-dji|hexadeca-octa|hexadeca-
                            octa-cwx|hexax|octa|octa-cwx|octa-dji|octa-
                            quad|octa-quad-cor|octa-quad-cw-cor|octa-quad-
                            cwx|quad|quad-can|scrimmage-
                            copter|singlecopter|tri|y6
                        Helicopter: heli|heli-blade360|heli-dual|heli-gas
                        Blimp: Blimp
                        ArduPlane: CRRCSim|calibration|firefly|gazebo-
                            zephyr|glider|jsbsim|last_letter|plane|plane-3d|pl
                            ane-dspoilers|plane-elevon|plane-ice|plane-
                            jet|plane-redundant|plane-soaring|plane-
                            tailsitter|plane-vtail|quadplane|quadplane-
                            can|quadplane-cl84|quadplane-
                            copter_tailsitter|quadplane-ice|quadplane-
                            tilt|quadplane-tilthvec|quadplane-
                            tilttri|quadplane-tilttrivec|quadplane-
                            tri|scrimmage-plane|stratoblimp
                        Rover: airsim-rover|balancebot|calibration|gazebo-
                            rover|motorboat|motorboat-skid|rover|rover-
                            omni3mecanum|rover-skid|rover-
                            vectored|sailboat|sailboat-motor
                        ArduSub: gazebo-bluerov2|vectored|vectored_6dof
                        AntennaTracker: tracker
                        sitl_periph_universal: universal
  --vehicle-binary=VEHICLE_BINARY
                        vehicle binary path
  -C, --sim_vehicle_sh_compatible
                        be compatible with the way sim_vehicle.sh works; make
                        this the first option
  -P PARAM, --param=PARAM
                        set some param with the format PARAM=VALUE

  Build options:
    -N, --no-rebuild    don't rebuild before starting ardupilot
    --no-configure      don't run waf configure before building
    -D, --debug         build with debugging
    -c, --clean         do a make clean before building
    -j JOBS, --jobs=JOBS
                        number of processors to use during build (default for
                        waf : number of processor, for make : 1)
    -b BUILD_TARGET, --build-target=BUILD_TARGET
                        override SITL build target
    --enable-math-check-indexes
                        enable checking of math indexes
    --force-32bit       compile sitl using 32-bit
    --configure-define=DEFINE
                        create a preprocessor define
    --rebuild-on-failure
                        if build fails, do not clean and rebuild
    --waf-configure-arg=WAF_CONFIGURE_ARGS
                        extra arguments to pass to waf in configure step
    --waf-build-arg=WAF_BUILD_ARGS
                        extra arguments to pass to waf in its build step
    --coverage          use coverage build
    --ubsan             compile sitl with undefined behaviour sanitiser
    --ubsan-abort       compile sitl with undefined behaviour sanitiser and
                        abort on error
    --num-aux-imus=NUM_AUX_IMUS
                        number of auxiliary IMUs to simulate

  Simulation options:
    -I INSTANCE, --instance=INSTANCE
                        instance of simulator
    -n COUNT, --count=COUNT
                        vehicle count; if this is specified, -I is used as a
                        base-value
    -i INSTANCES, --instances=INSTANCES
                        a space delimited list of instances to spawn; if
                        specified, overrides -I and -n.
    -V, --valgrind      enable valgrind for memory access checking (slow!)
    --callgrind         enable valgrind for performance analysis (slow!!)
    -T, --tracker       start an antenna tracker instance
    --enable-onvif      enable onvif camera control sim using AntennaTracker
    --can-peripherals   start a DroneCAN peripheral instance
    -A SITL_INSTANCE_ARGS, --sitl-instance-args=SITL_INSTANCE_ARGS
                        pass arguments to SITL instance
    -G, --gdb           use gdb for debugging ardupilot
    -g, --gdb-stopped   use gdb for debugging ardupilot (no auto-start)
    --lldb              use lldb for debugging ardupilot
    --lldb-stopped      use ldb for debugging ardupilot (no auto-start)
    -d DELAY_START, --delay-start=DELAY_START
                        delay start of mavproxy by this number of seconds
    -B BREAKPOINT, --breakpoint=BREAKPOINT
                        add a breakpoint at given location in debugger
    --disable-breakpoints
                        disable all breakpoints before starting
    -M, --mavlink-gimbal
                        enable MAVLink gimbal
    -L LOCATION, --location=LOCATION
                        use start location from Tools/autotest/locations.txt
    -l CUSTOM_LOCATION, --custom-location=CUSTOM_LOCATION
                        set custom start location (lat,lon,alt,heading)
    -S SPEEDUP, --speedup=SPEEDUP
                        set simulation speedup (1 for wall clock time)
    -t TRACKER_LOCATION, --tracker-location=TRACKER_LOCATION
                        set antenna tracker start location
    -w, --wipe-eeprom   wipe EEPROM and reload parameters
    -m MAVPROXY_ARGS, --mavproxy-args=MAVPROXY_ARGS
                        additional arguments to pass to mavproxy.py
    --scrimmage-args=SCRIMMAGE_ARGS
                        arguments used to populate SCRIMMAGE mission (comma-
                        separated). Currently visual_model, motion_model, and
                        terrain are supported. Usage:
                        [instance=]argument=value...
    --strace            strace the ArduPilot binary
    --model=MODEL       Override simulation model to use
    --use-dir=USE_DIR   Store SITL state and output in named directory
    --no-mavproxy       Don't launch MAVProxy
    --fresh-params      Generate and use local parameter help XML
    --mcast             Use multicasting at default 239.255.145.50:14550
    --udp               Use UDP on 127.0.0.1:5760
    --osd               Enable SITL OSD
    --osdmsp            Enable SITL OSD using MSP
    --tonealarm         Enable SITL ToneAlarm
    --rgbled            Enable SITL RGBLed
    --add-param-file=ADD_PARAM_FILE
                        Add a parameters file to use
    --no-extra-ports    Disable setup of UDP 14550 and 14551 output
    --no-wsl2-network   Disable setup of WSL2 network for output
    -Z SWARM, --swarm=SWARM
                        Specify path of swarminit.txt for shifting spawn
                        location
    --auto-offset-line=AUTO_OFFSET_LINE
                        Argument of form  BEARING,DISTANCE.  When running
                        multiple instances, form a line along bearing with an
                        interval of DISTANCE
    --flash-storage     use flash storage emulation
    --fram-storage      use fram storage emulation
    --enable-ekf2       disable EKF2 in build
    --disable-ekf3      disable EKF3 in build
    --start-time=START_TIME
                        specify simulation start time in format YYYY-MM-DD-
                        HH:MM in your local time zone
    --sysid=SYSID       Set MAV_SYSID
    --postype-single    force single precision postype_t
    --ekf-double        use double precision in EKF
    --ekf-single        use single precision in EKF
    --slave=SLAVE       Set the number of JSON slave
    --auto-sysid        Set MAV_SYSID based upon instance number
    --sim-address=SIM_ADDRESS
                        IP address of the simulator. Defaults to localhost
    --enable-DDS        Enable the dds client to connect with ROS2/DDS
    --disable-networking
                        Disable networking APIs
    --enable-ppp        Enable PPP networking
    --enable-networking-tests
                        Enable networking tests
    --enable-fgview     Enable FlightGear output

  Compatibility MAVProxy options (consider using --mavproxy-args instead):
    --out=OUT           create an additional mavlink output
    --map               load map module on startup
    --mavcesium         load MAVCesium module on startup
    --console           load console module on startup
    --aircraft=AIRCRAFT
                        store state and logs in named directory
    --moddebug=MODDEBUG
                        mavproxy module debug
    --no-rcin           disable mavproxy rcin

  Completion helpers:
    --list-vehicle      List the vehicles
    --list-frame=LIST_FRAME
                        List the vehicle frames

eeprom.bin in the starting directory contains the parameters for your
simulated vehicle. Always start from the same directory. It is recommended
that you start in the main vehicle directory for the vehicle you are
simulating, for example, start in the ArduPlane directory to simulate
ArduPlane
```

## ▶️ Test with SITL Simulator with Gazebo
# NOTE: !!!!

Use the official test script to launch ArduSub with a **6-DOF vectored ROV frame** (supports vertical, lateral, and rotational thrusters — ideal for advanced submarines):

```bash
./Tools/autotest/sim_vehicle.py -v ArduSub -f vectored_6dof --console --map
```

### What to Expect:
- A **console window** showing ArduSub telemetry
- A **map window** (if `--map` is used)
- MAVLink server running on `tcp://127.0.0.1:5760`

### Connect with QGroundControl
1. Download [QGroundControl](https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html)
2. Launch it — it will **automatically connect** to the SITL instance
3. Arm and control your virtual ROV using virtual joysticks or RC input
4. Solve problem of run SITL ArduSub and improtant link information and github is here: https://github.com/peterTPU/ROS-Download/blob/main/Problem%20and%20Solve%20information%20for%20%20STL_ArduSub

---

## 📚 Useful Resources

- 🌐 **Official ArduSub Docs**: https://ardusub.com  
- 🧪 **SITL Guide**: https://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html  
- 💬 **Support Forum**: https://discuss.ardupilot.org  
- 🤝 **GitHub Repo**: https://github.com/ArduPilot/ardupilot  

---

## 🎓 For Instructors & Student Teams

- ✅ This setup enables **full simulation of ROV dynamics**, thruster control, and mission planning.
- 🔗 Combine with **ROS/Gazebo** via **MAVROS** for advanced autonomy projects.
- 🧪 Students can modify control logic, test new frames, or integrate custom sensors in SITL before hardware deployment.


Let me know if you'd like a **PDF version**, a **launch script**, or integration instructions for **ROS + Gazebo + ArduSub**! 🌊🤖
