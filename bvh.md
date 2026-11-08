Bounding Volume Hierarchies

This part is by far the most difficult and involved part of the ray tracer we are working on. I am sticking it in this chapter so the code can run faster, and because it refactors hittable a little, and when I add rectangles and boxes we won't have to go back and refactor them.

Ray-object intersection is the main time-bottleneck in a ray tracer, and the run time is linear with the number of objects. But it’s a repeated search on the same scene, so we ought to be able to make it a logarithmic search in the spirit of binary search. Because we are sending millions to billions of rays into the same scene, we can sort the objects in the scene, and then each ray intersection can be a sublinear search. The two most common methods of sorting are to 1) subdivide the space, and 2) subdivide the objects. The latter is usually much easier to code up, and just as fast to run for most models.

The Key Idea

The key idea of creating bounding volumes for a set of primitives is to find a volume that fully encloses (bounds) all the objects. For example, suppose you computed a sphere that bounds 10 objects. Any ray that misses the bounding sphere definitely misses all ten objects inside. If the ray hits the bounding sphere, then it might hit one of the ten objects. So the bounding code is always of the form:

if (ray hits bounding object)
    return whether ray hits bounded objects
else
    return false

Note that we will use these bounding volumes to group the objects in the scene into subgroups. We are not dividing the screen or the scene space. We want any given object to be in just one bounding volume, though bounding volumes can overlap. 

Note that the blue and red bounding volumes are contained in the purple one, but they might overlap, and they are not ordered — they are just both inside. So the tree shown on the right has no concept of ordering in the left and right children; they are simply inside. The code would be:

if (hits purple)
    hit0 = hits blue enclosed objects
    hit1 = hits red enclosed objects
    if (hit0 or hit1)
        return true and info of closer hit
return false


The BVH Node Class

A BVH is also going to be a hittable — just like lists of hittables. It’s really a container, but it can respond to the query “does this ray hit you?”. One design question is whether we have two classes, one for the tree, and one for the nodes in the tree; or do we have just one class and have the root just be a node we point to. The hit function is pretty straightforward: check whether the box for the node is hit, and if so, check the children and sort out any details.

I am a fan of the one class design when feasible. Here is such a class: 

Splitting BVH Volumes

The most complicated part of any efficiency structure, including the BVH, is building it. We do this in the constructor. A cool thing about BVHs is that as long as the list of objects in a bvh_node gets divided into two sub-lists, the hit function will work. It will work best if the division is done well, so that the two children have smaller bounding boxes than their parent’s bounding box, but that is for speed not correctness. I’ll choose the middle ground, and at each node split the list along one axis. I’ll go for simplicity:

    randomly choose an axis
    sort the primitives (using std::sort)
    put half in each subtree

When the list coming in is two elements, I put one in each subtree and end the recursion. The traversal algorithm should be smooth and not have to check for null pointers, so if I just have one element I duplicate it in each subtree. Checking explicitly for three elements and just following one recursion would probably help a little, but I figure the whole method will get optimized later. The following code uses three methods—box_x_compare, box_y_compare, and box_z_compare—that we haven't yet defined. 
