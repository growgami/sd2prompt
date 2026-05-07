---
name: Screen / Monitor Orientation
category: continuity
tags: [screen, monitor, ui, gotcha, orientation]
tested: true
source: production-run
---

# Screen / Monitor Orientation

When a monitor or screen appears at an angle or with its back to the camera.

## The Problem

Without explicit direction, Seedance renders screen content on the wrong side of the monitor. You get a screen facing away from camera but with UI visible on the back.

## Fix

Explicitly state which side of the screen the camera can see:

```
monitor faces away from camera, only back of screen visible
```

## General Rule

Specify which side of any screen/display the camera can see. This applies to monitors, phones, tablets, TVs, any screen in scene.
