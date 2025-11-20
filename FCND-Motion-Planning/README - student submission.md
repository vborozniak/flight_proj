# FCND - 3D Motion Planning
![Quad Image](./misc/enroute.png)

This project is a continuation of the Backyard Flyer project where you executed a simple square shaped flight path. In this project I integrated the techniques that were learnt throughout the last several lessons to plan a path through an urban environment.

### Step 1: Set up your Python Environment
Python environment is set and all the relevant packages installed using Anaconda following instructions in [this repository](https://github.com/udacity/FCND-Term1-Starter-Kit)

### Step 3: Clone this Repository
```Cloned repo as per below link
git clone https://github.com/udacity/FCND-Motion-Planning
```
### Step 4: Test setup
Backyard Flyer solution code works as expected and drone can perform the square flight path in the new simulator. 
```
source activate fcnd
python backyard_flyer_solution.py
```
The quad took off, flied a square pattern and landed

### Step 5: Inspect the relevant files
For this project, two scripts provided, `motion_planning.py` and `planning_utils.py`. Found a file called `colliders.csv`, which contains the 2.5D map of the simulator environment. 

### Step 6: Explain what's going on in  `motion_planning.py` and `planning_utils.py`

`motion_planning.py` is basically a modified version of `backyard_flyer.py` that leverages some extra functions in `planning_utils.py`.
 
```sh
source activate fcnd # if you haven't already sourced your Python environment, do so now.
python motion_planning.py
```

Difference between `motion_planning.py` and `backyard_flyer_solution.py` script is in changed waypoints. In motion_planning they are changed to A* path planner that avoids obstacles while preserving the same state machine and flight execution logic. Planning occurs on the ground after arming (PLANNING state). Once planning compelte, the drone takes off and follows the waypoint sequence exactly as in the backyard flyer script.

The plan_path() method provided in `planning_utils.py` creates map of obstacles by loading colliders.csv to account for building positions and dimensions. A* function (a_star()) - Uses only 4-connected actions (N, S, E, W) with cost 1 each. Heuristic - Euclidean distance.

### Step 7: My planner

My planning algorithm is embedded in the code "student_motion_planning.py":

- Loaded the 2.5D map in the `colliders.csv` file describing the environment.
- Discretized the environment into a grid or graph representation.
- Defined the start and goal locations. Read first line of `colliders.csv` → extract lat0, lon0 → call `self.set_home_position(lon0, lat0, 0)`
- Perform a search using A* or other search algorithm. Modified `planning_utils.py`: added 4 diagonal actions (NE, NW, SE, SW) with cost `np.sqrt(2)`; updated `valid_actions()` accordingly
- Used a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
- Returned waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0]). 

### Step 8: Write it up!
Completed a detailed writeup of my solution shown in file `student_write_up.md` wuth working file `student_motion_planning.py`


