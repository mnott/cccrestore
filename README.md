# NAME

cccrestore - File Restore Utility for Carbon Copy Cloner

# VERSION

Version 0.0.2

# LICENCE AND COPYRIGHT

Copyright (c) 2018 - 2020 Matthias Nott (mnott (at) mnsoft.org).

Licensed under WTFPL.

# SYNOPSIS

./cccrestore [options] file [version]

Command line options can take any order on the command line;
the file parameter is mandatory, the version number optional.

    Options:

      General Options:

      -help            brief help message   (alternatives: ?, -h)
      -man             full documentation   (alternatives: -m)

      Program Options:

      -b               Start CCC backup task (alternative: -backup)
      -u               Unmount the snapshots (alternative: -unmount)
      -r               Root folder to create mount the snapshots to (alternative: -root)
      -s               Source to look for snapshots (alternative: -sources)
      -v               Print out more information   (alternative: -verbose)

      Other Options:

      -doc             Recreate the README.md (needs pod2markdown)

# OPTIONS

- **-help**

    Print a brief help message and exits.

- **-man**

    Prints the manual page and exits.

- **-doc**

    Use pod2markdown to recreate the documentation / README.md.
    You need to configure your location of pod2markdown at the
    top, if you want to do this (it is really an option for me,
    only...)

- **-b**

    Carbon Copy Cloner can be run from the command line. If you
    configure a snapshot task according to
    [this article](https://bombich.com/kb/ccc5/leveraging-snapshots-on-apfs-volumes),
    you can start this task like so:

        $ cccrestore -b Snapshot

    If you omit the name of the Snapshot task, cccrestore will
    use "Snapshot" by default.

- **-u**

    Unmount the snapshots. This option takes precedence
    of all other options. The mounted snapshots, if any,
    from the root folder are unmounted.

- **-r**

    Root folder to mount the snapshots into. Default:

        /private/tmp/snaps

- **-s**

    Drives to look for snapshots in. For example:

        -s / -s "/Volumes/Backup"

- **-v**

    Be more verbose in printing out information.

# SUMMARY

This is a small script that lets you find, within snapshots that
[Carbon Copy Cloner](https://bombich.com/) may have taken for you,
older versions of files.

A previous version of this script was working based on \_CCC SafetyNet
subdirectories within your backup locations. As the author of Carbon
Copy Cloner, Mike Bombich, has pointed out to me, CCC's safety net
[has some shortcomings](https://bombich.com/kb/ccc5/frequently-asked-questions-about-carbon-copy-cloner-safetynet#archived\_bundles)
and he has suggested to rather use [snapshots on APFS Volumes](https://bombich.com/kb/ccc5/leveraging-snapshots-on-apfs-volumes).
In that same article, he also writes how to have hourly snapshots
created by adding a dummy backup task that copies from one empty
folder into another. Mike also gives an example on
[how to run snapshots more frequently](https://bombich.com/kb/ccc5/can-i-run-my-backups-more-frequently-hourly);
I have used that example for the backup option of cccrestore.

With that information, you can utilize APFS snapshots to create
versions of files, and with this program, you can then find any
older version of a file or directory.

# CONFIGURATION

With the command line options mentioned above, there is no need
to configure the program. But if you want to permanently store
the list of backup source volumes within the code, so that you
do not have to pass them as command line parameters, you can do
so towards the top of the script, where you'll find this line:

    my @dsources = ("/", "/Volumes/Backup", "/Volumes/Samsung T3 - Daten")

Just adapt this to your needs.

If you want to trigger backups with cccrestore, you must make
sure to point the following line to your actual location of
Carbon Copy Cloner:

    my $ccc = "/Applications/Utilities/Carbon Copy Cloner.app/Contents/MacOS/ccc";

Also, it may make sense, should your snapshot task not be named
"Snapshot", to adapt the following line:

    my $backup_task = "Snapshot";

# NOTE

I have just noticed that for with a recent update of XCode, the
readlink -f command no longer works. I suggest to install the GNU
version of readlink, greadlink as described
[here](https://www.topbug.net/blog/2013/04/14/install-and-use-gnu-command-line-tools-in-mac-os-x/).
I'm updating the relevant line at the top of the script, but to
make it work for you, if you have not yet installed the GNU command
line tools, chances are that cccrestore will not work for you.

# INSTALLATION

Copy cccrestore to a directory that you have in your path on
your Mac and make it executable. For example, if you have
downloaded it to your Downloads folder, you could open a
command line and do this:

    chmod 755 ~/Downloads/cccrestore
    cp -av ~/Downloads/cccrestore /usr/local/bin

# USAGE

In a terminal, just call the script with some file or
directory of which you want to quickly identify the versions
that Carbon Copy Cloner has created fo you. For example, if you
have a file Handbook.pdf in your current directory, you'd call:

    $ cccrestore Handbook.pdf
    Version: [  1] - Size:   6205 - Modified: 07/22/2020 00:39:49 - Snapshot: 07/22/2020 00:44:07
    Version: [  2] - Size:   5680 - Modified: 07/21/2020 23:24:02 - Snapshot: 07/21/2020 23:44:07
    Version: [  3] - Size:   5596 - Modified: 07/21/2020 22:59:51 - Snapshot: 07/21/2020 23:00:03
    Version: [  4] - Size:   5346 - Modified: 07/21/2020 22:44:03 - Snapshot: 07/21/2020 22:44:03
    Version: [  5] - Size:   5325 - Modified: 07/21/2020 22:43:45 - Snapshot: 07/21/2020 22:44:00
    Version: [  6] - Size:   4959 - Modified: 07/21/2020 21:41:22 - Snapshot: 07/21/2020 21:44:07
    Version: [  7] - Size:   4183 - Modified: 07/21/2020 20:42:07 - Snapshot: 07/21/2020 20:44:07
    Version: [  8] - Size:   4048 - Modified: 07/21/2020 19:23:30 - Snapshot: 07/21/2020 19:44:07
    Version: [  9] - Size:   3974 - Modified: 07/21/2020 17:50:32 - Snapshot: 07/21/2020 18:00:00
    Version: [ 10] - Size:   3381 - Modified: 07/21/2020 16:59:46 - Snapshot: 07/21/2020 17:00:00
    Version: [ 11] - Size:   2891 - Modified: 07/21/2020 14:12:21 - Snapshot: 07/21/2020 16:00:00

You can see, in the first column, a version numbering. You need that
for restoring any given version of the file. In the second column,
you see the file size; the third column holds the modification date
of the file, while the last column holds the timestamp of when the
snapshot was taken.

There may be many more snapshots on your system, and cccrestore
will investigate all of them; the above list will only contain
those where the file was changing from one snapshot to the next.

If you now want to restore one given version, you just call the
program again, in exactly the same way, but add the version number
to the end of it:

    $ cccrestore Handbook.pdf 7

The above command will open the folder containing version number 7.

# CLEANUP

cccrestore will load all available snapshots. This can clutter
your desktop, depending on whether or not you chose to show
mounted locations. To clean up and unmount all those snapshots,
just do this:

    $ cccrestore -u

This should unmount all mounted snapshots.
