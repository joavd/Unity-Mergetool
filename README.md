# Unity Smart Merge Tool

Unity comes with a powerful tool to handle merge conflicts in Unity. It's hidden
and there's almost no information regarding it for newcomers.

This is a quick guide on how to configure Git and Unity so that you can have
multiple persons working in the same scene.

## Software installed

* [Unity](https://unity3d.com/)
* [Git](https://git-scm.com/) and [Git LFS](https://git-lfs.github.com/)
* [WinMerge](http://winmerge.org/)

## Unity Project Setup

**_Note: these need to be done whenever you make a new Unity Project_**

1. Open the project in Unity and in the top bar, select **Edit** and then
   **Project Settings > Editor**
2. Change the **Version Control** mode to **Visible Meta Files** and the 
   **Asset Serialization** mode to **Force Text**

    ![Editor](Images/editorSettings.png)
3. _(Optional)_ In the top bar, select **Edit** and then
   **Project Settings > Player**
4. Change the **Scripting Runtime Version** to **.NET 4.x Equivalent** and the
   **API Compatibility Level** to **.Net Standard 2.0**
   
   ![Player](Images/playerSettings.png)