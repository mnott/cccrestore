Welcome to cccrestore!
=====================


Summary
---------

This is a small script that lets you find snapshots you may have taken, of a file or directory, using the awesome backup tool [Carbon Copy Cloner](https://bombich.com/). Carbon Copy Cloner can keep versions in its so-called safety net. Other than TimeMachine, it does not provide for a simple user interface to identify which versions of any given file or directory are actually available. So when you are looking for some file **Handbook.pdf** that you may have on your desktop, you would end up hunting for that file in those ***_CCC SafetyNet*** subdirectories within your backup locations, and more often than not, you'd not find it in many of those - as it is only backed up into the safety net if it actually was changed.

**cccrestore** solves this searching issue by giving you a very simple command that you can run in your terminal: It shows you which versions are available of any given file or directory, and it also allows you to directly open the directory that contains a given version, in a Finder window.

----------

Configuration
---------

If you have downloaded **cccrestore** to your Downloads folder, open the file in an editor. Towards the top, you'll see a configuration section:

```
###################################################
# Configuration
# 
# Put here the sources where you would have CCC
# run your backups to. Quote or escape spaces. Use
# comma to separate multiple sources.
# 
###################################################

SOURCES=/Volumes/LaCie/Backup,"/Volumes/T3 - Backup Matthias"
```

In the line that reads SOURCES, add the specific location(s) where you normally have Carbon Copy Cloner run your backups to. You can easily identify them: They normally would have a subdirectory ***_CCC SafetyNet***.

----------

Installation
---------

Copy **cccrestore** to a directory that you have in your path on your Mac and make it executable. For example, if you have downloaded it to your Downloads folder, you could open a command line and do this:

```
chmod 755 ~/Downloads/cccrestore
cp -av ~/Downloads/cccrestore /usr/local/bin
```

----------

Usage
---------

In a terminal, just call the script with some file or directory of which you want to quickly identify the versions that Carbon Copy Cloner has created fo you. For example, if you have a file Handbook.pdf in your current directory, you'd call:

```
$ cccrestore Handbook.pdf 

Version found in /Users/mnott/Desktop/Handbook.pdf:

[  0] 2016-11-05 09:31:39     1000 /Users/mnott/Desktop/Handbook.pdf

Versions found in /Volumes/LaCie/Backup:

[  1] 2016-11-05 09:31:39     1000 /Volumes/LaCie/Backup/Users/mnott/Desktop/Handbook.pdf

Versions found in /Volumes/T3 - Backup Matthias:

[  2] 2016-11-05 09:31:39     1000 /Volumes/T3 - Backup Matthias/Users/mnott/Desktop/Handbook.pdf

To restore a version, run the same command again and add the version number as last parameter.
```

Whether you use absolute paths or relative paths makes no difference. Just make sure to escape them correctly, i.e., use either a backslash in front of spaces, or just double-quote the file path, if you have spaces in it.

Now, the output shows you, in the first column, a version number, in the second column the last modification date of the file, in the third column you see the file size, and in the last column you see the path where the file was found. Version number 0 is always the current file you have actually been looking for.

Let's assume you want to check out the version number 2. To do so, just call the exact same command again (typically, hit the up arrow), and put, as last parameter, that version number:

```
$ cccrestore Handbook.pdf 2
```
This will open the directory within which the version of the file or directory you were looking for, was found, in a finder window.

And that's already all to it! Have fun with it.

