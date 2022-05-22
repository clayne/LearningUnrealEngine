2020-04-11_08:59:35

# Lighting

## Types of light sources

Unreal Engine has a few different types of lights.
- Point
    Emanates from a single point in all directions.
- Spot
    Directional and has an orientation.
    The shape is defined by two cones, one inner and one outer.
    The inside of the inner code is fully lit/has full light intensity.
    The shell between the inner cone and the outer cone has fading intensity from the inner to the outer.
    Outside the outer cone there is no light.
 - Directional
    Simulates sunlight, an infinite volume of parallel light rays.
    Moving a directional light has no effect, but rotating does change the light's angle of attack.
- Sky
    Provides scene-specific ambient light using a cube map, either as a texture or as a scene capture.

## Light properties

- Intensity [cd]: Luminous intensity.
    The "density" of the light. How much light energy there is per unit solid angle. How strong each light ray is.
- Color: The color of the light.
- Attenuation radius [cm]: Culling distance.
    Objects farther than the attenuation radius away will ignore this light. Has no effect on objects well within the attenuation radius. There seems to be some form of interpolation near the edge of the attenuation radius.

## Baked lights

Lighting can be baked into so-called lightmaps.
It's a packaging/cooking stage that does the lighting calculation and stores it in textures.
This can give high quality lighting, but it can't changes to the scene.
Move an object in Unreal Editor and all baked light we act as-if the object was still at the old location.
No shadows move.
The editor will inform you about this with the `LIGHTING NEEDS TO BE REBUILT` message in the top-left of the Level Editor viewport.

[[2022-03-15_16:22:35]] [Building lighting.md](./Building%20lighting.md)

## Light mobility

- Static: Cannot move or change intensity or color during runtime.
- Stationary: Cannot move. Can change intensity and color. Will bake shadows.
- Movable: Can move and change color and intensity during runtime.

The default mobility is Stationary.

Light mobility can have a large impact on performance, so chose the most restrictive possible for each light.

Non-dynamic lighting must be built.
This is done from the Level Editor Tool Bar.

Some types of lights can be either be inverse square falloff or not.
Inverse square falloff is more realistic but requires large luminance and radius values.
Linear square falloff (which I assume the alternative is) can be easier to control.

[[2020-04-11_08:47:51]] [~Collection Lighting](./~Collection%20Lighting.md)  
[[2021-09-12_20:04:15]] [Light scenarios](./Light%20scenarios.md)  


[# Unreal Engine 4 Tutorial - Game Art - Lighting - How it Works by Ryan Laley @ youtube.com](https://www.youtube.com/watch?v=32r28R-ktDA)