```hlsl
Shader "Unlit/VertexSkew"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        _Speed("Animation Speed", float) = 1.0
        _Frequency("Skew Frequency", float) = 1.0
        _Scale("Skew Scale", float) = 1.0
    }
    SubShader
    {
        Tags { "RenderType"="Opaque" }

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            #include "UnityCG.cginc"

            struct appdata
            {
                float4 vertex : POSITION;
                float3 normal : NORMAL;
            };

            struct v2f
            {
                float4 vertex : SV_POSITION;
                float4 col : COLOR;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;
		    float _Speed, _Frequency, _Scale;

            v2f vert (appdata v)
            {
                v2f o;
                float4 inVert = 0;
                inVert = UnityObjectToClipPos(v.vertex);
                inVert.x += sin(_Time.y * _Speed + (v.vertex.y * _Frequency))*_Scale;
                o.vertex = inVert;
                o.col = float4(v.normal, 0.0)*0.5+0.5;
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                // sample the texture
                fixed4 col = i.col;
                return col;
            }
            ENDCG
        }
    }
}
```

>[!example]- Output
>![[Wavy.gif]]
>
>![[Wavy UV.gif]]

Now that we have completed surface normal visualization, we can move a step further and discuss actual vertex transformation.

In this example, we are declaring (and exposing) 3 new variables; namely `_Speed`, `_Frequency` and `_Scale`. These variables will be helping us tweak the output of the vertex stage.

In the vertex stage, we create a `float4 inVert` and give it the clip space coordinates. This gives us some room to apply transformations before we pack it up in the `v2f` struct and send it to the fragment stage.

The next line is what brings about the wavy effect:

`inVert.x += sin(_Time.y * _Speed + v.vertex.y * _Frequency)*_Scale;`

`_Time.y` : Unity will provide shaders with a time value internally; it is a float4 and the values are:  `(t/20, t, t*2, t*3)`, where t is time since level load.

`_Speed` : Will control the speed of the waves, exposed as "Animation Speed" in the editor.

`v.vertex.y` : y component of object space (local) coordinates, brings about a phase difference in the sin function value by offsetting it's final argument, which in turn creates waves.

`_Frequency` : Controls the frequency of the waves; a higher value will mean tighter waves. Exposed to editor as "Skew Frequency".

`_Scale` : Controls the amplitude of the waves, a higher value will mean greater deviations. Exposed to editor as "Skew Scale".

>[!tip] 
>Note that we did not use the clip space coordinates (`inVert`) to bring about a phase difference. We avoided clip space coordinates because they change according to **Camera View Vector and Object Position**. Using Clip space coordinates will create rapid warps in the object when the view vector or position of the object changes even slightly, which is unintended.

lastly in the fragment stage: 

```
fixed4 frag (v2f i) : SV_Target
{
	float4 col = i.col;
	return col;
}
```

We just take the vertex input and output the color it had stored (surface normals)


>[!note] Other examples
>Try to replace the `v.vertex.y` phasing elements with other variables related to the mesh
> - `v.vertex.x` or `v.vertex.w`
> - We can also take vertex ID, (Semantic : `SV_VERTEXID`) and divide it by total vertex count to create a smooth progressive gradient
> - `length(v.vertex.xyw)`

#VertexShader