# RR-Net
Few-Shot Fine-Grained Image Classification with Residual Reconstruction Network Based on Feature Enhancement

The code will be uploaded after the paper is accepted.

# Franka Panda 机械臂 Xbox 控制系统

本项目提供了两种控制模式，用户可以根据需求选择合适的方式来操作 Franka Panda 机械臂：  

1. **基于 Flask Web 服务的接口** —— 通过 Xbox 手柄远程控制 Franka Panda 机械臂(收集数据)  
2. **本机直连模式** —— 在本机上直接使用 Xbox 手柄控制 Franka Panda 机械臂  

---

## 实验前准备

在使用本项目之前，请确保以下环境配置正确：  

- **硬件准备**  
  - Franka Panda 机械臂  
  - Xbox 手柄（有线或无线）  

- **驱动准备**  
  - 如果使用 **有线 Xbox 手柄**，请确保 Linux 系统中已安装 **xpad 驱动**  
  - 如果使用 **无线 Xbox 手柄**，请确保 Linux 系统中已安装 **xow/xone 驱动**  

### 驱动检查方法：  
```bash
lsmod | grep xpad   # 检查有线驱动是否已加载
lsmod | grep xow    # 检查无线驱动是否已加载
lsmod | grep xone   # 检查无线驱动是否已加载
```
### 驱动启动方法：
```bash
sudo /usr/local/bin/xow
```
### 依赖安装
请先创建并激活虚拟环境 conda activate frantrol

### 🚀 使用方法
### 方式一：Flask Web 服务模式

启动 Flask 服务：
```bash
python web_server.py
```
默认运行在：http://127.0.0.1:7902

等待server启动之后，启动 Xbox 控制桥接：
```python
python xbox2franka_web.py
```

### 方式二：本机直连模式

确认系统识别 Xbox 手柄：
```bash
ls /dev/input/js*
jstest /dev/input/js0
```

启动本地 Teleoperation：
```bash
python Xbox_Teleoperation_Pizza.py
```

### 🎮手柄映射说明：
```bash
按键/摇杆	功能说明
左摇杆	末端位置 X/Y 平移（配合按键 Z 方向）
右摇杆	姿态旋转
LT / RT	配合右摇杆控制绕 X/Y/Z 旋转
A	夹爪开关
B	清除机械臂漂移
X	断开 Xbox 指令，重连机械臂
Y	切断机械臂与 Xbox 控制
```
如需修改控制速度参数，参考data_from_xbox.py，更改CONFIG参数信息。

优点：

延迟低，实时性更好

不依赖网络或 Web 服务

### ⚠️ 注意事项
```bash
使用web模式的时候 SPEED_FACTOR 、SPEED_FACTOR_Z 、SPEED_ROTATION 需要调大至 0.05 ， 0.002
使用直连模式的时候 SPEED_FACTOR 、SPEED_FACTOR_Z 、SPEED_ROTATION 需要调至 0.005 ， 0.0002
如果切换模式的时候没有修改上述值，会导致机械臂过速或者过慢！

确保机械臂周围无障碍物，避免碰撞

Web 服务模式需网络稳定
```
### 📂 项目目录结构
```plaintext
.
├── README.md
├── data_from_xbox.py           # Xbox手柄数据映射
├── web_server.py               # Flask Web 服务模式
├── xbox2franka_web.py          # Web 模式 Xbox 控制桥接
├── Xbox_Teleoperation_Pizza.py # 本机直连控制模式
├── Teleoperation_by_xbox_web   # 手柄数据转换成frankapy goto_pose()参数(Web模式)
├── frankapy_extensions.py      # goto_pose阻抗设置，及ros发布信息
└── Teleoperation_by_xbox.py    # 手柄数据转换成frankapy goto_pose()参数(本机直连控制模式)
```
### 额外测试功能：
```bash
python ~/frankapy/example/go_to_joints_with_joint_impedance_test.py 使用关节控制
```
### 收集数据：
```bash
python camera_service_and_data.py (未测试)
```
# Franka Panda Arm Xbox Control System

