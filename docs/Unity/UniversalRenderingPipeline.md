# Universal Rendering Pipeline
- To access the main texture the KeyWord is "_BaseMap" not "_MainTex" 
  ```csharp
  var renderer = GetComponent<Renderer>();
  renderer.material.GetTextureOffset("_BaseMap");
  ```
