Here's your script formatted as a clean and well-structured `README.md` file:

```markdown
# ROS Noetic and Development Environment Setup

This guide provides step-by-step instructions to set up **ROS Noetic**, along with commonly used development and productivity tools on a Debian-based Linux system (e.g., Ubuntu 20.04).

---

## 📦 ROS Noetic Installation

```bash
# Add ROS repository
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-l latest.list'

# Install curl and add ROS GPG key
sudo apt install -y curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

# Update package index and install ROS Noetic (Desktop Full)
sudo apt update -y
sudo apt install -y ros-noetic-desktop-full

# Source ROS environment in bashrc
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc

# Install ROS development tools
sudo apt install -y python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential

# Initialize rosdep
sudo apt install -y python3-rosdep
sudo rosdep init
rosdep update
```

### 🧱 Prepare Catkin Workspace

```bash
mkdir -p ~/catkin_ws/src
```

> You can build your packages inside `~/catkin_ws/src` later.

---

## 🖥️ Additional Software

### AnyDesk (Remote Desktop)

```bash
sudo apt update
wget -qO - https://keys.anydesk.com/repos/DEB-GPG-KEY | sudo apt-key add -
echo "deb http://deb.anydesk.com/ all main" | sudo tee /etc/apt/sources.list.d/anydesk-stable.list
sudo apt update
sudo apt install -y anydesk
```

### Visual Studio Code

```bash
sudo apt update -y
sudo apt install -y software-properties-common apt-transport-https wget

# Add Microsoft GPG key and repository
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository -y "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"

sudo apt update -y
sudo apt install -y code
```

### Google Chrome

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install -y ./google-chrome-stable_current_amd64.deb
```

### Terminator (Advanced Terminal Emulator)

```bash
sudo apt-get update -y
sudo apt-get install -y terminator
```
