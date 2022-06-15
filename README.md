# Publishing webmaps

Instructor: Keith Jenkins

In this workshop, we will use several tools to publish a public webmap from an existing QGIS project:
- **qgis2web** is a QGIS plugin that creates all the HTML, CSS, and JavaScript files for the webmap
- **Visual Studio Code** ("VS Code") is a powerfull code editor that will let us make and track changes to those files and send them to GitHub
- **GitHub** is a website that hosts Git repositories and allows us to create basic websites
- **Git** is a versioning tool and protocol that works behind the scenes to track changes to files.  Although it is also possible to use Git directly, we will use it indirectly through both VS Code and GitHub.

## Before the session

Before the workshop, please go ahead and install Visual Studio Code and Git, following the instructions below.

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

### Creating the webmap files

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

Leave the settings as they are for now.  There is currently no option directly within the qgis2web dialog to omit an attribute from the popup, so sometimes it is helpful to create a copy of the layer that only contains the attributes we want to display.  Another option is, in the "attributes form" section of the layer properties, to set the "widget type" of specific fields to "hidden".  Below, we are going to edit the HTML output to further customize the popups for the Beagle and Pit Bull layers.  Note that the Municipal_Boundaries layer has been set to not show popups at all.

The qgis2web plugin can output webmaps using three different webmapping frameworks (OpenLayers, Leaflet, and Mapbox GL JS).

- Select "Leaflet" (bottom)
- Click the "Update preview" button (bottom)

Explore the webmap preview, and try clicking a point.  Again, we see that the popups have more information than we would really like to show, but we will fix this later.

On the "Appearance" tab:
- Set "Add layers list" to "Expanded"
- Set "Template" to "full-screen"
- Set "Max zoom level" to 18 (which is the limit of the basemap)
- Check the box for "Restrict to extent"

Click the "Update preview" button again to check the output.  When we are ready, we can export the webmap files to a folder.

On the "Export" tab:
- Click the "..." button to select the output folder location (you can save it anywhere -- just remember where you saved it!)
- Click the "Export" button (bottom)

Once the files are saved, the webmap will automatically open in your web browser.  Note that the URL begins with `file:///`, which indicates that the webpage is coming from a local file, not the internet.

### Pushing the files to GitHub

There are many different ways of hosting the webmap files online.  If you already have file access to a standard web server, you could simply copy the output folder of files to that server.

We are going to use GitHub Pages to host the webmap.  GitHub Pages can be used for static web sites, which generally means read-only sites that don't need to write new information to the server.  Even though we are creating a dynamic, interactive map, the files themselves are static, which makes it a good use case for GitHub Pages.

We will use Visual Studio Code ("VS Code") to create a new GitHub Repository, push our files there, and publish using GitHub Pages.

Find the qgis2web output folder (using Windows File Explorer, MacOS Finder, etc.) -- the name will look something like `qgis2web_2022_06_16_10_54_37_779288` which is pretty long, so we'll rename it (using only numbers, letters, hyphens, and underscores, since it will also be the name of our GitHub repository)

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

- On the row at the top that says "Changes", click the `+` symbol to stage all changes.  (We could also stage files individually, but in this case we want them all.)

