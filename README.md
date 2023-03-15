# Minimal Reproduction of Godot 4 issue

See [the related Godot Engine issue](https://github.com/godotengine/godot/issues/74940).
This code can be used to reproduce that scenario.

## Using this repro

Steps:

1. Clone this repository
2. Enable debug options to see collisions and shapes
3. Run it in Godot 4 (I used 4.0 on Windows 10)

- Result: ball slides into the polygon
- Expected: ball slightly bouncing on the polygon

Variant: reset the transform `Scale.x` on `CharacterBody2D` to `1`.

- Result: ball bounces off the polygon super fast
- Expected: ball only slightly bouncing on the polygon