This project provides two modes for controlling the **Franka Panda robotic arm** using an **Xbox controller**.  
Users can choose the most suitable mode depending on their needs:

1. **Flask Web Service Mode** — Control the Franka Panda remotely via Xbox controller through a RESTful API  
2. **Direct Local Mode** — Control the Franka Panda directly on the same machine with lower latency  

---

## 🛠 Prerequisites

Before running this project, please ensure the following are properly configured:

### Hardware
- Franka Panda robotic arm  
- Xbox controller (wired or wireless)  

### Drivers
- For **wired Xbox controllers**, ensure the Linux system has the **xpad driver** installed  
- For **wireless Xbox controllers**, ensure the Linux system has the **xow/xone driver** installed  

### Check drivers with:
```bash
lsmod | grep xpad   # Check if wired driver is loaded
lsmod | grep xow    # Check if wireless driver xow is loaded
lsmod | grep xone   # Check if wireless driver xone is loaded
```
### Driver Startup

To start the Xbox wireless driver, run:

```bash
sudo /usr/local/bin/xow
```

### Environment Setup
It is recommended to use a virtual environment:
```bash
conda activate frantrol
```
### 🚀 Usage
### Mode 1: Flask Web Service

Start the Flask server:
```bash
python web_server.py
```

Default address: http://127.0.0.1:7902

Once the server is running, start the Xbox control bridge:
```bash
python xbox2franka_web.py
```
### Mode 2: Direct Local Mode

Confirm that the Xbox controller is recognized:
```bash
ls /dev/input/js*
jstest /dev/input/js0
```
Launch local teleoperation:
```bash
python Xbox_Teleoperation_Pizza.py
```
### 🎮 Controller Mapping
```bash
Button/Stick	Function
Left Stick	End-effector translation in X/Y (combine with triggers for Z)
Right Stick	End-effector orientation control
LT / RT	Rotate around X/Y/Z axes (with Right Stick)
A	Gripper open/close
B	Reset arm drift
X	Stop Xbox input, reconnect arm
Y	Disconnect Xbox control from the arm

To modify control speed parameters, edit CONFIG values in data_from_xbox.py.
```
### ⚡ Speed Parameter Notes
```bash
Web Mode: set data_from_xbox[CONFIG]

    SPEED_FACTOR = 0.05

    SPEED_FACTOR_Z = 0.002

    SPEED_ROTATION = 0.05

Direct Mode: set data_from_xbox[CONFIG] 

    SPEED_FACTOR = 0.005

    SPEED_FACTOR_Z = 0.0002

    SPEED_ROTATION = 0.005
⚠️ If switching between modes without updating these values, the arm may move too fast or too slow.
```
### ⚠️ Safety Notes
```bash
Always ensure the arm’s workspace is free of obstacles to avoid collisions

Web Service mode requires stable network conditions

Test new configurations in low-speed mode first
```
### 📂 Project Structure
```plaintext
.
├── README.md
├── data_from_xbox.py           # Xbox controller input mapping
├── web_server.py               # Flask Web Service mode
├── xbox2franka_web.py          # Xbox to Web bridge
├── Xbox_Teleoperation_Pizza.py # Direct Local Mode
├── Teleoperation_by_xbox_web   # Xbox input → frankapy goto_pose() (Web mode)
├── frankapy_extensions.py      # goto_pose impedance config + ROS publisher
└── Teleoperation_by_xbox.py    # Xbox input → frankapy goto_pose() (Local mode)
```
### Extra Testing Feature
You can also use the built-in **FrankaPy** example function for joint impedance control:

```bash
python ~/frankapy/example/go_to_joints_with_joint_impedance_test.py
```
### Data Collection

You can collect data using the following script (untested):

```bash
python camera_service_and_data.py
```
