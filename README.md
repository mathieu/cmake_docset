cmake-docset
============

Create a CMake docset to use in @Kapeli 's [Dash](http://kapeli.com/dash) 

Installation
------------
 - checkout this repo 
 - create a subdirectory 'build'
 - into this subdirectory type ```cmake ..```
 - then ```make intall```
 - you should now have a CMake.docset that can be fead into Dash
 
Packaging
---------
 - checkout this repo 
 - create a subdirectory 'build'
 - into this subdirectory type ```cmake ..```
 - then ```make package```
 - you should now have a CMake.tar.gz that can be used to create a feed for Dash
 
Using existing docset
---------------------

For your convenience, the latest available docset is hosted on [Bintray](https://bintray.com/mathieu/CMake-docset/CMake-docset) :  [ Dash's  dash-feed ](http://tinyurl.com/cmake-dash) (via tinyurl)

```
dash-feed://https%3A%2F%2Fraw.github.com%2Fmathieu%2Fcmake-docset%2Fmaster%2FRelease%2FCMake.xml
```