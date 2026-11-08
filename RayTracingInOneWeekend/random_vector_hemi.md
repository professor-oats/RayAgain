 There are many equally valid ways of generating a random vector on a hemisphere using the rejection method, but for our purposes we will go with the simplest, which is:

    Generate a random vector inside the unit sphere
    Normalize this vector to extend it to the sphere surface
    Invert the normalized vector if it falls onto the wrong hemisphere

First, we will use a rejection method to generate the random vector inside the unit sphere (that is, a sphere of radius 1). Pick a random point inside the cube enclosing the unit sphere (that is, where x, y, and z are all in the range [−1,+1]). If this point lies outside the unit sphere, then generate a new one until we find one that lies inside or on the unit sphere. 

Now that we have a random unit vector, we can determine if it is on the correct hemisphere by comparing against the surface normal: 

We can take the dot product of the surface normal and our random vector to determine if it's in the correct hemisphere. If the dot product is positive, then the vector is in the correct hemisphere. If the dot product is negative, then we need to invert the vector.
