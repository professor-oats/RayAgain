A solid (or spatial) texture depends only on the position of each point in 3D space. You can think of a solid texture as if it's coloring all of the points in space itself, instead of coloring a given object in that space. For this reason, the object can move through the colors of the texture as it changes position, though usually you would to fix the relationship between the object and the solid texture.

To explore spatial textures, we'll implement a spatial checker_texture class, which implements a three-dimensional checker pattern. Since a spatial texture function is driven by a given position in space, the texture value() function ignores the u and v parameters, and uses only the p parameter.

To accomplish the checkered pattern, we'll first compute the floor of each component of the input point. We could truncate the coordinates, but that would pull values toward zero, which would give us the same color on both sides of zero. The floor function will always shift values to the integer value on the left (toward negative infinity). Given these three integer results (⌊x⌋,⌊y⌋,⌊z⌋) we take their sum and compute the result modulo two, which gives us either 0 or 1. Zero maps to the even color, and one to the odd color.

Finally, we add a scaling factor to the texture, to allow us to control the size of the checker pattern in the scene. 
