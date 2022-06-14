# Publishing webmaps

Instructor: Keith Jenkins

In this workshop, we will use several tools to publish a public webmap from an existing QGIS project:
- QGIS2Web: a QGIS plugin that creates all the HTML/CSS/Javascript/etc. files for the webmap
- Visual Studio Code: a powerfull code editor that will let us make changes to those files and automatically use Git to send the files to our own GitHub repository
- GitHub: a website that hosts Git repositories and allows us to create basic websites

## Before the session

Before the workshop, please go ahead and install Visual Studio Code and Git.

### Install Microsoft Visual Studio Code

Microsoft's Visual Studio Code ("VS Code") is a powerful text editor that can be used for any kind of text-based editing -- it's not just for code!  It integrates directly with Git and GitHub, making it easier for you to track changes to your files, keep a repository with a complete history of those changes, sync it with a copy stored on GitHub, and collaborate with others (or even just your future self).

- Go to https://code.visualstudio.com/
- Download the version for your system (Windows, MacOS, or Linux)
- Run the installer
  - When asked, check the boxes to enable:
    - Add "Open with Code" action to Windows Explorer file context menu
    - Add "Open with Code" action to Windows Explorer directory context menu
    (MacOS and Linux may have similar options, I don't know)
  - Accept the other defaults

The first time you run VS Code, you'll see a "Getting Started" tab that will walk you through various setup and customization options.  Feel free to explore, but you can also just close that tab.  (You can always get back to it later via the "Help" menu.)


### Install Git

You should see five icons along the left side of VS Code.
- Click the 3rd one, for "Source Control"

Source control (also called "version control") helps you track changes to your files.  This feature relies on Git, which must be installed on your computer.

If you already happen to have Git installed, VS Code should recognize it, and you will see "Open Folder" and "Clone Repository" buttons.

If you don't yet have Git installed, you should see a button to "Download Git for Windows" (or Mac/Linux).
- Click the button, which will open the Git website
- Download the latest version of Git for your computer
  - For Windows, you'll want the Standalone Installer, 64-bit version
- Run the installer, generally accepting any default values, with the following exceptions:
  - Choosing the default editor used by Git = Visual Studio Code
  - Adjusting the name of the initial branch in new repositories = override, "main"
  - Accept all other defaults
- Close and reopen VS Code so that it will now recognize Git on your system

## During the session

Download the sample QGIS project and data from:
- https://github.com/kgjenkins/webmap-workshop/archive/refs/heads/main.zip

Be sure to unzip the .zip file.


## After the session

### Reflection


