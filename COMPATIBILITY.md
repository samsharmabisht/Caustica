# Compatibility and limits

| Area | Status |
| --- | --- |
| Unity 2022.3.62f3 | Editor and Windows player tested |
| Built-in Render Pipeline | Supported integration |
| Windows / Direct3D 11 | Fresh import, build, and runtime checks passed |
| Unity 2020.3 and Unity 6 | Not certified; a separate import/build test is required |
| URP / HDRP | Requires a separate rendering integration |
| Mobile, WebGL, VR, other graphics APIs | Not certified |
| Orthographic cameras / overlapping surfaces | Not certified |

## Scene requirements

- Use a horizontal water surface with zero rotation and world scale one.
- Use a perspective camera with `WaterCameraRenderer` connected to the surface.
- Keep the camera's three serialized shader references assigned for player builds.
- Submerged geometry needs depth-writing materials for refraction and caustics.
- The example input scripts use Unity's legacy Input Manager. Set Active Input Handling to **Input Manager (Old)** or **Both** when running them.
- The supplied sky reflection uses the procedural atmosphere. An arbitrary HDRI skybox is not automatically sampled.

## Simulation scope

The interactive fields simulate surface waves over finite moving regions. They do not simulate the volume of every water particle. Set Interaction Focus near the boat or player to keep fine detail where it matters.

Buoyancy approximates a hull with collider samples. Adjust the lower hull collider, density, sample count, centre of mass, and response curves for your own model. A decorative open paper mesh is not a watertight collision volume.

Quality presets, reflection resolution/update rate, caustic update rate, ripple resolution, and interaction limits affect cost. Profile on your target hardware before setting a performance budget. The showcase film is an offline frame capture and does not measure live frame rate.
