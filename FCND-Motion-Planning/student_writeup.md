Implementing My Path Planning Algorithm
1. Set global home position
In `plan_path(`) of `student_motion_planning.py`, I read the first line of `colliders.csv`, parsed lat0 and lon0, and invoked `self.set_home_position(lon0, lat0, 0)`. This ensures the local coordinate frame is fixed to the map origin regardless of spawn location.

2. Set current local position
Immediately after setting home, I converted the drone’s current global position `(self.global_position)` to local NED coordinates using `global_to_local(self.global_position, self.global_home)`.

3. Set grid start position from local position
The grid start is computed as
`grid_start = (int(current_local[0] - north_offset), int(current_local[1] - east_offset))`.
This enables takeoff and planning from any location on the map.

4. Set grid goal position from geodetic coords
I defined a reachable goal 100 m north and 100 m east of home in local meters `(goal_north = 100.0, goal_east = 100.0)`. This position is converted to grid coordinates using the same offset subtraction as the start, guaranteeing an obstacle-free cell in the San Francisco map.

5. Modify A* to include diagonal motion
In `planning_utils.py`, I extended the Action Enum with four diagonal actions (NORTHEAST, NORTHWEST, SOUTHEAST, SOUTHWEST), each assigned cost `np.sqrt(2)`. The `valid_actions()` function was updated to check bounds and occupancy for all eight neighbors. The Euclidean heuristic remains admissible and consistent for an 8-connected grid.

6. Cull waypoints
I implemented `prune_path()` in `planning_utils.py` using a collinearity test based on the cross-product magnitude. Points yielding near-zero area (< 1e-5) are removed, dramatically reducing waypoint count while preserving the geometric path.

Execute the flight
The solution works reliably. The drone arms, plans on the ground, takes off from its actual position, follows a smooth pruned path with diagonal segments, avoids all buildings, reaches the goal approximately 140 m away, and lands. A fallback emergency waypoint prevents empty-path crashes.
