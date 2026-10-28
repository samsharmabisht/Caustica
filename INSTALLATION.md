# Caustica

Procedural ocean rendering, surface ripples, buoyancy, planar reflections, underwater absorption, and procedural caustics for Unity's **Built-in Render Pipeline**.

## Start here

1. Import `Caustica.unitypackage` into a Built-in project.
2. Choose **Water Physics > Open Water Only Scene** and press Play.
3. For your own scene, choose **Water Physics > Create Water in Current Scene**. This creates the surface and connects a camera with serialized shader references.
4. Choose **Water Physics > Validate Current Setup** to check surface, camera integration, and missing scripts.

`Examples/Scenes/WaterPhysicsOnly.unity` contains only water, a camera, and a directional light. `WaterCausticsPreview` adds a sand receiver. `WaterInteractionLab` demonstrates floating bodies and particle rain.

## Paper boat showcase

Choose **Water Physics > Open Paper Boat Showcase**. The supplied Paper Boat is a rescaled and oriented version of the team's origami model with a two-sided fibre paper material and fold edges. Its original FBX remains in `Examples/Models`. The reusable boat prefab is in `Examples/Prefabs`.

The five scene roots describe their purpose: Water Surface, Paper Boat, Showcase Camera, Sun Light, and Sand Bed - Caustics Receiver. The boat floats with a lower hull collider and sampled buoyancy; its visual folds do not act as a solid collision volume.

Select **Paper Boat** to adjust the following curve graphs in the Inspector:

- **Buoyancy > Lift By Submergence**: submerged fraction to lift fraction. Keep endpoints at (0,0) and (1,1).
- **Boat Controller > Thrust By Speed**, **Steering By Speed**, and **Drag By Speed**: normalized speed to response. Set Mode to Drift, Player, or Autopilot. Player mode uses WASD/arrow keys.
- **Boat Controller > Cruise Over Lap**: route progress to throttle multiplier. Match the endpoints for a continuous lap.
- **Body Wake > Wake By Speed**: normalized contact speed to wake strength. Reference Speed sets the curve's X=1 speed.
- **Showcase Camera > Camera Ease**: normalized shot time to camera movement. Disable Animate Camera for your own camera.

The water Inspector also shows an approximate wave-energy envelope. Boat speed, wet fraction, and ripple timing are shown during Play. Curves are simulation controls, not a guarantee of physical validity for arbitrary values. Sample count, density, collider shape, centre of mass, and the curve endpoints still matter.

All source scripts carry `Made by samxwayout`. Inspector attribution is a label rather than an editable field. Editable source code allows a recipient to change comments and labels; an immutable attribution cannot be guaranteed in a source package.

## Scene integration

The creation menu connects an existing camera, or creates the supplied Water Camera when none exists. You can also drag `Prefabs/Water Surface.prefab` into a scene and add `WaterCameraRenderer` to the viewing camera manually. Assign its Water field and all three shaders: WaterCameraComposite, WaterCausticFocus, and WaterCausticReceiver. These references keep the shaders in player builds.

Keep the water horizontal, with zero rotation and world scale one. Change its height with Transform Y. The Water layer excludes the surface from its own reflection. Use a perspective camera with Forward rendering. Opaque submerged objects need depth-writing shaders for refraction and caustics.

For the supplied appearance, assign `Materials/Sky.mat` to the scene skybox and a directional light to Render Settings > Sun. Sky reflections use this procedural atmosphere; arbitrary HDRI skyboxes are not automatically sampled. The creation menu preserves existing skybox and lighting settings.

## Interactions

- Add a Rigidbody, collider, and `Buoyancy` to a floating object. Density below 1000 floats; higher values sink. Shape samples control the buoyant force.
- Nearby rigidbodies receive `RippleCreator` automatically during Play. Airborne and deeply submerged bodies do not continually paint surface wakes. No water trigger collider is required.
- Particle systems crossing the surface receive `WaterParticleInteractor` automatically. Add it explicitly to tune strength and particle limits. The particle collision module is not required. One crossing creates one impact; Consume On Contact removes that particle.
- Set `WaterRipples.InteractionFocus` to a player or boat to keep detailed interactions near it. Fine and broad simulation fields have finite extents. This is a surface wave solver, not a fluid simulation of every water molecule.
- Call `WaterRipples.AddImpulse(worldPosition, radiusMetres, heightMetres)` from gameplay code. Use `GetSurfaceSample` for water height and normal. For a distant pointer interaction, call `FocusInteraction` before adding the impulse.

The optional scripts in `Examples/Scripts` provide mouse disturbances and demo camera controls. They use Unity's legacy Input Manager: set Active Input Handling to **Input Manager (Old)** or **Both**. Core rendering and physics do not require the demo input scripts.

## Example controls

Hold right mouse to look; WASD moves; Q/E changes height; Shift moves faster; Home resets the camera. Hold left mouse over water to disturb it. In Interaction Lab, drag a float, press Space to drop a sphere, and R to toggle rain.

## Performance and compatibility

Start with Balanced quality. Low reduces geometry and refraction resolution and disables planar geometry reflections. Reflection scale, distance, update rate, caustic update rate, ripple resolution, and body/particle limits affect cost. The ripple solver uses a worker thread, with a main thread fallback on platforms without threads.

The code uses APIs available in Unity 2020.3 and later. The tested editor and Windows player are **Unity 2022.3.62f3, Direct3D 11**. Unity 2020.3, Unity 6, WebGL, mobile, VR, orthographic cameras, and multiple overlapping surfaces have not been certified. URP and HDRP require separate rendering integrations. Compatibility with every Unity release is not claimed.

## Folder guide

- `Runtime`: reusable rendering, simulation, contact, and buoyancy scripts.
- `Shaders`: water, sky, caustics, underwater composite, spray, and shared includes.
- `Materials` and `Prefabs`: reusable surface and camera setup.
- `Editor`: creation menu, setup validation, and water presets.
- `Examples`: three scenes, sample input, materials, and a sand shader. Keep this folder to use the examples; omit it for a runtime-only distribution.

The mesh, capillary detail, ripple textures, reflections, and caustic atlases are generated by the system. No terrain, imported normal textures, ColorPicker, Standard Assets, or old asset-pack demos are required.

## Personal project license

Caustica is distributed under the included `LICENSE.txt`. Personal noncommercial use and modifications are permitted under its terms. Keep the samxwayout source credit. Commercial use and redistribution as a reusable asset require written permission.
