# Summary

[Links](https://github.com/AnoukBV/memo/blob/main/raytracingbasics.md#links)

[The Basics of raytracing](https://github.com/AnoukBV/memo/blob/main/raytracingbasics.md#the-basics-of-raytracing)

------------------------------------------

# Links

### On Raytracing

[*Basic Raytracing*](https://gabrielgambetta.com/computer-graphics-from-scratch/02-basic-raytracing.html)

[*Raytracing in one week-end*](https://raytracing.github.io/books/RayTracingInOneWeekend.html)

[*An Explanation of Basic Ray Tracing*](https://gregorycernera.medium.com/an-explanation-of-basic-ray-tracing-313373c852ac)

[*Ray Tracing Basics*](https://web.cse.ohio-state.edu/~shen.94/681/Site/Slides_files/basic_algo.pdf)

### Mathematic notions

[*PRODUIT SCALAIRE*](https://www.maths-et-tiques.fr/telech/ProduitScal.pdf)

[*LE COURS : Produit scalaire - Première*](https://www.youtube.com/watch?v=dII7myZuLvo)


-----------------------------------------

# The basics of raytracing

Let's take O, the cam or the eye, a fixed viewpoint, **the origin of the coordinate system.**

```
O = (x, y, z)
```

The POV is the fixed camera orientation. 
- it goes towards the direction of the Z axis (!Z)
- up the Y axis (!Y)
- to the right, towards the X axis (!X)

Our frame, of dimensions Vw, Vh, is perpendicular to !Z, at a distance d from O. Its sides are parallel to !X and !Y. **This frame is called the viewport, and whatever we see from or pov/cam/eye/O through the vieport is drawn on our image.**

So d, Vw and Vh determine the angle visible from O, that is to say, the **field of view** (FOV), that can go from 0 to 180 degrees.

If Vw = Vh = d = 1, the FOV is 53 degrees.


A raytracing algorithm can be summarized as follow:
1. Place cam and viewport
For each pixel on image
2. Determine which square on the viewport corresponds to this pixel.
3. Determine the color
4. Paint the pixel correspondingly

The second step is just a simple change of scale:
```
Vx = Cx * Vw/Cw
Vy = Cy *
Vz = d
```

## Tracing rays

The rays originate from the camera. These first rays are called viewing rays. They all originate from O, and they are shot through each part of the viewport, looking for an object to intersect.

### Ray equation
```
P = O + t(V - O)
```
What we are looking for is always the value of P.

Given that V - O = d, the distance between O and the viewpoert, so the vector !D, we can write:
```
P = O + t!D
```
We start at the origin O and advance along the direction of the ray ! by some amount t. **Here t is the only variable.**

### Sphere equation

To tackle the objects hit by our rays, let's accept the sphere as building blocks.

So let's take a sphere whose center is named C, P is a point on its surface, and r the distance between C and P. **The length of the vector from P to C is r (the distance between P and C).
```
|P - C| = r
```

We know the length of a vector is the square root of its dot product with itself ( denoted < !v, !V >). So if 
```
|P - C| = r
sqrt(<P - C, P - C>) = r
< P - C, P - C > = r²
```

**Recap: ray and sphere equations**
```
P = O + t!D
< P - C, P - C > = r²
```

### Ray meets sphere

If we rewrite the sphere eq substituting P with the ray equation, we get:
```
< O + t!D - C, O + t!D - C > = r²

```
After we change O - C with !OC (vector origin towards center of our object). developp the left expression ((a+b)*(a+b)), factorize, and move r² to the left side, we get:
```
t²<!D, !D> + t(2<!CO, t!D>) + <!CO,!CO> - r² = 0
```
which, if we say that
```
a = < !D, !D >
b = 2<!CO, t!D>
c = < !CO, !CO > - r²
```
give us
```
at² + bt + c = 0
```
which is a quadratic expression ! 

The values of parameter t where the ray intersects the sphere  can be found using the quadratic formula:

![Quadform](https://upload.wikimedia.org/wikipedia/commons/7/75/Quadratic_Formula.jpg)

Depending on the value of the discriminant (b² - 4ac), we either get no solutions (the ray doesn't touch the sphere), one (the ray only touchs the sphere at point P), or two (the ray goes through the sphere).

**Once we have found the value of t, we can plug it back into the ray equation, and we finally get the intersection point P corresponding to that value of t.**


## Rendering our first sphere - Pseudocode for basic algorithm

We have a pixel on our image
- We calculate the equivalent on the viewport
- We know the position of the cam (O), so we can express the equation of a ray that starts at O and goes through our point on the viewport.
- We can compute where the ray intersects a sphere, given that we know where that sphere is.

if t = 1, P = O, if t = 1, P = V

if t < 0, P = located behind the camera

So:
- t < 0 -> P behind the cam
- 0 <= t <= 1 -> P between the cam and the viewport
- t > 1 -> P in front of the viewport

We will seek for solutions to t > 1.


Pseudo code:

```
O = (0, 0, 0)
for x = -Cw/2 to Cw/2 {
    for y = -Ch/2 to Ch/2 {
        D = CanvasToViewport(x, y)
        color = TraceRay(O, D, 1, inf)
        canvas.PutPixel(x, y, color)
    }
}
```
CanvasToViewport is just the change of scale function:
```
CanvasToViewport(x, y) {
    return (x*Vw/Cw, y*Vh/Ch, d)
}
```
TraceRay is the main thing, it computs the intersection of the ray with every object seen through the viewport, and returns the color of the sphere at the nearest intersection inside the requested range of t.
```
TraceRay(0, D, t_min, t_max) {
    closest_t = inf (or big big nb)
    closest_sphere = NULL;
    for sphere in scene.spheres {
        t1, t2 = IntersectRaySphere(0, D, sphere)
        if t1 in [tmin, t_max] and t1 < closest_t {
                closest_t = t1
                closest_sphere = sphere
        } 
        if t1 in [tmin, t_max] and t1 < closest_t {
                closest_t = t1
                closest_sphere = sphere
        }                                   //???? shouldn't this be hidden behind the obj?
    }
    if closest_sphere = NULL
        return BACKGROUND_COLOR
    return closest_sphere.color
}
```

IntersectRaySphere solves the quadratic equation
```
IntersectRaySphere(0, D, sphere) {
    r = sphere.radius
    CO = O - sphere.radius

    a = dot(D, D)
    b = 2*dot(CO, D)
    c = dot(CO, CO) - r*r

    discriminant = b*b - 4*a*c
    if discriminant < 0
        return inf, inf

    t1 = (-b + sqrt(discriminant)) / 2*a
    t2 = (-b - sqrt(discriminant)) / 2*a
    return t1, t2
}
```

Let's say this is our sphere:

![GreenBlueRed spheres](https://gabrielgambetta.com/computer-graphics-from-scratch/images/04-simple-scene.png)

The structures would be filled as follow:
```
viewport_size = 1 x 1
projection_plane_d = 1
sphere {
    center = (0, -1, 3)
    radius = 1
    color = (255, 0, 0)  # Red
}
sphere {
    center = (2, 0, 4)
    radius = 1
    color = (0, 0, 255)  # Blueviewport_size = 1 x 1
projection_plane_d = 1
sphere {
    center = (0, -1, 3)
    radius = 1
    color = (255, 0, 0)  # Red
}
sphere {
    center = (2, 0, 4)
    radius = 1
    color = (0, 0, 255)  # Blue
}
sphere {
    center = (-2, 0, 4)
    radius = 1
    color = (0, 255, 0)  # Green
}
```

and the rendering would be:

![Rendering](https://gabrielgambetta.com/computer-graphics-from-scratch/images/raytracer-01.png)
