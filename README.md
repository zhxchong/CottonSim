# 🌾 [Sensing and Automation in Agri-System (SAAS) Lab](https://sites.google.com/view/xin-zhang-lab/home)

🏛️ [**School of Environmental, Civil, Agricultural & Mechanical Engineering**](https://engineering.uga.edu/schools/ecam/), [**University of Georgia**](https://www.uga.edu/)  
🏛️ [**Department of Agricultural & Biological Engineering**](https://www.abe.msstate.edu/), [**Mississippi State University**](https://www.msstate.edu/)


---

# ❓ Why do we create this repo?
*This repo is created for accompanying the publication of **"CottonSim: Development of an autonomous visual-guided robotic cotton-picking system in the Gazebo"** (arXiv:2505.05317). [📄 arXiv Link](https://arxiv.org/abs/2505.05317)*

📢 This paper has been **accepted in *Computers and Electronics in Agriculture***.  
Details and the official publication link will be added soon.

---


# 📄 Abstract
Cotton is a major cash crop in the United States, with the country being a leading global producer and exporter. Nearly all U.S. cotton is grown in the Cotton Belt, spanning 17 states in the southern region. Harvesting remains a critical yet challenging stage, impacted by the use of costly, environmentally harmful defoliants and heavy, expensive cotton pickers. These factors contribute to yield loss, reduced fiber quality, and soil compaction, which collectively threaten long-term sustainability. To address these issues, this study proposes a lightweight, small-scale, vision-guided autonomous robotic cotton picker as an alternative. An autonomous system, built on Clearpath’s Husky platform and integrated with the Cotton-Eye perception system, was developed and tested in the Gazebo simulation environment. A virtual cotton field was designed to facilitate autonomous navigation testing. The navigation system used **Global Positioning System (GPS)** and map-based guidance, assisted by an RGB-depth camera and a YOLOv8n-seg instance segmentation model. The model achieved a mean Average Precision (mAP) of **85.2%**, recall of **88.9%**, and precision of **93.0%**. The GPS-based approach reached a **100% completion rate (CR)** within a (5e-6)° threshold, while the map-based method achieved a **96.7% CR** within a 0.25 m threshold. The developed **Robot Operating System (ROS)** packages enable robust simulation of autonomous cotton picking, offering a scalable baseline for future agricultural robotics. CottonSim code and datasets are publicly available on GitHub: [https://github.com/imtheva/CottonSim](https://github.com/imtheva/CottonSim).

---

# 📂 How to use this repo?

### 🗂️ Repository Overview

```text
.
├── catkin_ws/                   # ROS workspace (ROS 1)
│   ├── src/
│   │   ├── camnav/     # Map-based + CV navigation and mask operations for the navigation assistance
│   │   ├── camops/     # Use of left and right camera detections (apply if you want to see the detections - Default `OFF`)
│   │   ├── computervision/   # Control package for the Navigation Assistance
│   │   ├── cotton_env/     # Gazebo worlds, maps, URDFs
│   │   ├── Husky # CLearpath's Husky Control Packages
│   │   ├── husky_manipulation/ # Husky_UR_bringup packages
│   │   ├── husky_viz/        # RViz package
│   │   ├── outdoor_waypoint_nav/    # GPS-based + CV navigation package
│   │   ├── pc2laser/    # Converts Velodyne 3D point cloud into 2D laser scan for navigation
│   │   ├── universal_robot/    # universal robot control description and other default packages
│   │   ├── ur5control/    # testing ur5 control packages for picking and control
│   │   └── ur5-joint-position-control/      # UR5 models joint position control packages
├── datasets/                 # Detection and Segmentation datasets
├── Images/                  # Example images of the environment
├── Trained_weights/         # Trained weights for the cotton boll detection and the scene segmentation
├── Videos/                  # Sample navigation videos mentioned in the manuscript
└── README.md

```

## 🚀 Getting Started

### ⚙️ Prerequisites
- 💻 Ubuntu 20.04+
- 🤖 ROS 1 (Noetic)
- 🏞️ Gazebo (compatible with ROS 1)
- 🐍 Python 3.10+
- 🦾 UR5e ROS drivers (ur_robot_driver - Available here)
    

### 🛠️ Installation
```bash
git clone https://github.com/imtheva/CottonSim.git
cd CottonSim

cd catkin_ws
rosdep install --from-paths src --ignore-src -r -y
catkin_make
source devel/setup.bash

pip install -r requirements.txt

 ```
#### 🏃 Launching the Simulation
***🗺️ Map-based Navigation*** <br/><br/>
Launching Cotton Gazebo world :
  ```bash
  roslaunch cotton_env cotton_world_odom.launch
  ```

Launching navigation:
  ```bash
  roslaunch camnav camnav.launch
  ```

***📡 GPS-based Navigation*** <br/><br/>
Launching Cotton Gazebo world :
  ```bash
  roslaunch cotton_env cotton_world_gps.launch
  ```



Launching navigation based on GPS-coordinates:
  ```bash
  roslaunch outdoor_waypoint_nav send_goals_cv.launch
  ```

### 📝 Common behavior (both approaches) ###

The cotton environment launch in both approaches will:<br/>

- 🌱 Load the cotton field geometry from cotton_env/urdf/cotton_geometry.urdf.xacro

- 🚜 Spawn the Husky robot into the Gazebo world

- 🔦 Initialize passthrough filters (pc2laser)

- 👁️ Launch computer vision (computervision.launch)

- 🧭 Start navigation (camnav, husky_navigation)

- 📊 Open RViz visualization (husky_viz/view_robot_map.launch)


## 🔧 Configuration
- 📂 Placed the trained weights in `Trained_weights/`
- 📂 Placed the datasets in `datasets/`


## 🐞 Troubleshooting
- ❌ *No detections?* Check weights path and topics in `groundmaskops.py` and `skymaskops.py`  
- ❌ *Navigation stuck?* Tune pc2laser, local planner & tolerances
- ❌ *Gazebo crashes?* Check ROS/Gazebo compatibility  

# 📖 How to properly cite us if you find this repo useful?
*To cite this repo in your works, use the following BibTeX entry:*

```bibtex
@article{thayananthan2025cottonsim,
  title={CottonSim: A vision-guided autonomous robotic system for cotton harvesting in Gazebo simulation},
  author={Thayananthan, Thevathayarajh and Zhang, Xin and Huang, Yanbo and Chen, Jingdao and Wijewardane, Nuwan K and Martins, Vitor S and Chesser, Gary D and Goodin, Christopher T},
  journal={Computers and Electronics in Agriculture},
  volume={239},
  pages={110963},
  year={2025},
  publisher={Elsevier}
}
```
