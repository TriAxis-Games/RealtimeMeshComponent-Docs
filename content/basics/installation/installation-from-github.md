+++
title = "Install from GitHub"
weight = 2
+++

To install the RealtimeMeshComponent from GitHub requires a few steps.

1. You must have a C++ ready project. If you already have C++ code, you're ready to go! If not you can go to `Tools -> New C++ Class` and create some template actor to convert your project to a C++ project. 

2. You can download the plugin from GitHub depending on your version from one of the two links below. You can either download the project as a zip or clone the project by clicking the green `<> Code` button.
   1. The Core version, which is free to use can be obtained here: (Realtime Mesh Component Core)[https://github.com/TriAxis-Games/RealtimeMeshComponent]
   2. The Pro version, which is paid-access can be obtained here: (Coming Soon!)[#]
   
3. If you downloaded the zip version, you must unzip the contents into the `{YourProject}/Plugins/RealtimeMeshComponent/` folder

4. Compile and run your project, which the engine should be able to do automatically by opening the `.uproject` file or through your code editor of choice. 

5. From here if you'd like to use the RMC from C++ code, you simple need to add `RealtimeMeshComponent` to your `.Build.cs` file like any other module you intend to use from C++.
