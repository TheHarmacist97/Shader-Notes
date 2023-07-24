
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
                float2 uv : TEXCOORD0;
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
                o.col = float4(v.uv, 0.5, 0);
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
>![[UV Vis.gif]]

In this example, we defined the vertex input struct to have:

1) `float4 vertex` variable with a `POSITION` Semantic: We use this to feed the `UnityObjectToClipPos(vertex);`  so the object can have the correct Clip Space coordinates 
2) `float2 uv` variable with a `TEXCOORD0` Semantic: We used the UV to transform it into color and output that color into the fragment shader, which in turn outputs the pixel colors of the given mesh

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
o.col = float4(v.uv, 0.5, 0);
```

Now, we're converting the UV coordinates into colors by creating a float4 with the first two components directed by the UV 

so if at a certain vertex, the UV values are `v.uv = float2(0.3, 0.7)` then the color for that fragment would be defined as:

```hlsl
o.col = float4(0.3,0.7, 0.5, 0);
```

which would give us this color:

![[Pasted image 20230722122453.png]]

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


#VertexShader 