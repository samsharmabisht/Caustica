# Caustica v0.1.0-preview

The first public preparation build of Caustica: procedural water, interactive ripples, sampled buoyancy, and the Paper Boat Showcase for Unity's Built-in Render Pipeline.

## Download

- **Caustica.unitypackage** — import this into your Unity project; includes editable source, four scenes, three prefabs, setup documentation, and the personal project license.
- **Caustica-PaperBoat-Showcase-1080p.mp4** — 32-second silent showcase, 1920×1080, 30 fps.
- **SHA256SUMS.txt** — file hashes for the distributed package and film.

## Try it

Import the package, then choose **Water Physics → Open Paper Boat Showcase**. For your own scene, use **Create Water in Current Scene** and **Validate Current Setup**.

Select the boat to edit its lift, thrust, steering, drag, wake, and cruise response curves. The four examples separately demonstrate water, caustics, interaction, and the boat.

## Validation and scope

Unity **2022.3.62f3**, **Built-in Render Pipeline**, **Windows Direct3D 11**. Fresh import and Windows build passed with **0 errors and 0 warnings**, followed by a runtime check without script or shader exceptions. Water, boat, rendering, and response-curve checks passed.

This is a preview. Unity 2020.3, Unity 6, URP/HDRP, mobile, WebGL, VR, and other graphics APIs have not been certified. Film playback is not a real-time performance benchmark.

## License

Made by **samxwayout**. Licensed under the custom **Caustica Personal Project License**: personal noncommercial projects are allowed under its terms; commercial use and reusable-asset redistribution require written permission. The license is included in the Unity package and repository.
