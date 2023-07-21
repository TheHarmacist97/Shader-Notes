## `#pragma` Directives

1) `#pragma vertex vert` - Direction to compile `vert` as the vertex shader
2) `#pragma fragment frag` - Direction to compile `frag` as the geometry shader

The words `vert` and `frag` are meant to be user defined and can be changed, you just need to define vertex shader and fragment shader with the names you declared earlier

#### Other `#pragma` Directives also exist; they are as follows:

- `#pragma geometry name` - compile function `name` as DX10 geometry shader. Having this option automatically turns on `#pragma` target 4.0.
- `#pragma hull name` - compile function `name` as DX11 hull shader. Having this option automatically turns on `#pragma target 5.0`.
- `#pragma domain name` - compile function `name` as DX11 domain shader. Having this option automatically turns on `#pragma target 5.0`.

>[!example]- Miscellaneous Pragma Directives
>- `#pragma target _name_` - which shader target to compile to. See _Shader targets_ below for details.
>- `#pragma only_renderers _space separated names_` - compile shader only for given renderers. By default shaders are compiled for all renderers. See _Renderers_ below for details.
>- `#pragma exclude_renderers _space separated names_` - do not compile shader for given renderers. By default shaders are compiled for all renderers. See _Renderers_ below for details.- `#pragma multi_compile …`_ - for working with [multiple shader variants](https://dev.rbcafe.com/unity/unity-5.3.3/en/Manual/SL-MultipleProgramVariants.html).
>- `#pragma enable_d3d11_debug_symbols` - generate debug information for shaders compiled for DirectX 11, this will allow you to debug shaders via Visual Studio 2012 (or higher) Graphics debugger.
