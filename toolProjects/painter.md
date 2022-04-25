---
layout: page
title: Unity Object Painter Tool
---

## Unity Object Painter Tool

<img src="{{site.url}}{{site.baseurl}}/assets/img/toolProjects/painter/i1.png" width="600" height="450">

After producing my <a href="{{site.url}}{{site.baseurl}}/toolProjects/population/">Cave Population Tool</a>, I felt confident enough to produce a painting tool in Unity that would save the artists time in Cave Population, but also for a tool that can be useful in other circumstances. For this tool, I actively raycast from the camera to the Mouse position in world cordinates. At the hit, a ring is drawn around that point, with random normals drawn within that ring. When the user clicks the left-mouse button, the connected prefab will be created at all of the normals. All of these parameters are adjustible and give the user visual feedback with lines drawn in the world. They can choose how big the ring is, as well as how many points are randomly selected within that ring. 

To make it more useful in other situations, I created a single object mode, that allows the user to simply click to create a prefab at the desired world location. I also hooked this tool up to Unity's Undo system, so that all changes are tracked and can be reverted. Which actually is much simplier than one would suspect! The following is the line of code use, with obj being the object changed by the script.

```cpp
Undo.RegisterCreatedObjectUndo(obj, "Paint");
```

Creating this tool has familiarized myself with Unity's Handles class, which has many useful functions for drawing lines and shapes within the world. This can be very useful when it comes to user feedback, as any tool that you create, you want to make sure is easy to use. It needs to be intuitive and informational as to what it does, and what it is currently doing. Here are some of my Handles calls that draws the initial hit line and the ring around the point.

```cpp
Vector3 hitNormal = hit.normal;
Vector3 hitTangent = Vector3.Cross(hitNormal, camTransform.up).normalized;
Vector3 hitBiTangent = Vector3.Cross(hitNormal, hitTangent);

Handles.color = Color.red;
Handles.DrawAAPolyLine(hit.point, hit.point + hit.normal);

Handles.zTest = UnityEngine.Rendering.CompareFunction.Always;
Handles.color = Color.green;
Handles.DrawWireDisc(hit.point, hit.normal, radius);
Handles.zTest = UnityEngine.Rendering.CompareFunction.LessEqual;
```

With this implementation, the artists had not trouble using both my Cave Population Tool and this Object Painter to quickly populate and Set Dress different physical spaces that we had within our world. This allowed me to heavily shorten the pipeline that the artists had on dressing these spaces. This is valuable time that can now be spend on other tasks that are more valuable for their time.

For this tool, I also created a Tool Guide Document! This ensured that artists could understand and use my tool, without me having to be with them explaining every single piece. You can find the document I made here: <a href="{{site.url}}{{site.baseurl}}/toolProjects/painter/guide.html/">Guide</a>

If you would like to see my code, here is a link to my Tool Script: <a href="{{site.url}}{{site.baseurl}}/toolProjects/painter/script.html/">Code</a>
