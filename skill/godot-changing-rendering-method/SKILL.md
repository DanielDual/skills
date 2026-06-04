---
name: godot-changing-rendering-method
description: "If errors like below occurs, you should read this skill. 

```log
_initialize_device: D3D12CreateDevice failed with error 0x887a0004.

initialize: Condition \"err != OK\" is true. Returning: ERR_CANT_CREATE

initialize: Failed to initialize driver for device.

DisplayServerWindows: Your video card drivers seem not to support Direct3D 12 or Vulkan, switching to OpenGL 3.
```"
---

# Godot: Changing the Rendering Method

## Documentation

[String]() **rendering/renderer/rendering_method** = `"forward_plus"`

Sets the renderer that will be used by the project. Options are:

**forward_plus** (Forward+): High-end renderer designed for desktop devices. Has a higher base overhead, but scales well with complex scenes. Not suitable for older devices or mobile.

**mobile** (Mobile): Modern renderer designed for mobile devices. Has a lower base overhead than Forward+, but does not scale as well to large scenes with many elements.

**gl_compatibility** (Compatibility): Low-end renderer designed for older devices. Based on the limitations of the OpenGL 3.3 / OpenGL ES 3.0 / WebGL 2 APIs. Lighting calculations are performed on nonlinear sRGB-encoded color data, which produces inaccurate results that may look acceptable for some games.

This can be overridden using the `--rendering-method <method>` command line argument.

**Note:** The actual rendering method may be automatically changed by the engine as a result of a fallback, or a user-specified command line argument. To get the actual rendering method that is used at runtime, use [RenderingServer.get_current_rendering_method()]() instead of reading this project setting's value.

---

[String]() **rendering/renderer/rendering_method.mobile** = `"mobile"`

Override for [rendering/renderer/rendering_method]() on mobile devices.

---

[String]() **rendering/renderer/rendering_method.web** = `"gl_compatibility"`

Override for [rendering/renderer/rendering_method]() on web.

## When to change

**DO NOT** change the rendering method, unless the error below occurs:

```log
_initialize_device: D3D12CreateDevice failed with error 0x887a0004.

initialize: Condition "err != OK" is true. Returning: ERR_CANT_CREATE

initialize: Failed to initialize driver for device.

DisplayServerWindows: Your video card drivers seem not to support Direct3D 12 or Vulkan, switching to OpenGL 3.
```

If this occurs, you should change `rendering/renderer/rendering_method`, `rendering/renderer/rendering_method.mobile`, `rendering/renderer/rendering_method.web` all to `"gl_compatibility"` and restart the godot editor.