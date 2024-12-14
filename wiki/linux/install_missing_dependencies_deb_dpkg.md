When you manually install a .deb package using DPKG, dependencies will not be automatically installed.

This leaves the package installed, but in a unusable status until those dependencies are solved.
Fortunately the `dpkg` installation does indeed mark those dependencies in the system database, and `apt` can solve and install those automatically.

```
sudo apt-get -f install
```


## Example with Lutris

For example, with a manual installation of lutris:

```
sudo dpkg -i lutris_0.5.18_all.deb
```

you get several errors for unresolved dependencies:

```
dpkg: dependency problems prevent configuration of lutris:
 lutris depends on python3-lxml; however:
  Package python3-lxml is not installed.
 lutris depends on python3-gi-cairo; however:
  Package python3-gi-cairo is not installed.
 lutris depends on python3-setproctitle; however:
  Package python3-setproctitle is not installed.
 lutris depends on python3-magic; however:
  Package python3-magic is not installed.
 lutris depends on fluid-soundfont-gs; however:
  Package fluid-soundfont-gs is not installed.
 lutris depends on mesa-utils; however:
  Package mesa-utils is not installed.
 lutris depends on vulkan-tools; however:
  Package vulkan-tools is not installed.

dpkg: error processing package lutris (--install):
 dependency problems - leaving unconfigured
Processing triggers for gnome-menus (3.36.0-1.1ubuntu3) ...
Processing triggers for desktop-file-utils (0.27-2build1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
Processing triggers for man-db (2.12.0-4build2) ...
Errors were encountered while processing:
 lutris
```


After running the `apt` command, those dependencies are solved and installed:

```
sudo apt-get -f install
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Correcting dependencies... Done
The following additional packages will be installed:
  fluid-soundfont-gm fluid-soundfont-gs mesa-utils mesa-utils-bin python3-bs4
  python3-cssselect python3-gi-cairo python3-html5lib python3-lxml
  python3-magic python3-setproctitle python3-soupsieve python3-webencodings
  vulkan-tools
Suggested packages:
  python3-genshi python-lxml-doc
The following NEW packages will be installed:
  fluid-soundfont-gm fluid-soundfont-gs mesa-utils mesa-utils-bin python3-bs4
  python3-cssselect python3-gi-cairo python3-html5lib python3-lxml
  python3-magic python3-setproctitle python3-soupsieve python3-webencodings
  vulkan-tools
0 upgraded, 14 newly installed, 0 to remove and 6 not upgraded.
1 not fully installed or removed.
Need to get 135 MB of archives.
After this operation, 163 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
...
```

And finally you can use your manually installed `deb` package 
