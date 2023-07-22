The **Vertex Shader** is the programmable Shader stage in the rendering pipeline that handles the processing of individual vertices.

Basically, it runs on every available vertex on the mesh, and then runs transformations on the vertices. Usually it converts vertices from Object (local) positions to Clip Positions in the camera frustum.

The Vertex Shader inputs can be tweaked, as discussed in [[Shader Structure Definitions]]
If you want to access different vertex data, you have to declare vertex structure yourself, or add input parameters to the vertex shader. Vertex data is identified by Cg/HLSL semantics, and has to be from the following list:

- POSITION is the vertex position, typically a float4.
- NORMAL is the vertex normal, typically a float3.
- TEXCOORD0 is the first UV coordinate, typically a float2..float4.
- TEXCOORD1 .. TEXCOORD3 are the 2nd..4th UV coordinates.
- TANGENT is the tangent vector (used for normal mapping), typically a float4.
- COLOR is the per-vertex color, typically a float4.

We'll now discuss multiple examples that pertain to vertex shaders and their inputs

Examples
1) [[UV visualization]]
2) [[Normal visualization]]
3) [[Vertex Skewing]]
