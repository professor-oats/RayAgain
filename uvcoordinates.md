Constant-color textures use no coordinates. Solid (or spatial) textures use the coordinates of a point in space. Now it's time to make use of the u,v texture coordinates. These coordinates specify the location on 2D source image (or in some 2D parameterized space). To get this, we need a way to find the u,v coordinates of any point on the surface of a 3D object. This mapping is completely arbitrary, but generally you'd like to cover the entire surface, and be able to scale, orient and stretch the 2D image in a way that makes some sense. We'll start with deriving a scheme to get the u,v coordinates of a sphere. 

For spheres, texture coordinates are usually based on some form of longitude and latitude, i.e., spherical coordinates. So we compute (θ,ϕ) in spherical coordinates, where θ is the angle up from the bottom pole (that is, up from -Y), and ϕ is the angle around the Y-axis (from -X to +Z to +X to -Z back to -X).

We want to map θ and ϕ to texture coordinates u and v each in [0,1], where (u=0,v=0) maps to the bottom-left corner of the texture. Thus the normalization from (θ,ϕ) to (u,v) would be:


