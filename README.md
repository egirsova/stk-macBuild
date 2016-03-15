# Building SuperTuxKart on Mac OS X
This repository contains all of the necessary files to build SuperTuxKart on Mac OS X

## Getting Started
### Getting the Necessary Tools
Open up Terminal and type the commands below

1. Install Developer Tools if you don't have them already

	```
	xcode-select --install
	```

2. symlink include folder of OpenGL framework to /usr/local/include/GL

	```
	sudo ln -s /System/Library/Frameworks/OpenGL.framework/Versions/A/Headers/ /usr/local/include/GL
	```
	
**Note:** STK's [official wiki page](http://supertuxkart.sourceforge.net/Building_and_packaging_on_OSX) suggests executing the following code if step **2** does not work:

1. **Alternative to step 2**  
	Note the sdk number listed when you run `xcrun --show-sdk-path`:
	
	```
	$ xcrun --show-sdk-path
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.10.sdk
	```
	
	Replace `MacOSX10.9.sdk` in the below commands with the sdk number that you got from the previous command
	
	```
$ sudo ln -s  /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/usr/include/  /usr/include
$ sudo ln -s  /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk/System/Library/Frameworks/OpenGL.framework/Headers/ /usr/local/include/OpenGL
	```

	
### Getting the Source Code
1. Clone repository

	```
	git clone https://github.com/supertuxkart/stk-code.git stk-code
	```

2. Get Assets

	```
	svn checkout https://svn.code.sf.net/p/supertuxkart/code/stk-assets stk-assets
	```

**NOTE:** Make sure `stk-code` and `stk-assets` are downloaded/cloned to the same folder, so that the folder structure looks like this:

```	
- <Base Directory>
	- stk-code
		- ... 
	- stk-assets
		- ... 
```

## Building on Mac OS X 10.8 or Higher (64-bit)
### Getting the Dependencies
1. Use Finder to navigate to the `build-x86_64/build_dependencies` directory in this repository 
2. Copy the frameworks located here to `Library/Frameworks` on your Mac

### Building With CMake
1. Use Finder to navigate to the `build-x86_64/build_stk-code` directory
2. Move the `CMakeLists.txt` file located here to the source root folder that you cloned (`stk-code`). If prompted, select "Replace".
3. Use Finder to navigate to the `build-x86_64/build_stk-code/lib` directory in this repository
4. Move the `CMakeLists.txt` files associated with the libraries in this directory to the corresponding locations in `stk-code/lib`. **DO NOT** replace the entire library folder; just replace the `CMakeLists.txt` files within the folders.
5. In Terminal, navigate to the cloned `stk-code` directory using the `cd` command
5. To build with clang, run the following commands:

	```
	mkdir cmake_build && cd cmake_build
	cmake ..
	make
	```

## Optional
### Setting up Xcode as the IDE
1. If you don't have Xcode installed, download it from [here](https://developer.apple.com/xcode/download/) and install it
2. Place an additional copy of the frameworks in this repo (`build-x86_64/build_dependencies`) into `Users/USERNAME/Library/Frameworks` on your Mac <sup id="a1">[1](#f1)</sup>
3. In Terminal, navigate to your cloned `stk-code` directory 
4. Execute the following commands:

	```
	mkdir xcode_build && cd xcode_build
	cmake .. -GXcode
	```

5. Use Finder to navigate to your `stk-code/xcode_build` folder and open the newly generated Xcode project (`SuperTuxKart.xcodeproj`)
6. Build the project in Xcode: Product -> Build

### Setting up Qt as the IDE
1. If you don't have Qt installed, download it from [here](http://www.qt.io/download/) and install it
2. Create a new folder in `stk-code` and call it `qt_build`
3. Open Qt Creator and complete the following steps
	1. File -> Open File or Project
	2. Navigate to `/stk-code/CMakeLists.txt` and select it
	3. Choose your CMake executable if prompted
	4. Select the `stk-code/qt_build` folder as the build location
	5. Run CMake
4. Build the project in Qt: Build -> Build Project

## Credits
This information is based on STK's [official wiki page](http://supertuxkart.sourceforge.net/Building_and_packaging_on_OSX).

<br>
<b id="f1">1</b> It's necessary to also add the dependencies to `Users/USERNAME/Library/Frameworks` because for some reason even if you explicitly specify `/Library/Frameworks` in Build Settings, Xcode can't seem to find them (but has no problem finding the ones in the user frameworks location) . [↩](#a1)