```hlsl
Shader "Unlit/UV Vis"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
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
                float4 col : COLOR;
                float4 vertex : SV_POSITION;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.col = float4(v.normal.xyz,0) * 0.5 + 0.5f;
                return o;
            }

            fixed4 frag (v2f i) : SV_Target
            {
                float4 col = i.col;
                return col;
            }
            ENDCG
        }
    }
}
```


>[!example]- Output
>![[Normals Vis.gif]]

In this example, we defined the vertex input struct to have:

1) `float4 vertex` variable with a `POSITION` Semantic: We use this to feed the `UnityObjectToClipPos(vertex);`  so the object can have the correct Clip Space coordinates 
2) `float3 normal` variable with a `NORMAL` Semantic: We used the surface normals to transform it into color and output that color into the fragment shader, which in turn outputs the pixel colors of the given mesh

and the fragment input struct has:

1) `float4 col : COLOR;`  float4 value defining the color of that fragment(pixel)
2) `float4 vertex : SV_POSITION;` float4 value defining the position of that vertex in clip space

The vertex stage creates v2f structs for the fragment stage. It does this by first transforming all vertices to clip space:

```hlsl
v2f o;
o.vertex = UnityObjectToClipPos(v.vertex);  //standard stuff till here
```

Then we have the line:
```hlsl
o.col = float4(v.normal.xyz,0) * 0.5 + 0.5f;
```

Now, we're converting the surface normals into colors by creating a float4 with the first three components directed by the normals 

We're also offsetting the values by the expression $$ *0.5 + 0.5$$to account for negative surface normals:
Since the values of the components of normals range from $$-1 \leq x \leq +1$$,we can multiply the range by $0.5$
The range then becomes
$$-0.5 \leq x \leq 0.5 $$
further offsetting it by $0.5$, we get $$0 \leq x \leq 1$$so if at a certain vertex, the surface normal is `v.normal = float3(0.36, -0.52, 0.81)` then the color for that fragment would be defined as:

```hlsl
o.col = float4(0.36, -0.52, 0.81, 0) *0.5 + 0.5;
```

which would give us this color:

![[Pasted image 20230722140644.png]]

lastly in the fragment stage: 

```
fixed4 frag (v2f i) : SV_Target
{
	float4 col = i.col;
	return col;
}
```

We just take the vertex input and output the color it had stored


Read about more vertex shader examples [[Vertex Shader|over here]]


>[!note] Other examples
>We can apply the same processes to visualize tangents and binormals
>>[!example]- Tangent
>>Changing the Semantic of the normal float3 from `NORMAL` to `TANGENT` gives us this
>>![[Tangents Vis.gif]] 
