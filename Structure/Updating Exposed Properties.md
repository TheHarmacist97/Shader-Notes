Parameters controlling the output of a shader in Unity can be controlled by referencing the materials attached to them. Commonly, we update these properties, although a bunch of others also exist:
1) Texture
2) Float
3) Vector
4) Color


We can update exposed properties of a material in a total of 3 ways :

1) **Material Asset Update** : Updating the property value in either the Unity Editor or by updating the default values of the Material Asset by using `Material.Set<Property>` : 

```csharp
public class VertexCount : MonoBehaviour
{
    [SerializeField] private Material objectMat; // Set in editor
    void Start()
    {
        Mesh mesh = GetComponent<MeshFilter>().mesh
        objectMat.SetFloat("_VertexCount", mesh.vertexCount);
    }
}
   ``` 
   
   
   This method will not create another instance, and the value update will be applied to every object accessing this material (every object that has not overridden the same value by instancing the material). You can also use `Renderer.sharedMaterial` to get the shared material from an object
   
   ![[Global Material Update.gif]]

   
2) **Material Instance Update** : Updating the property value by instancing the material for that object will keep the changes localized. We still use  `Material.Set<Property>` but we get the reference to the material in a different way :
   
```csharp
private void OnValidate()
    {
        if (objectMat != null)
        {
            objectMat.SetColor("_Color", color);
        }
        else
        {
            objectMat = GetComponent<Renderer>().material;
        }
        
    }
```

![[Instanced Material Update.gif]]

3) **Using Material Property Block** : Use it in situations where you want to draw multiple objects with the same material, but slightly different properties. For example, if you want to slightly change the color of each mesh drawn. This is always recommended
   
```csharp

public class ColorPropertySetter : MonoBehaviour 
	{
	//The color of the object 
	public Color MaterialColor;
	//The material property block we pass to the GPU private 
	MaterialPropertyBlock propertyBlock;
	
	void OnValidate() 
	{ 
		//create propertyblock only if none exists 
		if (propertyBlock == null) propertyBlock = new MaterialPropertyBlock(); 
		//Get a renderer component either of the own gameobject or of a child
		Renderer renderer = GetComponentInChildren<Renderer>(); 
		//set the color property 
		propertyBlock.SetColor("_Color", MaterialColor); 
		//apply propertyBlock to renderer 
		renderer.SetPropertyBlock(propertyBlock); 
	}
}

```

