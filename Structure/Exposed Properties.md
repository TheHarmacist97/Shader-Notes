Exposed Properties in shaders are those variables which are declared globally in the CG part of the shader and are open to edition via inspector or scripts. These reside in the `Properties{...}` block of the shader file

Example:

```hlsl
Properties
    {
		_MainTex ("Texture", 2D) = "white" {}
        _AmbientLight("Ambient Light", color) = (0,0,0,0)
        [Range(0, 10)]_Gloss("Glossiness", float) = 1
        _Color("Albedo", Color) = (1,1,1,1)
        _SpecSteps("Specular Posterize Steps", Int) = 3
        _DiffSteps("Diffuse Posterize Steps", Int) = 8
    }
```

The Inspectors of Unity render the above text as![[Exposed Properties.png]]

>[!info]- Exposed Property Syntax
>Any exposed property follows this syntax
>`[optional: attribute] _Name("Display Text In Inspector", type name) = default value`
>Check out the various types and modifiers in Shaderlab [here](https://docs.unity3d.com/Manual/SL-Properties.html) 

Now that we can expose globally declared CG properties to the Unity Editor, we can also change them via scripts. Learn more about [[Updating Exposed Properties|modifying exposed properties value here]]