![image](https://user-images.githubusercontent.com/3355358/173660915-fd6ab735-40f7-4c6f-95d1-7aa1a29edcbe.png)

Once the files are staged, the display changes ever so slightly -- all the files are now listed under "Staged Changes".  The `A` after each filename indicates that these are new files that have been added.  (An `M` would indicate a file that was already being tracked that has been modified.)

![image](https://user-images.githubusercontent.com/3355358/173662190-a3c89b5c-b19d-444c-aea3-ae95f19aa22b.png)

- Click the checkmark icon at the top to commit the staged changes to your local repository
- In the prompt at top center of the window, type a short commit message (something like "initial qgis2web files") and press enter

Now, let's publish our files to GitHub.

- Click the "Publish Branch" button

At top center, you have an option to rename the GitHub
repository (leave it as `dogs-webmap`).  And you also can choose whether to publish it as a private or public repository.  You can choose either, although the GitHub Pages site will always be public, so you might as well make the repository public.  (It will still be read-only, so no one can make changes unless you explicitly grant them that permission.)

- Click "Publish to GitHub public repository"

It may take a moment for the files to transfer.  When complete, you should see a message at bottom right.

- Click "Open on GitHub"

The GitHub webpage for your new repository should appear in your web browser.  We are almost there!

### Publishing a website via GitHub Pages

The last step is to activate GitHub Pages to publish these files as an actual website.

- On the GitHub page for your repository, click "Settings" (near the top)
- Click "Pages" (left)
- Under "Source", change where it says "None" to "main"
- Click "Save"

It should now say "Your site is ready to be published at `https://{username}.github.io/dogs-webmap`"

- Click the link

If your webmap doesn't open at first, it may be that GitHub is still building the site, which usually takes less than a minute for a simple qgis2web web map.  Be patient and try again after a short wait.

### Modifying the webmap code

As we saw, there are many different settings we can change when exporting the webmap via qgis2web.  We could also change the symbology in QGIS and re-export the files.

But with just a little knowledge of HTML, CSS, and Javascript, we can also edit the output files directly, which will give us an even greater level of customization.

Let's make some changes to our local files, test the results in our web browser.  When we are ready, we can then update GitHub with the updated files.

First, let's do something about those long lists of every single attribute that shows up when we click a point.  Let's simplify the popups to just show the jurisdiction and name and breed of the dog.

- In VS Code, click the "Explorer" icon (top left)
- Click the `index.html` file to edit it

If you've never worked with HTML/CSS/Javascript before, it might look a bit scary!  But don't worry -- we'll find some bits of text we can recognize, and if we make any fatal mistakes that break the webmap, we can always go back to the previous version that we have committed.

Scroll down to line 124, where we start to see some code for the pitbull popup that goes through each attribute in the dog license data.  Each attribute is within `<tr>...</tr>` tags, which stands for "table row".  We can simply remove the table rows we don't want to show.

https://user-images.githubusercontent.com/3355358/173693548-1d852148-cb6c-482e-a206-5affcb00d136.mp4

The trick is to delete whole `<tr>...</tr>` sections.  If you leave a stray opening `<tr>` or closing `</tr>` tag, you may get an error when viewing in a web browser.

- Delete at least some of the unwanted rows from both the pitbull popup and the beagle popup (further down in the `index.html` file).

You may notice that whenever you delete lines, there is a red triangle marker indicating where the deleted lines used to be.

Also, there is a circle icon up next to the `index.html` tab, indicating that there are unsaved changes.

- Press CTRL-S to save your edits.

Let's open the modified local file in our web browser to see the effects of our changes.

- Double-click the `index.html` file in your file browser (Windows File Explorer or MacOS Finder)

If something goes wrong, try checking that your remaining `<tr>...</tr>` pairs match up.  You could also use CTRL-Z to undo your edits to back up to before the mistake.  You can also always go back to a previously-committed version, as described below.

### Updating your local repository

After saving changes, you should see a number "1" appear on the "Source Control" icon on the left side of the VS Code window, indicating that there is one file that has been changed since the last commit.

- Click the "Source Control" icon to see which files have changes

If you've really messed things up and just want to go back to the last committed version, you can click the curved back-arrow icon ("Discard Changes").

If you want to see exactly what has changed, click on the filename, and VS Code will show what is commonly called a "diff", a color-coded display of changes, with red indicating deletions, and green indicating additions.  Git generally works line by line, so even if you change just a single letter, it will look like you removed the whole line and replaced it with a new one.

If you like your changes and want to commit them:

- Click the `+` for the file you want to stage
- Enter a brief message describing the commit
- Click the checkmark icon at the top -- this commits the staged changes to your local Git repository

https://user-images.githubusercontent.com/3355358/173695377-03da23a7-5c18-439b-90c3-577c98adb2f0.mp4

### Pushing your changes to GitHub

- Click "Sync Changes" to push the changes to GitHub

At this point, you can reload your repository page on GitHub to confirm that your changes appear there.  There is no need to do anything else for GitHub Pages, since it will automatically update your site whenever new changes are pushed to GitHub.

Go back to your GitHub pages site (`https://{username}.github.io/dogs-webmap`) and press CTRL-SHIFT-R to force your web browser to reload the site.  (Sometimes the normal CTRL-R is not enough, since your browser tries to re-used cached files as much as it can.)

### Git Notes

Any folders being tracked as a local Git repository will have a hidden folder called `.git` that contains special files for tracking changes.  Don't mess with these files!  (Depending on your system configuration, you may not even see this folder, since it is marked as a hidden system folder.  It also does not appear in the VS Code explorer.)  Only the outermost folder will have this .git folder -- subfolders will not have one.

Git also has a concept of "branches", which are different, possibly experimental, versions of a repository.  For example, I might create a new branch for some substantial data cleanup.  I can easily switch between branches, so I can easily interrupt my cleanup work in progress, switch back to the main branch to fix something in my existing web map, commit that to GitHub to update my public webmap, then switch back to the data-cleanup branch to continue my work there.

### GitHub Notes

GitHub has a bunch of other useful features, including:
- **Issues**, for documenting tasks to be done, assigning people to do them, and having conversations about rationales and approaches
- **Pull Requests**, which allow collections of commits to be bundled and proposed for approval.  For example, I might add some new features to your webmap -- you could look at the changes, try them out, and either approve them or suggest further edits.  A pull request (or "PR") is often tied to a specific branch, to isolate the PR from other changes that might be happening.
- **Projects**, which makes it easier to organize, filter, and sort collections of issues and pull requests
- **Wikis**, which can be used to help document your project (or just make the documentation part of repository)

### VS Code Notes

VS Code has a lot of powerful editing features, including:
- search and replace across many files at once:
  - all files in the project
  - or just selected filetypes
  - regular expressions for more sophisticated matching
- fast switching between Git branches
- extensions for specific tasks or types of files, such as:
  - "Edit csv" lets you view and edit .csv files in a table viewer (rather than plaintext)
  - Syntax highlighters to provide context-aware color coding for various file types and programming languages
  - Customizable keyboard shortcuts to mimic other popular code editors

## After the session

### Reflection


