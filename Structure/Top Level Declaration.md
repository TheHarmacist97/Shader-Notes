The first line in a Unity shader declares it's hierarchy in the shader inspector and it's name
A shader declared as:  `Shader "Custom/Example"`  will be in the the Custom folder of the shader inspector and will be named as `Example`

> [!caution] New Shaders
> Newly created shaders will be created under the `Hidden` folder Unity will not display the `Hidden` folder, so remember to change this

![[Hierarchy 1.png]]
![[Hierarchy 2.png]]

>[!Tip] Shader Hierarchy Management
You can make subfolders by extending the declaration like this
`Shader "Custom/SubFolder/Example"`

Next up [[Exposed Properties|We'll learn about exposed properties]]