
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
                float2 uv : TEXCOORD0;
                float4 vertex : SV_POSITION;
            };

            sampler2D _MainTex;
            float4 _MainTex_ST;

            v2f vert (appdata v)
            {
                v2f o;
                o.vertex = UnityObjectToClipPos(v.vertex);
                o.col = float4(v.uv, 0, 0);
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

![[Image Sequence_001_0000.jpg]]

In this example, we defined the vertex input struct to have:

1) `float4 vertex` variable with a `POSITION` Semantic: We use this to feed the `UnityObjectToClipPos(vertex);`  so the object can have the correct Clip Space coordinates 
2) `float2 uv` variable with a `TEXCOORD0` Semantic: We used the UV to transform it into color and output that color into the fragment shader, which in turn outputs the pixel colors of the given mesh