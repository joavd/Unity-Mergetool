# Unity Smart Merge Tool

Unity comes with a powerful tool to handle merge conflicts in Unity. It's hidden
and there's almost no information regarding it for newcomers.

This is a quick guide on how to configure Git and Unity so that you can have
multiple persons working in the same scene.

## Software installed

* [Unity](https://unity3d.com/)
* [Git](https://git-scm.com/) and [Git LFS](https://git-lfs.github.com/)
* [WinMerge](http://winmerge.org/)

## Unity Project set up

**_Note: these steps need to be done whenever you make a new Unity Project_**

1. Open the project in Unity and in the top bar, select **Edit** and then 
   **Project Settings > Editor**
2. Change the **Version Control** mode to **Visible Meta Files** and the **Asset Serialization** 
   mode to **Force Text**

    ![Editor](Images/editorSettings.png)
3. _(Optional)_ In the top bar, select **Edit** and then
   **Project Settings > Player**
4. Change the **Scripting Runtime Version** to **.NET 4.x Equivalent** and the
   **API Compatibility Level** to **.Net Standard 2.0**
   
   ![Player](Images/playerSettings.png)

## Git set up

**_Note: these steps need to be done whenever you make a new Unity Project_**

1. Initialize repository `$ git init`
2. Initialize Git LFS `$ git lfs install`
3. _(Optional)_ Add these files to the root folder:
*   [`.gitignore`](https://raw.githubusercontent.com/github/gitignore/master/Unity.gitignore) -
    Indicates files to ignore, that won't be saved to the repository
  
*   [`.gitattributes`](https://raw.githubusercontent.com/JoaoAlexandreDuarte/Unity-Mergetool/master/.gitattributes) -
    Indicates attributes of the project, and which files to save in Git LFS mode (images, sounds, textures, etc).

1. Go to the project folder and make sure you can see the hidden files and folders, go to the 
   **.git folder** and open the **config** file with a text editor of your choice.
2. **Add the following lines** to the file and save it: **_(You will probably need to replace the_**
   **_path to the merge tool in line 5)_**
   ```
   [merge]
	tool = unityyamlmerge
   [mergetool "unityyamlmerge"]
	trustExitCode = false
	cmd = 'C:\\Program Files\\Unity\\Hub\\Editor\\2018.3.0f2\\Editor\\Data\\Tools\\UnityYAMLMerge.exe' merge -p "$BASE" "$REMOTE" "$LOCAL" "$MERGED"
   [mergetool]
	keepBackup = false
   ```

## Unity Smart Merge set up

1. Download a fallback merge tool of your preference. The only one that works for me personally is
   [WinMerge](http://winmerge.org/), but if you already have some other skip this step.
2. Go to the Unity installation folder in **Editor/Data/Tools and find the mergespecfile.txt** and 
   open the file with **administrator mode**, the path may vary according to the OS and if you have
    _Unity Hub_ installed.

   If you have _Unity Hub_ installed the file it's probably in `C:\Program Files\Unity\Hub\Editor\201x.x.xfx\Editor\Data\Tools\mergespecfile.txt`, if you don't it's 
   probably in `C:\Program Files\Unity\Editor\Data\Tools\mergespecfile.txt`-
   
   This file tells you how to set up the merge tool for various third party applications. The
   only uncommented lines are `unity use .....` and `prefab use .....`, these lines indicate the 
   calls to the fallback tool to solve scene _(unity use)_ and prefab _(prefab use)_ conflicts when
   it needs manual input on how it should solve it.
3. **Replace** these 2 lines:
   ```
   unity use "%programs%\YouFallbackMergeToolForScenesHere.exe" "%l" "%r" "%b" "%d"
   prefab use "%programs%\YouFallbackMergeToolForPrefabsHere.exe" "%l" "%r" "%b" "%d"
   ```

   **with**

   ```
   unity use "C:\Program Files (x86)\WinMerge\WinMergeU.exe" "%r" "%b" "%l" -o "%d" --auto-merge
   prefab use "C:\Program Files (x86)\WinMerge\WinMergeU.exe" "%r" "%b" "%l" -o "%d" --auto-merge
   * use "C:\Program Files (x86)\WinMerge\WinMergeU.exe" "%r" "%b" "%l" -o "%d" --auto-merge
   ```

   _You may need to change the path if you prefer another software or isntall it in another directory_.

## How to Use

When trying to merge or rebase your project and a **conflict appears**, like this

```
Auto-merging Assets/Scenes/Scene.unity
CONFLICT (content): Merge conflict in Assets/Scenes/Scene.unity
Automatic merge failed; fix conflicts and then commit the result.
```

instead of manually fixing it, you just type:

**`$ git mergetool`**

The tool will then resolve the conflicts for you automatically, if there is any conflict that requires
input in order to be fixed, the fallback tool _(WinMerge)_ will open, and then you will have to choose
the right option. If you don't have a fallback tool, you'll just get a message stating this, but 
you'll still be able to resolve the conflitcs in your editor.

**Save and close** the fallback tool. Then you simply need to run **`git add .`** in order to add the
changes made in git, and then **`git commit -m 'Your message here'`** to complete the merge and save it.

### Resources

[Unity Manual - Smart Merge](https://docs.unity3d.com/Manual/SmartMerge.html) <br/>
[Smart Merge not working](https://forum.unity.com/threads/smart-merge-not-working.315903/) <br/>
[Anacat - Unity Mergetool](https://github.com/anacat/unity-mergetool) <br/>
[Videojogos - Unity Git](https://github.com/VideojogosLusofona/unity_git)