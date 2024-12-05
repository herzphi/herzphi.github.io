---
title: "Water Flow Grid Problem"
search: true
categories: 
  - Open Access
last_modified_at: 2024-10-16T06:36:00-05:00
permalink: /blog/2024-10-16-8th-post
use_math: true
---

Imagine you have a grid where in the top layer at one place a source of water resides. The water can flow through that grid from the top to the bottom. Within the grid at one position an obsticle can be placed. The obsitcles interferes with the flow of water such that it splits the water from the top to the two cells next to each other of the obsticle. Lets simplify by labeling the source for water with `1`and the obsticles with `-1`. 
```
[ 0.  1.  0.  0.]
[ 0. -1.  0.  0.]
[ 0.  0. -1.  0.]
[ 0.  0.  0.  0.]
[ 0.  0.  0.  0.]
```
The logic is now that in the second row the water value of `1` should split equal between the cells next to the obsticle in row two.
```
[ 0.  1.  0.  0.]
[ 1/2 -1.  1/2  0.]
[ 0.  0. -1.  0.]
[ 0.  0.  0.  0.]
[ 0.  0.  0.  0.]
```
The water in row two and column one flows directly to the bottom because there is no obsticle in its way while the water in row two column three has an obsticle beneath it, so we split again.
```
[ 0.  1.  0.  0.]
[ 1/2 -1.  1/2  0.]
[ 0.  1/4 -1. 1/4]
[ 0.  1/4  0.  1/4]
[ 0.  1/4  0.  1/4]
```
This concludes the task. There are two special cases which needs further exploration. The first case is when an obsitcle is placed at the border and when two obsticles are places next to each other in a single row. 

The following code solves this puzzle without considering special cases. We start by defining the grid, where the starting source and obstacles are located. In this case, we have a 5 by 4 grid, with the starting source in row 1, column 1. The first for loop places the obstacles in the grid. Then, we print how the grid looks before we write the flow values. The main part is now done row by row. We start with the first row, look for sources, and check whether the value below the source is -1. Based on this, we set the value in the grid to 1, as long as no obstacle is below the source, or we set the neighboring cells to the obstacles to half the source value.
````
import numpy as np

grid = np.zeros((5, 4))
source = 1
obstacles = [(1, 1), (2, 2)]
grid[0, source] = 1
for obstacle in obstacles:
    grid[obstacle] = -1

print(grid)
sources = [source]
for i, row in enumerate(grid[1:]):
    for s in sources:
        if row[s] != -1:
            row[s] = grid[i, s]
        elif row[s] == -1:
            row[s - 1] = grid[i, s] / 2
            row[s + 1] = grid[i, s] / 2
    sources = np.where(row > 0)[0]
print(grid)

````
