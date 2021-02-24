---
title: Unity之查找资源被哪里引用了
tags:
  - Find References
  - Unity
id: '1293'
categories:
  - - Unity3D
date: 2017-02-23 11:25:15
---

有时候要删除一个资源，就需要先查看下被哪里引用了，防止误删。Unity提供了一个方法 AssetDatabase.GetDependencies()，但是它只能查找这个资源引用了那些资源。 但是我想要的是查找某个资源被那些资源引用了，这是两种相反的查找公式。 网上找到一个查找脚本，速度很快，不过只能在Mac上用。右键选择一个资源，点击 Find References，会在Console面板上输出结果。

```
using UnityEngine;
using System.Collections;
using UnityEditor;
using System.Collections.Generic;

public class FindProject {

#if UNITY_EDITOR_OSX

[MenuItem("Assets/Find References In Project", false, 2000)]
private static void FindProjectReferences()
{
string appDataPath = Application.dataPath;
string output = "";
string selectedAssetPath = AssetDatabase.GetAssetPath (Selection.activeObject);
List<string> references = new List<string>();

string guid = AssetDatabase.AssetPathToGUID (selectedAssetPath);

var psi = new System.Diagnostics.ProcessStartInfo();
psi.WindowStyle = System.Diagnostics.ProcessWindowStyle.Maximized;
psi.FileName = "/usr/bin/mdfind";
psi.Arguments = "-onlyin " + Application.dataPath + " " + guid;
psi.UseShellExecute = false;
psi.RedirectStandardOutput = true;
psi.RedirectStandardError = true;

System.Diagnostics.Process process = new System.Diagnostics.Process();
process.StartInfo = psi;

process.OutputDataReceived += (sender, e) => {
if(string.IsNullOrEmpty(e.Data))
return;

string relativePath = "Assets" + e.Data.Replace(appDataPath, "");

// skip the meta file of whatever we have selected
if(relativePath == selectedAssetPath + ".meta")
return;

references.Add(relativePath);

};
process.ErrorDataReceived += (sender, e) => {
if(string.IsNullOrEmpty(e.Data))
return;

output += "Error: " + e.Data + "\n";
};
process.Start();
process.BeginOutputReadLine();
process.BeginErrorReadLine();

process.WaitForExit(2000);

foreach(var file in references){
output += file + "\n";
Debug.Log(file, AssetDatabase.LoadMainAssetAtPath(file));
}

Debug.LogWarning(references.Count + " references found for object " + Selection.activeObject.name + "\n\n" + output);
}

#endif
}
```