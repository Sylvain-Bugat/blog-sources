+++
categories = ["blog","hugo"]
date = "2015-04-26T19:05:20+02:00"
description="Create a blog from scratch with Hugo"
keywords = ["blog creation","blog","hugo"]
title = "Blog creation with Hugo"
+++
***
[Hugo](http://gohugo.io/) is a nice and fast blog (or static sites) creation tools written in [Go](https://golang.org/).

# Installation
The installation of Hugo is very simple, you just have to download the package/executable from the [GitHub lastest release page](https://github.com/spf13/hugo/releases/latest) and install it or copy it in a directory accessible with the ```PATH``` variable.

### Debian/Raspbian
On Debian (and Raspbian), ```deb``` packages are available to download for i386, amd64 and armhf. .deb packages are very simple to install with ```dpkg```.

Example on Raspbian (Raspberry PI B model) ARM Linux (**take the lastest version if a more recent version is available!**):
```bash
wget https://github.com/spf13/hugo/releases/download/v0.13/hugo_0.13_armhf.deb
sudo dpkg -i hugo_0.13_armhf.deb
```
And it's done, just type ```hugo version``` to test the installation.

### Raw executable
On other x64 Linux/UNIX (**take the lastest version if a more recent version is available!**):
```bash
#get the hugo tarball
wget https://github.com/spf13/hugo/releases/download/v0.13/hugo_0.13_linux_amd64.tar.gz
tar -xzvf hugo_0.13_linux_amd64.tar.gz

#You can copy the hugo binary somewhere else before adding it to the PATH
cd hugo_0.13_linux_amd64
#A symbolic link or an alias can be defined to use directly hugo by typing "hugo" instead of "hugo_0.13_linux_amd64"
ln -s ln -s hugo_0.13_linux_amd64 hugo
#Temporary PATH export to test only the executable from anywhere
export PATH="${PATH}:${PWD}"
```
And it's done, just type ```hugo version``` to test the installation. You can customize the user account profile to automatically set the ```PATH``` variable.

# Initialize a new site

You can look at the official [Hugo quickstart video](http://gohugo.io/overview/quickstart/) to have another example.

### Create the site directories

Into the target empty directory use the ```hugo``` command with ```new site .``` arguments to create all
```bash
hugo new site .
```
The created files and directories are:
```bash
archetypes
config.toml
content
data
layouts
static
```
The ```config.toml``` file is the main configuration file and the ```content``` directory will contain site pages.

### Installing a theme

A least one theme is required to skin the site. Available themes are listed on this dedicated [GitHub project](https://github.com/spf13/hugoThemes).

By using the ```git``` command the [hugo base theme] can be get from the site main directory:
```bash
mkdir -p themes
cd themes
git clone https://github.com/crakjie/hugo-base-theme.git
```
If you don't have git installed, just download [the zip archive](https://github.com/crakjie/hugo-base-theme/archive/master.zip) from this theme repository and unzip it in your themes sub-directory.

### Installing all themes

It's possible to install all themes at once with the ```git``` command from the site main directory:
```bash
git clone --recursive https://github.com/spf13/hugoThemes.git themes
```
All themes can be tested to see which one do you prefer. **Note: some seems partially or totally broken due to Hugo updates?**

### Configure the site

The sample configuration file contains:
```bash
baseurl = "http://yourSiteHere/"
languageCode = "en-us"
title = "My New Hugo Site"
```
To configure the installed theme, just add this line:
```bash
theme="hugo-base-theme"
```
Other parameters can be modified as you wish.

### Creating a blog post

From the
```bash
hugo new post/test.md 
```
This command will create a ```test.md``` file into the ```content/post``` directory with some header fields:
```
+++
date = "2015-04-29T23:10:31+02:00"
title = "test"
weight = 5

[menu]
  [menu.main]
    parent = "x"

+++
```
You can write a test content like **Your skill is great!** after this header.

# Test the new site

Now test the site with this command, **let's rock**:
```bash
hugo server
```
The default server URL is <a href="http://localhost:1313/">http://localhost:1313/</a> and the site must be displayed on your browser.
