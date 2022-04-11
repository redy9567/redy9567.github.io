---
layout: page
title: Unity Object Painter Tool Script
permalink: /toolProjects/painter/script
---

## Unity Object Painter Tool Script

Previous Page: <a href="{{site.url}}{{site.baseurl}}/toolProjects/painter/index.html/">Back</a>

```cpp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
#if UNITY_EDITOR
using UnityEditor;
#endif

#if UNITY_EDITOR
public class ObjectPainter : EditorWindow
{

    [MenuItem("My Tools/Object Painter")]
    public static void OpenWindow() => GetWindow<ObjectPainter>();

    public float radius = 2f;
    public int spawnCount = 8;

    private float raycastOriginDistance = 2.0f;

    public GameObject spawnPrefab;

    public bool enablePainting;
    public bool singleObjectMode;
    public bool useRandomRotations;
    public bool shouldEmbed;

    public float embedAmount = 1.0f;

    Vector2[] randomPoints; //Random points in disk surrounding center
    List<Vector3> objectPoints;
    List<Vector3> objectNormals;

    SerializedObject serializedInstance;
    SerializedProperty radiusProperty;
    SerializedProperty spawnCountProperty;
    SerializedProperty spawnPrefabProperty;
    SerializedProperty enablePaintingProperty;
    SerializedProperty singleObjectModeProperty;
    SerializedProperty useRandomRotationsProperty;
    SerializedProperty shouldEmbedProperty;
    SerializedProperty embedAmountProperty;

    void OnEnable()
    {
        serializedInstance = new SerializedObject(this);
        radiusProperty = serializedInstance.FindProperty("radius");
        spawnCountProperty = serializedInstance.FindProperty("spawnCount");
        spawnPrefabProperty = serializedInstance.FindProperty("spawnPrefab");
        enablePaintingProperty = serializedInstance.FindProperty("enablePainting");
        singleObjectModeProperty = serializedInstance.FindProperty("singleObjectMode");
        useRandomRotationsProperty = serializedInstance.FindProperty("useRandomRotations");
        shouldEmbedProperty = serializedInstance.FindProperty("shouldEmbed");
        embedAmountProperty = serializedInstance.FindProperty("embedAmount");

        GenerateRandomPoints();

        SceneView.duringSceneGui += DuringSceneGUI;
    }

    void OnDisable() => SceneView.duringSceneGui -= DuringSceneGUI;

    void DuringSceneGUI(SceneView sceneView)
    {
        Handles.zTest = UnityEngine.Rendering.CompareFunction.LessEqual;

        Transform camTransform = sceneView.camera.transform;

        

        Ray ray = HandleUtility.GUIPointToWorldRay(Event.current.mousePosition);


        if(Physics.Raycast(ray, out RaycastHit hit))
        {
            

            if(singleObjectMode)
            {
                Handles.color = Color.yellow;
                Handles.DrawAAPolyLine(hit.point, hit.point + hit.normal);

                objectPoints = new List<Vector3>();
                objectNormals = new List<Vector3>();
                objectPoints.Add(hit.point);
                objectNormals.Add(hit.normal);
            }
            else
            {
                Vector3 hitNormal = hit.normal;
                Vector3 hitTangent = Vector3.Cross(hitNormal, camTransform.up).normalized;
                Vector3 hitBiTangent = Vector3.Cross(hitNormal, hitTangent);

                Handles.color = Color.red;
                Handles.DrawAAPolyLine(hit.point, hit.point + hit.normal);

                Handles.zTest = UnityEngine.Rendering.CompareFunction.Always;
                Handles.color = Color.green;
                Handles.DrawWireDisc(hit.point, hit.normal, radius);
                Handles.zTest = UnityEngine.Rendering.CompareFunction.LessEqual;

                Handles.color = Color.yellow;

                objectPoints = new List<Vector3>();
                objectNormals = new List<Vector3>();
                foreach (Vector2 point in randomPoints)
                {
                    Vector3 rayOrigin = hit.point + (hitTangent * point.x + hitBiTangent * point.y) * radius;

                    rayOrigin += hitNormal * raycastOriginDistance;

                    Vector3 rayDirection = -hitNormal;
                    Ray pointRay = new Ray(rayOrigin, rayDirection);

                    if (Physics.Raycast(pointRay, out RaycastHit pointHit))
                    {
                        objectPoints.Add(pointHit.point);
                        objectNormals.Add(pointHit.normal);
                        Handles.DrawAAPolyLine(pointHit.point, pointHit.point + pointHit.normal);
                    }


                }
            }

            
        }

        if(Event.current.type == EventType.MouseMove)
        {
            sceneView.Repaint();
        }

        bool escapeKey = (Event.current.modifiers & EventModifiers.Alt) != 0;

        if(enablePainting && !escapeKey && Event.current.type == EventType.ScrollWheel)
        {
            float scrollDirection = Event.current.delta.y;

            serializedInstance.Update();

            radiusProperty.floatValue += scrollDirection;
            
            serializedInstance.ApplyModifiedProperties();
            Repaint();

            Event.current.Use();
            
        }

        if(enablePainting && !escapeKey && Event.current.type == EventType.MouseDown && Event.current.button == 0)
        {
            SpawnObjects();
            Event.current.Use();
            Selection.activeObject = null;
        }
    }

    private void OnGUI()
    {

        serializedInstance.Update();
        EditorGUILayout.PropertyField(radiusProperty);
        EditorGUILayout.PropertyField(spawnCountProperty);
        EditorGUILayout.PropertyField(spawnPrefabProperty);
        EditorGUILayout.PropertyField(enablePaintingProperty);
        EditorGUILayout.PropertyField(singleObjectModeProperty);
        EditorGUILayout.PropertyField(useRandomRotationsProperty);
        EditorGUILayout.PropertyField(shouldEmbedProperty);

        if(shouldEmbedProperty.boolValue)
        {
            EditorGUILayout.PropertyField(embedAmountProperty);
        }

        radiusProperty.floatValue = Mathf.Max(1.0f, radiusProperty.floatValue);
        spawnCountProperty.intValue = Mathf.Max(1, spawnCountProperty.intValue);

        if(serializedInstance.ApplyModifiedProperties())
        {
            GenerateRandomPoints();
            SceneView.RepaintAll();
        }

        if(GUILayout.Button("Generate New Points"))
        {
            GenerateRandomPoints();
            SceneView.RepaintAll();
        }

        if(Event.current.type == EventType.MouseDown && Event.current.button == 0)
        {
            GUI.FocusControl(null);
            Repaint();
        }
    }

    void GenerateRandomPoints()
    {
        randomPoints = new Vector2[spawnCount];
        for(int i = 0; i < spawnCount; i++)
        {
            randomPoints[i] = Random.insideUnitCircle;
        }

    }

    void SpawnObjects()
    {
        if (spawnPrefab == null)
            return;

        GameObject parent = GameObject.Find("Painted Objects");

        if(parent == null)
        {
            parent = new GameObject();
            parent.name = "Painted Objects";
        }

        for(int i = 0; i < objectPoints.Count; i++)
        {
            Vector3 loc = objectPoints[i];
            Vector3 normal = objectNormals[i];

            GameObject obj = GameObject.Instantiate(spawnPrefab, parent.transform);
            

            if(useRandomRotations)
            {
                float randomAngle = Random.Range(0.0f, 360.0f);
                Quaternion randRotation = Quaternion.Euler(0f, 0f, randomAngle);
                Quaternion objRotation = Quaternion.LookRotation(normal) * randRotation * Quaternion.Euler(90f, 0f, 0f);

                obj.transform.rotation = objRotation;
            }

            if(shouldEmbed)
            {
                loc += -normal * embedAmount;
            }

            obj.transform.position = loc;


            Undo.RegisterCreatedObjectUndo(obj, "Paint");
        }

        GenerateRandomPoints();
    }

}
#endif
```


