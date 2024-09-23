# Minimal Reproduction of Godot 4 issue

See [the related Godot Engine issue](https://github.com/godotengine/godot/issues/74940).
This code can be used to reproduce that scenario.

**⚠ That issue has since been closed.**
It was deemed "_user error_" and "_wrong expectations [from the developer]_", not a bug or missing documentation or tooling.
Because I feel others might run into these same gotchas as I did, I will leave up this repository.
**Before the info on how to _reproduce_ the situation, let me _start_ with what I found out and how I worked around it.**

## ⚠️ Notice about updates

Note that this repository is provided "as-is" and will most likely not receive any (security) updates.

## Root cause and solution/workaround

The root cause of the bug summarized from [the more extensive reply](https://github.com/godotengine/godot/issues/74940#issuecomment-1470369683) is (paraphrased):

> Combining `Control` and `Node2D` nodes can give the expectation of them not having special interaction, but sometimes they might break when combined.

In short: the advice there and [from the other Contributor](https://github.com/godotengine/godot/issues/74940#issuecomment-1470309153) is to not mix them at all.

Later I found that there's a `Control` node called [`SubViewportContainer`](https://docs.godotengine.org/en/stable/classes/class_subviewportcontainer.html#class-subviewportcontainer) which can hold a `SubViewport` which seems _meant_ for my scenario:
layout with Control notes, and part of that layout dedicated to 2D physics.

You can check [my other sample repository](https://github.com/jeroenheijmans/sample-godot-drag-drop-from-control-to-node2d) to see how that would work.

## Issue demonstrated by this repository

Steps:

1. Clone this repository
2. Enable debug options to see collisions and shapes
3. Run it in Godot 4 (I used 4.0 on Windows 10)

- Result: ball slides into the polygon
- Expected: ball slightly bouncing on the polygon

Variant: reset the transform `Scale.x` on `CharacterBody2D` to `1`.

- Result: ball bounces off the polygon super fast
- Expected: ball only slightly bouncing on the polygon

Videos of the unexpected and expected results:

- ❌ [repro-godot-bounce-bug-unexpected-result.mp4](repro-godot-bounce-bug-unexpected-result.mp4)
- ❌ [repro-godot-bounce-bug-unexpected-result-variant.mp4](repro-godot-bounce-bug-unexpected-result-variant.mp4)
- ✔ [repro-godot-bounce-bug-expected-result.mp4](repro-godot-bounce-bug-expected-result.mp4)

Or, for as long as the links work (since the raw videos were uploaded via Godot's issues, which may get archived):

----

Video of the ❌ unexpected result with the base repro:

https://user-images.githubusercontent.com/1590536/225328868-fae41a2c-fff4-4888-9b3a-d1f2f3949c34.mp4

Video of the ❌ unexpected _variant_ with non-flipped CharacterBody2D polygon:

https://user-images.githubusercontent.com/1590536/225336577-0f4b7942-8ca5-4c28-96e1-6d535c5be60a.mp4

Video of the expected result (after removing `ColorRect` from the aforementioned tree):

https://user-images.githubusercontent.com/1590536/225329993-e2e41067-0dd3-4394-a29b-7db113f66cfc.mp4

----

## Closing remarks

I'm _learning_ Godot at the time of writing so don't take all this as expert advice.
Still, I'm hoping it may help someone.
