# MA-360 URDF package (ROS 1)

Generated from the uploaded STEP file `MA360 客户交流图（含电池）25082901.STEP`.

## What is included
- `meshes/ma360_body.stl`: tessellated visual mesh from the STEP geometry.
- `urdf/ma360.urdf`: URDF with visual mesh, simplified box collision and a Gazebo Classic differential-drive plugin skeleton.
- `launch/display_rviz.launch`: RViz model display.
- `launch/gazebo.launch`: Gazebo Classic spawn launch.

## Geometry findings
- STEP was successfully imported as one fused solid by the available CAD importer.
- Bounding-box dimensions from the STEP geometry are approximately 420.5 x 99.41 x 370.4 mm in the source CAD axes. The mesh is rotated to a likely ROS Z-up orientation, giving an envelope of approximately X=420.5, Y=370.4, Z=99.4 mm.
- Mesh coordinates are centered on the bounding-box center and rotated 90 degrees about X to map the likely CAD Y-up axis to ROS Z-up; URDF mesh scale is 0.001 to convert mm to metres.

## Important limitations before using in Gazebo
1. Since the STEP imports as one fused solid, the tracks/wheels are not separated into independently moving links. The wheel joints in this first-pass URDF are placeholders for plugin integration, not confirmed mechanical wheel locations.
2. The wheel separation (0.32 m), wheel diameter (0.12 m), base mass/inertia and collision box are provisional placeholders. Replace with measured/CAD-derived values before physics or navigation validation.
3. The CAD body is a single visual mesh; tracks will not visibly rotate in Gazebo. For realistic simulation, export tracks/drive wheels as separate STEP/STL parts or provide the original assembly STEP with individual components preserved.
4. Magnetic adhesion, wall transitions, track contact/friction, IMU/odometry, and safety logic are not modelled here. A standard differential-drive plugin alone does not simulate safe wall climbing.
5. Gazebo launch assumes ROS 1 with Gazebo Classic and `gazebo_ros`; this is not a ROS 2/Gazebo Harmonic package.
6. Launch files call `xacro`; install `ros-<distro>-xacro`, `gazebo_ros_pkgs`, `robot_state_publisher`, `joint_state_publisher`, and `rviz` as applicable.

## Build
Copy this folder into `~/catkin_ws/src/`, then:
```bash
cd ~/catkin_ws
catkin_make
source devel/setup.bash
roslaunch ma360_urdf_ros1 display_rviz.launch
```
Gazebo Classic:
```bash
roslaunch ma360_urdf_ros1 gazebo.launch
```

The package is a first-pass conversion and should be visually checked in RViz before simulation. Do not use the provisional physics parameters for engineering validation.
