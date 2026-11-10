 Pause a bit here and consider that if you use the (α,β) coordinates to determine if a point lies inside a quadrilateral (parallelogram), it's not too hard to imagine using these same 2D coordinates to determine if the intersection point lies inside any other 2D (planar) primitive!

For example, suppose we change the is_interior() function to return true if sqrt(a*a + b*b) < r. This would then implement disk primitives of radius r. For triangles, try a > 0 && b > 0 && a + b < 1.

We'll leave additional 2D shape possibilities as an exercise to the reader, depending on your desire to explore. You could even create cut-out stencils based on the pixels of a texture map, or a Mandelbrot shape! As a little Easter egg, check out the alternate-2D-primitives tag in the source repository. This has solutions for triangles, ellipses and annuli (rings) in src/TheNextWeek/quad.h

