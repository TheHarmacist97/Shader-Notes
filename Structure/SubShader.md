A Shader object contains one or more SubShaders. SubShaders let you define different GPU settings and shader programs for different hardware, render pipelines, and runtime settings. Some Shader objects contain only a single SubShader; others contain multiple SubShaders to support a range of different configurations.

Inside the SubShader block, we can:
- Assign a [**LOD**](https://docs.unity3d.com/Manual/LevelOfDetail.html) (level of detail) value to the SubShader, using the `LOD` block. See [assigning a LOD value to a SubShader](https://docs.unity3d.com/Manual/SL-ShaderLOD.html).
- Assign key-value pairs of data to the SubShader, using the `Tags` block. See [ShaderLab: assigning tags to a SubShader](https://docs.unity3d.com/Manual/SL-SubShaderTags.html).
- Add GPU instructions or shader code to the SubShader, using ShaderLab commands. See [ShaderLab: using commands](https://docs.unity3d.com/Manual/shader-shaderlab-commands.html).
- Define one or more Passes, using the `Pass` block. See [ShaderLab: defining a Pass](https://docs.unity3d.com/Manual/SL-Pass.html).
- Specify package requirements using the `PackageRequirements` block. This makes Unity only run the SubShader if the required packages are installed. See [ShaderLab: specifying package requirements](https://docs.unity3d.com/Manual/SL-PackageRequirements.html).

This example code demonstrates the syntax for creating a Shader object that contains a single SubShader, which in turn contains a single Pass.

```hlsl
Shader "Examples/SinglePass"
{
    SubShader
    {
        Tags { "ExampleSubShaderTagKey" = "ExampleSubShaderTagValue" }
        LOD 100

         // ShaderLab commands that apply to the whole SubShader go here. 

        Pass
        {                
              Name "ExamplePassName"
              Tags { "ExamplePassTagKey" = "ExamplePassTagValue" }

              // ShaderLab commands that apply to this Pass go here.

              // HLSL code goes here.
        }
    }
}
```

_Haven't really done anything with SubShader yet, will update this later_
