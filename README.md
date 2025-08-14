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
