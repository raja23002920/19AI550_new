# Ex.No: 3  Basic movements in Unity 
### DATE: 07/08/2026
### REGISTER NUMBER : 212223240134
### AIM: 
 To learn the basic movements translation,scaling and rotation of game objects through code.
### Procedure:
1. Setup the Scene
2. Open Unity and create a 3D Scene.
3. Add three objects:Cube → Rename to Object1 (for movement),Sphere → Rename to Object2 (for rotation).Capsule → Rename to Object3 (for scaling).
4. Add the Script,Create a C# Script → Name it TransformOperations.cs.
5. Write the code for translation,scaling and rotation,save and close the script
6. Save the script
7. Select any empty GameObject (or create one: GameObject → Create Empty).
8. Attach the TransformOperations script to it.
9. In the Inspector, assign Object1 → Drag the Cube,Object2 → Drag the Sphere.Object3 → Drag the Capsule.
10. Run the Scene Press Play ▶️ in Unity
11. Stop the program.
### Program 
```c
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class movement : MonoBehaviour
{
 public Transform Ob1;
 public Transform Ob2;
 public Transform Ob3;
 void Start()
 {
    

 }

 // Update is called once per frame
 void Update()
 {
     Ob1.Translate(0.2f, 0, 0);
     Ob2.Rotate(0.2f, 0, 0);
     Ob3.localScale += new Vector3(0, 0.2f, 0);
 }
}
```
### Output:

<img width="1376" height="731" alt="image" src="https://github.com/user-attachments/assets/f19638c0-052f-4d1e-aff4-8490328ee327" />



<img width="1380" height="736" alt="image" src="https://github.com/user-attachments/assets/f1738565-6e5b-4b20-8fa8-74d8042139d2" />



### Result:
Thus the basic movement is learned through scripting


