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

Download the sample QGIS project and data from
https://github.com/kgjenkins/webmap-workshop/archive/refs/heads/main.zip

Be sure to unzip the .zip file.
- On Windows: right-click the .zip file > Extract All...

Browse into the extracted folders until you are in the folder where you see the files.
- Open the QGIS Project: `dog-map.qgz`

This project has 4 layers:
- Beagle = just the records from the CSV file of dog licenses where `primary_breed='BEAGLE'`
- Pit Bull = just the records from the CSV file of dog licenses where `primary_breed='PIT BULL'`
- Municipal_Boundaries = city and village boundaries
- Stamen Toner Lite = a muted basemap that lets our data stand out

### Creating the webmap

We are going to use the qgis2web plugin to create a small website that contains an interactive version of the map that can be viewed in a web browser.

To install the plugin:
- "Plugins" menu > Manage and Install Plugins...
- Select the "All" tab on the left
- Search for qgis2web
- Click the "Install Plugin" button (bottom-right)

Once the plugin in installed:
- "Web" menu > qgis2web > Create web map

The dialog has several tabs of options.  The first tab "Layers and Groups" lets you control which layers are visible, whether features in a layer will display popups when clicked, and how the attribute data will be formatted.

For example, this configuration:  
![image](https://user-images.githubusercontent.com/3355358/173639307-fb6493ff-adf8-4dc4-8b43-b5f9a3913aa8.png)

will show a popup like this:  
![image](https://user-images.githubusercontent.com/3355358/173639497-e0cf7d5e-0e15-4263-9f15-7ee60d254e8d.png)

Leave the settings as they are for now.  There is currently no option directly within qgis2web to omit an attribute from the popup, so sometimes it is helpful to create a copy of the layer that only contains the attributes we want to display.  We can also edit the HTML output later to further customize the popups.

The qgis2web plugin can output webmaps using three different webmapping frameworks (OpenLayers, Leaflet, and Mapbox GL JS).

- Select "Leaflet" (bottom)
- Click the "Update preview" button (bottom)

Explore the webmap preview, and try clicking a point.  Again, we see that there is more information display than we would like, but we will fix this later.

On the "Appearance" tab:
- Set "Add layers list" to "Expanded"
- Set "Template" to "full-screen"
- Set "Max zoom level" to 19 (which is near the limit of the basemap)
- Check the box for "Restrict to extent"

Click the "Update preview" button again to check the output.  When we are ready, we can export the webmap files to a folder.

On the "Export" tab:
- Click the "..." button to select the output folder location (you can save it anywhere -- just remember where you saved it!)
- Click the "Export" button (bottom)

Once the files are saved, the webmap will automatically open in your web browser.  Note that the URL begins with `file:///`, which indicates that the webpage is coming from a local file, not the internet.



### Publishing the webmap

There are many different ways of hosting the webmap files online.  If you already have file access to a standard web server, you could simply copy the output folder of files to that server.

We are going to use GitHub Pages to host the webmap.  GitHub Pages can be used for static web sites, which generally means read-only sites that don't need to write new information to the server.  Even though we are creating a dynamic, interactive map, the files themselves are static, which makes it a good use case for GitHub Pages.

We will use Visual Studio Code ("VS Code") to create a new GitHub Repository, push our files there, and publish using GitHub Pages.

Find the qgis2web output folder (using Windows File Explorer, MacOS Finder, etc.) -- the name will look something like `qgis2web_2022_06_16_10_54_37_779288` which is pretty long, so we'll rename it (using only numbers, letters, hyphens, and underscores, since it will also be the name of our github repository)

- Rename the folder to something like `dogs-webmap`
- Open the folder

The folder contains an `index.html` file as well as several folders containing more files.  These were all generated by the qgis2web plugin.  The `index.html` file contains pointers to the other files it needs, so don't rename or move anything here!

Let's open all this as a project within VS Code

- Right-click the blank space within the `dogs-webmap` folder > Open with Code
- Close the "Get Started" tab if it appears

On the left, you should see a list of the folders and files in the `dogs-webmap` folder.  If you don't see it, click the top icon ("Explorer") that looks like pages.

![image](https://user-images.githubusercontent.com/3355358/173659220-7eec4e46-cb77-4a3a-bb3a-56a44ac80dbe.png)

Before we start any editing, let's create a local repository so that we can track any changes.

- Click the 3rd icon ("Source Control")
- Click the "Initial Repository" button

You should see a list of all 51 files within the `dogs-webmap` folder.  The `U` to the right of each filename means that the file is currently untracked.  We want to track all these files, so:

- On the row at the top that says "Changes", click the `+` symbol to stage all changes.






## After the session

### Reflection


