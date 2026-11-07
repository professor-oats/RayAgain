 The amount that a refracted ray bends is determined by the material's refractive index. Generally, this is a single value that describes how much light bends when entering a material from a vacuum. Glass has a refractive index of something like 1.5–1.7, diamond is around 2.4, and air has a small refractive index of 1.000293.

When a transparent material is embedded in a different transparent material, you can describe the refraction with a relative refraction index: the refractive index of the object's material divided by the refractive index of the surrounding material. For example, if you want to render a glass ball under water, then the glass ball would have an effective refractive index of 1.125. This is given by the refractive index of glass (1.5) divided by the refractive index of water (1.333).

You can find the refractive index of most common materials with a quick internet search. 

 The refraction is described by Snell’s law:

η⋅sinθ=η′⋅sinθ′

Where θ and θ′ are the angles from the normal, and η and η′ (pronounced “eta” and “eta prime”) are the refractive indices. The geometry is:

Figure 17: Ray refraction

In order to determine the direction of the refracted ray, we have to solve for sinθ′:

sinθ′=ηη′⋅sinθ

On the refracted side of the surface there is a refracted ray R′ and a normal n′, and there exists an angle, θ′, between them. We can split R′ into the parts of the ray that are perpendicular to n′ and parallel to n′:

R′=R′⊥+R′∥

If we solve for R′⊥ and R′∥ we get:

R′⊥=ηη′(R+|R|cos(θ)n)
R′∥=−1−|R′⊥|2−−−−−−−−√n

You can go ahead and prove this for yourself if you want, but we will treat it as fact and move on. The rest of the book will not require you to understand the proof.

We know the value of every term on the right-hand side except for cosθ. It is well known that the dot product of two vectors can be explained in terms of the cosine of the angle between them:

a⋅b=|a||b|cosθ

If we restrict a and b to be unit vectors:

a⋅b=cosθ

We can now rewrite R′⊥ in terms of known quantities:

R′⊥=ηη′(R+(−R⋅n)n)

When we combine them back together, we can write a function to calculate R′:



Total Internal Reflection

One troublesome practical issue with refraction is that there are ray angles for which no solution is possible using Snell's law. When a ray enters a medium of lower index of refraction at a sufficiently glancing angle, it can refract with an angle greater than 90°. If we refer back to Snell's law and the derivation of sinθ′:

sinθ′=ηη′⋅sinθ

If the ray is inside glass and outside is air (η=1.5 and η′=1.0):

sinθ′=1.51.0⋅sinθ

The value of sinθ′ cannot be greater than 1. So, if,

1.51.0⋅sinθ>1.0

the equality between the two sides of the equation is broken, and a solution cannot exist. If a solution does not exist, the glass cannot refract, and therefore must reflect the ray:

if (ri * sin_theta > 1.0) {
    // Must Reflect
    ...
} else {
    // Can Refract
    ...
}

Attenuation is always 1 — the glass surface absorbs nothing.

If we render the prior scene with the new dielectric::scatter() function, we see … no change. Huh?

Well, it turns out that given a sphere of material with an index of refraction greater than air, there's no incident angle that will yield total internal reflection — neither at the ray-sphere entrance point nor at the ray exit. This is due to the geometry of spheres, as a grazing incoming ray will always be bent to a smaller angle, and then bent back to the original angle on exit.

So how can we illustrate total internal reflection? Well, if the sphere has an index of refraction less than the medium it's in, then we can hit it with shallow grazing angles, getting total external reflection. That should be good enough to observe the effect.

We'll model a world filled with water (index of refraction approximately 1.33), and change the sphere material to air (index of refraction 1.00) — an air bubble! To do this, change the left sphere material's index of refraction to

index of refraction of air / index of refraction of water

Schlick Approximation

Now real glass has reflectivity that varies with angle — look at a window at a steep angle and it becomes a mirror. There is a big ugly equation for that, but almost everybody uses a cheap and surprisingly accurate polynomial approximation by Christophe Schlick. This yields our full glass material: 


Modeling a Hollow Glass Sphere

Let's model a hollow glass sphere. This is a sphere of some thickness with another sphere of air inside it. If you think about the path of a ray going through such an object, it will hit the outer sphere, refract, hit the inner sphere (assuming we do hit it), refract a second time, and travel through the air inside. Then it will continue on, hit the inside surface of the inner sphere, refract back, then hit the inside surface of the outer sphere, and finally refract and exit back into the scene atmosphere.

The outer sphere is just modeled with a standard glass sphere, with a refractive index of around 1.50 (modeling a refraction from the outside air into glass). The inner sphere is a bit different because its refractive index should be relative to the material of the surrounding outer sphere, thus modeling a transition from glass into the inner air.

This is actually simple to specify, as the refraction_index parameter to the dielectric material can be interpreted as the ratio of the refractive index of the object divided by the refractive index of the enclosing medium. In this case, the inner sphere would have an refractive index of air (the inner sphere material) over the index of refraction of glass (the enclosing medium), or 1.00/1.50=0.67. 
