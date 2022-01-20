# Simple Setup for Optris Thermal Camera

```bash
cd
mkdir -p optirs_ws/src
cd optris_ws/src/
git clone https://github.com/evocortex/optris_drivers
cd ../..
catkin build
source devel/setup.bash
# Plug in your camera unit for calibration
sudo ir_download_calibration # This will generate config/12090031.xml
roslaunch optris_drivers optris_drivers.launch
```
