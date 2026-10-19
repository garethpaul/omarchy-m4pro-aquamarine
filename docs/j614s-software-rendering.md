# J614s software rendering checkpoint

This branch is an experimental Omarchy M4 Pro dependency port, based on
Aquamarine v0.15.0. It is not a supported M4 installation release.

On a MacBook Pro M4 Pro (J614s), Hyprland 0.56.2 starts against simpledrm but
Aquamarine repeatedly fails output commits because its DRM file descriptor does
not match an enumerated EGL device. Mesa's GBM platform independently creates a
working OpenGL ES 3.2 llvmpipe context on the same device.

The change tries the existing GBM allocator overload when DRM-to-EGL device
lookup fails. Both renderer initialization and initial format discovery use the
fallback. Successful DRM device lookup keeps its existing path; failure of both
paths remains an initialization failure.

## Verification

- Cross-built unmodified v0.15.0, loaded that exact library on the Mac and
  reproduced the original output failure.
- Cross-built the patched library and confirmed the Mac loaded it.
- Native Aquamarine attachment and output tests passed.
- The patched compositor initialized llvmpipe, committed a new framebuffer to
  the connected 3024x1964 display, passed configuration reload/error checks and
  mapped a visible Foot Wayland terminal. The owner confirmed a graphical
  desktop on the physical panel.

This uses CPU rendering. M4 GPU acceleration and physical input are not qualified
by this test. Native default boot and the complete Omarchy installation remain
separate work.
