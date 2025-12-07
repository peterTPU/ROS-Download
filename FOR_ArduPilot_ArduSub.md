
# 📝 ArduSub Setup Guide for Ubuntu 20.04  
*For ROV/Submarine Simulation with SITL*

This guide walks you through downloading, building, and testing **ArduSub** — the ArduPilot firmware for underwater vehicles (ROVs/submarines) — on **Ubuntu 20.04** using **Software-In-The-Loop (SITL)** simulation.

> ✅ Verified for educational use with student teams.

---

## 🛠️ Step-by-Step Installation

### 1. **Clone the ArduPilot Repository**
```bash
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot/
```

### 2. **Initialize Submodules**
```bash
git submodule update --init --recursive
```

### 3. **Install System Dependencies**
> ⚠️ Note: The correct script path is `Tools/scripts/install_prereqs.sh` (not `environment_install`).

```bash
Tools/scripts/install_prereqs.sh -y
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
```bash
./waf configure --board Pixhawk1
```
## And to compile:
```bash
./waf sub
```
## for test
```bash
./waf configure --board sitl
./waf sub
```

> ✅ This compiles the **ArduSub** firmware for simulation.

---

## ▶️ Test with SITL Simulator

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
