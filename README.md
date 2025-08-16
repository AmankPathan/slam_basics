# SLAM Basics
## Day 2
### SLAM
Simultaneous Localization and Mapping is the process where a robot builds a map of an unknown environment while estimating its own position within that map, using onboard sensors (e.g., lidar, cameras, IMU).

### 1. Occupancy Grid
Map divided into cells, each storing probability of being occupied, free, or unknown.

Updated with sensor data via Bayes’ rule.

Used for mapping and path planning.

Analogy: Coloring graph paper squares for walls, free space, or uncertainty.

### 2. Loop Closure
Detecting return to a previously mapped location.

Corrects drift by adding loop closure constraints and optimizing the map.

Process: Detection → Constraint addition → Global optimization.

Analogy: Realizing you’re back home and fixing your drawn map.

### 3. Frontend vs Backend
Frontend: Converts sensor data into measurements (features, odometry, data association).

Backend: Optimizes trajectory and map using those measurements.

Analogy: Frontend = photographer, backend = puzzle solver.

### 4. Scan Matching
Aligns new sensor scans (e.g., lidar) with previous scans or the map to estimate motion.

Methods: ICP, Correlative Scan Matching.

Reduces drift and improves local accuracy.

Analogy: Sliding transparent maps until streets align.

### 5. Connection Between Scan Matching and Loop Closure
Scan matching = local alignment; loop closure = global correction.

Good scan matching keeps maps consistent, aiding loop closure detection.

During loop closure, scan matching refines alignment before optimization.

Analogy: Adjust each step on the trail (scan matching) vs redraw whole map at camp (loop closure).

### 6. Importance of Scan Matching for Loop Closure
Refines rough place matches into precise alignments.

Ensures accurate loop closure constraints for backend optimization.

One-liner: “Scan matching sharpens loop closure so the backend can correct the map reliably.”

### 7. Constraints in SLAM
Definition: Relationships between robot poses and/or landmarks from measurements.

Types: Odometry, observation, and loop closure constraints.

Backend uses constraints in graph optimization:

Nodes = poses/landmarks.

Edges = constraints (with uncertainty).

Analogy: Blindfolded friends connected by ropes; optimization pulls them into positions that best fit all rope lengths.

##Day 3
In mobile robotics, the robot needs to know:
Where it is relative to the world.
Where its sensors are located relative to the robot body.
Where detected objects are relative to itself or to the environment.
We express all of these using coordinate frames and transforms.

###1. What is a Frame?
A frame is basically a coordinate system with an origin (0,0,0) and orientation (x, y, z axes).
Example:
map frame → global reference (fixed in the world).
odom frame → robot’s estimated position without global correction (can drift).
base_link frame → attached to the robot’s body center.
laser frame → attached to the LiDAR sensor.
Each frame gives you a way to express positions and orientations of objects.

Frame Diagram Example (simplified):

World (map)
  |
  |  (map -> odom)
  v
Odom frame
  |
  |  (odom -> base_link)
  v
Base_link (robot body)
  |
  |  (base_link -> laser)
  v
Laser frame (LiDAR sensor)

###2. What is a TF (Transform)?
A TF describes the spatial relationship between two frames:
Translation → how far the origin of one frame is from another.
Rotation → how one frame’s axes are oriented relative to another.

Mathematically, a transform is often stored as:
Position vector: (x, y, z)
Orientation: Quaternion or Euler angles (roll, pitch, yaw)

Example:
base_link → laser
Translation: (0.2 m forward, 0.0 m sideways, 0.1 m up)
Rotation: (0°, 0°, 90°)
This means the LiDAR is 20 cm in front of the robot center, 10 cm above, rotated 90°.

###3. TF in ROS (Robot Operating System)
In ROS, tf (or tf2) is a package that:
Maintains a tree of coordinate frames over time.

Allows queries like:
"Where is the laser frame now relative to the map frame?"
"Where was the camera frame 5 seconds ago relative to the base_link?"
Broadcasts transforms using TF broadcasters (from robot state publishers, SLAM, odometry, etc.).

TF Tree Example in a Mobile Robot:
map
 └── odom
      └── base_link
           ├── laser
           ├── camera_link
           └── wheel_left

Each arrow has an associated time-varying transform.

###4. Why Frames and TF are Important
Sensor fusion → Combine LiDAR, camera, IMU data in a common frame.
Navigation → Convert a goal in map frame to wheel commands in base_link frame.
Localization → Match sensor data in its own frame to a map in the global frame.
Manipulation → Know where the robot’s arm/gripper is relative to an object.

###5. Key Practical Notes
Static transforms → Fixed relationships (e.g., base_link → laser).
Dynamic transforms → Change over time (e.g., odom → base_link as robot moves).
Accuracy matters → Bad TF calibration means sensors don’t align and maps become wrong.
Time stamping → TF in ROS is time-aware so transforms correspond to exact sensor timestamps.
