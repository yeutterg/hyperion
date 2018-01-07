# Hyperion Installation on MacOS

Updated 2018-01-06

This guide details the installation of [Hyperion](https://hyperion-project.org/), an application to mimic the functionality of [Ambilight]() or [DreamScreen](), on MacOS. 

Hyperion does not come pre-packaged for MacOS, so you have to compile it yourself.

A [similar guide](https://github.com/hyperion-project/hyperion/blob/master/CompileHowto.txt) is provided on the Hyperion Project GitHub, but is not specific to Mac systems and could lead to confusion. In addition, I had a number of build issues with the stable version of MacOS High Sierra. So, this guide is written for the next generation (alpha) version of the software, [Hyperion.ng](https://github.com/hyperion-project/hyperion.ng).

This guide was tested on a system running MacOS High Sierra. I cannot guarantee this will work smoothly on your system. This is alpha software, so expect issues even after install.

## 1. Install Xcode and Command Line Tools

Open the Mac App Store (App Store.app). Search for and install Xcode. Your system may need to restart to complete the installation.

Once that's complete, install Xcode's command line tools. Open Terminal.app and run the following command:

```
xcode-select --install
```

## 2. Install or Update Homebrew

You will need the Mac package manager Homebrew to download dependencies. You can learn more about Homebrew [here](https://brew.sh/).

In Terminal.app, run the following command:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

If you already have Homebrew, update it:

```
brew update && brew upgrade
```

## 3. Install Dependencies

Now run these commands:

```
brew tap cartr/qt4
brew tap-pin cartr/qt4
brew install git cmake qt@4 libusb python doxygen
```

If you get a warning about Python not being linked (probably because Python 3+ is linked), run this command:

```
brew link python
```

If you get any errors, you'll want to dig into those before proceeding.

## 4. Download Hyperion

Consider where you would like to locate the "Hyperion" directory. A logical place might be in Macintosh HD > Applications (the Applications folder). I will use the Applications folder in this example.

Open Terminal, cd to the Applications directory, and add the Hyperion folder as an environment variable:

```
cd /Applications
export HYPERION_DIR="Hyperion"
```

Now clone the files from GitHub:

```
git clone --recursive https://github.com/yeutterg/hyperion.git "$HYPERION_DIR"
```

## 5. Build and Install Hyperion

Make a new folder called "build" in the Hyperion directory, then enter it:

```
mkdir "$HYPERION_DIR/build"
cd "$HYPERION_DIR/build"
```

Run cmake then make:

```
cmake -DENABLE_DISPMANX=OFF -DENABLE_SPIDEV=OFF -DENABLE_V4L2=OFF -DENABLE_OSX=ON ..
make -j $(nproc)
```

If everything works (you get the message '[100%] Built target hyperion-osx'), install Hyperion on your system:

```
sudo make install/strip
```

Then clean up:

```
strip bin/*
```

## 6. Run Hyperion

