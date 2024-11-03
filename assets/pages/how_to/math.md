# Transform 2D data in one-dimensional array via indexing with i and j for loops
This was used when we need to flip ROS images which are stored in a 1D array.  
The images are 2D and have a height, width, and i and j are based in that 2D space.

Flip horizontally
new_array[(W - j) + (i * W) - 1] = old_array[(i * W) + j]

Flip vertically
new_array[(H - i - 1) * W + j] = old_array[(i * W) + j]

Flip both vertically and horizontally:
new_array[(H * W) - (i * W) - j - 1] = old_array[(i * W) + j]