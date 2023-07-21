A Pass block inside a SubShader looks something like this

```hlsl
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
		float2 uv : TEXCOORD0;
		float4 vertex : SV_POSITION;
	};

	sampler2D _MainTex;
	float4 _MainTex_ST;

	v2f vert (appdata v)
	{
		v2f o;
		o.vertex = UnityObjectToClipPos(v.vertex);
		return o;
	}

	fixed4 frag (v2f i) : SV_Target
	{
		// sample the texture
		fixed4 col = tex2D(_MainTex, i.uv);
		return col;
	}
	ENDCG
}
```

# Line By Line Breakdown:-

- `CGPROGRAM` : Token for defining the start of HLSL cg code, almost all of the magic happens here
- [[Pragma Directives]]
- `#include` File Inclusion Directives: 
	- The `UnityCG.cginc` file is included below the `#pragma` directives. It contains a lot of important helper functions. example: `UnityObjectToClipPos(vertex);` this helper function warps the vertex of the target objects to fit onto the frustum clip space of the camera
-  [[Shader Structure Definitions]]
- Global properties (see also...[[Exposed Properties]])
- [[Vertex Shader]]
- [[Fragment Shader]]
- `ENDCG` Token for defining end of HLSL cg code
