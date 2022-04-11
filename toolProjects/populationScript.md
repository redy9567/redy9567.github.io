---
layout: page
title: Unity Cave Population Tool Script
permalink: /toolProjects/population/script
---

## Unity Cave Population Tool Script

Previous Page: <a href="{{site.url}}{{site.baseurl}}/toolProjects/population/index.html/">Back</a>

```cpp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
#if UNITY_EDITOR
using UnityEditor;
using UnityEditor.SceneManagement;
#endif

#if UNITY_EDITOR
public class CavePopulation : EditorWindow
{

    public GameObject caveObject;

    public GameObject genObjectPrefab;

    public bool visualize;
    public int visualizerMode = 0;
    public int batchSize = 1;
    public int angleOfTolerance = 0;
    public float regressionValue = 0.0f;
    public int distributionPercentage;
    public bool isCeiling;
    public bool shouldRotate;
    public bool randomAngleOffsets;
    public float randomAngleOffsetValue = 0.0f;
    public bool boundBySphere;

    private bool visualizeEquipped = false;
    private GameObject caveMeshImmediate = null;
    private Mesh caveMesh = null;
    private MeshVisualizer mv = null;

    private GameObject boundingSphere = null;

    string[] visualizerModeNames = new string[] { "Vertex", "Normal/No Indication", "Normal/Vertical Indication" };
    int[] visualizerOptions = { 0, 1, 2 };

    [MenuItem("My Tools/Cave Population")]
    public static void ShowWindow()
    {
        GetWindow(typeof(CavePopulation));
    }

    public void OnGUI()
    {

        SerializedObject serialized = new SerializedObject(this);
        SerializedProperty caveObjectProperty = serialized.FindProperty("caveObject");

        EditorGUILayout.LabelField("Cave Population", EditorStyles.boldLabel);
        EditorGUILayout.PropertyField(caveObjectProperty, true);
        serialized.ApplyModifiedProperties();

        if(caveObject != null)
        {
            MeshFilter mf = caveObject.GetComponentInChildren<MeshFilter>();
            caveMeshImmediate = mf.gameObject;
            caveMesh = mf.sharedMesh;

            visualize = EditorGUILayout.Toggle("Should Visualize Mesh?", visualize);

            if(visualize && !visualizeEquipped)
            {
                visualizeEquipped = true;
                mv = caveMeshImmediate.AddComponent<MeshVisualizer>();
                mv.sphereRadius = 1.0f;
            }
            else if(!visualize && visualizeEquipped)
            {
                DestroyImmediate(mv);
                mv = null;
                visualizeEquipped = false;
            }

            if(visualize && visualizeEquipped)
            {
                visualizerMode = EditorGUILayout.IntPopup("Visualizer Mode:", visualizerMode, visualizerModeNames, visualizerOptions);
                mv.visualizerMode = visualizerMode;

                EditorGUILayout.PrefixLabel("# of Items:");
                batchSize = EditorGUILayout.IntSlider(batchSize, 1, 1000);
                if (mv.startingIndex + batchSize > mv.vertexCount)
                    batchSize = mv.vertexCount - mv.startingIndex;

                mv.batchSize = batchSize;

                EditorGUILayout.LabelField("Showing item " + mv.startingIndex + " through " + (mv.startingIndex + batchSize - 1) + " out of " + mv.vertexCount, EditorStyles.helpBox);
                EditorGUILayout.BeginHorizontal();

                if(GUILayout.Button("Previous Set"))
                {
                    if(mv.startingIndex - batchSize >= 0)
                    {
                        mv.startingIndex -= batchSize;
                    }
                    else
                    {
                        mv.startingIndex = 0;
                    }
                }

                if(GUILayout.Button("Next Set"))
                {
                    if (mv.startingIndex + batchSize + batchSize <= mv.vertexCount)
                    {
                        mv.startingIndex += batchSize;
                    }
                    else if (mv.startingIndex + batchSize <= mv.vertexCount)
                    {
                        mv.startingIndex = mv.vertexCount - batchSize;
                    }
                }

                EditorGUILayout.EndHorizontal();

            }

            EditorGUILayout.PrefixLabel("Angle of Tolerance:");
            angleOfTolerance = EditorGUILayout.IntSlider(angleOfTolerance, 0, 75);

            if(mv != null)
                mv.angleOfTolerance = angleOfTolerance;

            EditorGUILayout.PrefixLabel("Regression Into Mesh:");
            regressionValue = EditorGUILayout.Slider(regressionValue, 0.0f, 1.0f);

            SerializedProperty genObjectProperty = serialized.FindProperty("genObjectPrefab");
            EditorGUILayout.PropertyField(genObjectProperty, true);
            serialized.ApplyModifiedProperties();

            isCeiling = EditorGUILayout.Toggle("Is Ceiling?", isCeiling);

            shouldRotate = EditorGUILayout.Toggle("Should Rotate to Normals?", shouldRotate);

            if(shouldRotate)
                randomAngleOffsets = EditorGUILayout.Toggle("Randomize Offset Angles?", randomAngleOffsets);

            if(shouldRotate && randomAngleOffsets)
            {
                EditorGUILayout.PrefixLabel("Maximum Offset Angle:");
                randomAngleOffsetValue = EditorGUILayout.Slider(randomAngleOffsetValue, 0.1f, 10.0f);
            }

            boundBySphere = EditorGUILayout.Toggle("Bound Gen to Sphere?", boundBySphere);

            if(boundBySphere && boundingSphere == null)
            {
                boundingSphere = GameObject.CreatePrimitive(PrimitiveType.Sphere);
                boundingSphere.name = "Cave Tool Bounding Sphere";
            }

            if(!boundBySphere && boundingSphere)
            {
                DestroyImmediate(boundingSphere);
            }

            if (mv)
            {
                mv.boundBySphere = boundBySphere;

                if (boundBySphere)
                {
                    mv.boundingSphere = boundingSphere;
                }
            }

            EditorGUILayout.PrefixLabel("Distribution Percentage:");
            distributionPercentage = EditorGUILayout.IntSlider(distributionPercentage, 1, 100);

            if (GUILayout.Button("Populate Cave"))
            {
                PopulateCave();
            }

        }

        

    }

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    private void OnDestroy()
    {
        if(mv)
        {
            DestroyImmediate(mv);
        }

        if(boundingSphere)
        {
            DestroyImmediate(boundingSphere);
        }
    }

    private void PopulateCave()
    {
        MeshFilter mf = caveObject.GetComponentInChildren<MeshFilter>();
        GameObject immediateObj = mf.gameObject;
        Mesh mesh = mf.sharedMesh;

        GameObject parent = new GameObject();
        parent.name = "Rocks Generated";

        float tolerance = (angleOfTolerance / 90.0f);

        if (genObjectPrefab != null)
        {
            if(isCeiling)
                for(int i = 0; i < mf.sharedMesh.vertexCount; i++)
                {
                    if (checkBound(mesh.vertices[i]) && ShouldGen() && Vector3.Dot(mesh.normals[i], Vector3.up) < -tolerance)
                    {
                        GameObject obj = Instantiate(genObjectPrefab, parent.transform);
                        obj.transform.position = immediateObj.transform.position + mesh.vertices[i] + new Vector3(0, regressionValue, 0);

                        if(shouldRotate)
                        {
                            Vector3 rotateTo = mesh.normals[i];

                            if(randomAngleOffsets)
                            {
                                float theta;
                            }

                            obj.transform.rotation = Quaternion.FromToRotation(obj.transform.up, mesh.normals[i]);
                        }
                            
                    }
                }
            else
                for (int i = 0; i < mf.sharedMesh.vertexCount; i++)
                {
                    if (checkBound(mesh.vertices[i]) && ShouldGen() && Vector3.Dot(mesh.normals[i], Vector3.up) > tolerance)
                    {
                        GameObject obj = Instantiate(genObjectPrefab, parent.transform);
                        obj.transform.position = immediateObj.transform.position + mesh.vertices[i] - new Vector3(0, regressionValue, 0);

                        if (shouldRotate)
                            obj.transform.rotation = Quaternion.FromToRotation(obj.transform.up, mesh.normals[i]);
                    }
                }
        }
    }

    private bool ShouldGen()
    {
        float val = Random.Range(0.0f, 100.0f);

        return val < distributionPercentage;
    }

    private bool checkBound(Vector3 pos) //If bound by sphere, determine if location is in sphere. Otherwise, return true.
    {
        if (boundBySphere)
        {
            if (Vector3.Distance(pos, boundingSphere.transform.position) < boundingSphere.transform.localScale.x / 2.0f)
                return true;
            else
                return false;
        }
        else
            return true;
    }
}
#endif
#endif
```


