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

My planning algorithm is embedded in the code "submission_motion_planning.py":

- Load the 2.5D map in the `colliders.csv` file describing the environment.
- Discretize the environment into a grid or graph representation.
- Define the start and goal locations. You can determine your home location from `self._latitude` and `self._longitude`. 
- Perform a search using A* or other search algorithm. 
- Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
- Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0]). 

Some of these steps are already implemented for you and some you need to modify or implement yourself.  See the [rubric](https://review.udacity.com/#!/rubrics/1534/view) for specifics on what you need to modify or implement.

### Step 8: Write it up!
When you're finished, complete a detailed writeup of your solution and discuss how you addressed each step. You can use the [`writeup_template.md`](./writeup_template.md) provided here or choose a different format, just be sure to describe clearly the steps you took and code you used to address each point in the [rubric](https://review.udacity.com/#!/rubrics/1534/view). And have fun!

## Extra Challenges
The submission requirements for this project are laid out in the rubric, but if you feel inspired to take your project above and beyond, or maybe even keep working on it after you submit, then here are some suggestions for interesting things to try.

### Try flying more complex trajectories
In this project, things are set up nicely to fly right-angled trajectories, where you ascend to a particular altitude, fly a path at that fixed altitude, then land vertically. However, you have the capability to send 3D waypoints and in principle you could fly any trajectory you like. Rather than simply setting a target altitude, try sending altitude with each waypoint and set your goal location on top of a building!

### Adjust your deadbands
Adjust the size of the deadbands around your waypoints, and even try making deadbands a function of velocity. To do this, you can simply modify the logic in the `local_position_callback()` function.

### Add heading commands to your waypoints
This is a recent update! Make sure you have the [latest version of the simulator](https://github.com/udacity/FCND-Simulator-Releases/releases). In the default setup, you're sending waypoints made up of NED position and heading with heading set to 0 in the default setup. Try passing a unique heading with each waypoint. If, for example, you want to send a heading to point to the next waypoint, it might look like this:

```python
# Define two waypoints with heading = 0 for both
wp1 = [n1, e1, a1, 0]
wp2 = [n2, e2, a2, 0]
# Set heading of wp2 based on relative position to wp1
wp2[3] = np.arctan2((wp2[1]-wp1[1]), (wp2[0]-wp1[0]))
```

This may not be completely intuitive, but this will yield a yaw angle that is positive counterclockwise about a z-axis (down) axis that points downward.

Put all of these together and make up your own crazy paths to fly! Can you fly a double helix?? 
![Double Helix](./misc/double_helix.gif)

Ok flying a double helix might seem like a silly idea, but imagine you are an autonomous first responder vehicle. You need to first fly to a particular building or location, then fly a reconnaissance pattern to survey the scene! Give it a try!
