This repository is a starting point for making your own image collections that work with SIL software (e.g. Bloom & WeSay). It includes:

* a sample collection with a multilingual index
* a windows batch file that
    * converts all images to highly compressed png's
    * embeds intellectual property information in each image
    * optionally watermarks each image
    * optionally adds a prefix to each image file
* an installer setup


The following instructions are for people working in Windows, but could be readily adapted for Linux.

# 1) Clone this repository into your own github account.

(Click the green 'clone' button)

# 2) Set up programs on you machine

If you don't know about setting the PATH variable, this process is probably going to be more technical than you are used to. The first 3 programs below will each need to have an entry in your Windows PATH variable, and you're going to have to add it. There are various tutorials. Here's a good Google query to use: https://www.google.com/search?q=set+windows+path+variable+-java&oq=set+windows+path+variable+-java. Note that if you have a CMD window open, it won't "see" changes you've made to the PATH variable. You need to close the CMD window and re-open.

Add this folder (where this readme and process-images.bat are) to your PATH (this will help in finding pngout).

## ExifTool

ExifTool is used to embed your intellectual property information into each image. Get it at http://www.sno.phy.queensu.ca/~phil/exiftool/ and install it. In April 2018, it does not have an actual installer, so here are some steps:
1. Get the "windows executable", unzip it into this folder, alongside the file named 'process-images.bat'.
2. Rename it from "exiftool(-k).exe" to just "exiftool.exe".

Verify that if you open a new CMD window and cd to this folder, this command works:

    exiftool -ver

You should see something like

    x:\dev\image-collection-starter>exiftool -ver
    8.68

## ImageMagick

ImageMagick can be used to convert you images to PNG. If your collection contains jpeg photographs or very complicated color drawings that include shading and such, then this is not what you want. Please contact us.

Get it at http://www.imagemagick.org/script/download.php. Get the first choice under "Windows Binary Release", and verify that if you open a new CMD window, this command works:

    magick --version

You should see something like

    x:\dev\image-collection-starter>magick --version
    Version: ImageMagick 7.0.7-1 Q16 x64 2017-09-09 http://www.imagemagick.org
    Copyright: Copyright (C) 1999-2015 ImageMagick Studio LLC

## PNGOut.exe

PNGOut.exe is used to compress the daylights out of the png. Get it here: http://advsys.net/ken/utils.htm. Like exiftool, it lacks an installer. So put the file named pngout.exe in this folder, alongside the file named 'process-images.bat'. Type following command in a CMD window while cd'd into this folder:

    pngout

You should see a bunch of documentation on how to use pngout, which you can ignore.

## InnoSetup

InnoSetup will make your installer. Get the unicode version here: http://www.jrsoftware.org/isdl.php

# 2) See if you can create the Sample Collection

Before customizing anything, test out your setup by creating the sample collection that comes with this project. Open a command prompt window, cd to the project folder, and run

    process-images.bat

That should create an /output folder and an /output/processed-images folder, with two png image files.

Now make an an installer. Double click the "installer.iss" file. InnoSetup should run. Click "Build:Compile". That should create "/output/Shapes Collection 1.0.exe". Now run that program. When it is done, look in %programdata%\SIL\ImageCollections\. You should see a "Shapes".

# Setting up your image files

Copy your images into the /images folder. If your images are in various sub-folders, that's ok.

# Setting up your index

The index is a tab-delimited, unicode (utf-8) text file. The first row is heading and iso-639 language codes:

filename(tab)subfolder(tab)en(tab)fr(tab)es

There are only two required columns: filename and at least one language. *subfolder* is optional, use this if your files are not all together in one folder. Any other columns that have more than three letters will be ignored. So for example if you have and "artist" column, that's great, leave it in there. Maybe someday it will be of use.

Following that header row, your index needs a row for each image in the collection. Within a column of index terms for a language, terms are separated by commas.
For example, this row is for a file named "B-NA-2". It could end in ".tif", ".png", whatever, the index doesn't care. It is in a subfolder named "Brazil".

B-NA-2(tab)Brazil(tab)Romero Britto(tab)Brazil(tab)bird,bird of prey,hawk,flying(tab)птица,хищная птица(tab)burung,burung pemangsa

An easy mistake to make when creating index.txt is to lose the unicode (utf-8) formatting. For example, you might create the index in a spreadsheet program, then export as "tab delimited text". Make sure you tell it to create a unicode (utf-8) document, otherwise any non-Western character will be changed to a question mark.

You should also ensure that no file is listed twice. If you do, it will cause an error in Bloom or wherever your collection is used. One way to check for duplicates is, in your spreadsheet program, turn on "conditional formatting" where the condition is "duplicate".

# Processing your images

The idea is that we need an automated process that will take each image in the images/ directory, get it all ready, and deposit it in the /output/process-images directory. You never commit anything in /output to github; instead the installer maker will gather file from there when it makes the installer. For that we have process-images.bat.

What the process-images.bat batch file does:

For each image in /images/
	Make a PNG out of it in /output/processed-images, using ImageMagick.
	Compress it really well, using PNGOut.
	Push in metadata, using exiftool.

There is no currently no way here to read the index and embed things like the artist name or index terms into the image. You may notice that Art Of Reading images do have that, but that was done with some other process. We consider embedding the artist name to be very important and helpful, but that is not yet part of this project.

# Make the installer

Double click on the "installer.iss" file. Innosetup should open.

Replace the values of the sample collection with your own values.

Don't miss that you need to replace the appid guid there with a unique on for your library. Otherwise, installing one library would replace another on the user's computer.

Click "Build:Compile". That should create "/output/Shapes Collection 1.0.exe".

Now run that program. When it is done, look in %programdata%\SIL\ImageCollections\. You should see your collection. Finally, run an SIL program that allows inserting images from a gallery (e.g. Bloom 4.0 or greater), and search for one of your images.

---
## Relevant documentation on metadata

http://www.sno.phy.queensu.ca/~phil/exiftool/TagNames/XMP.htm

http://www.metadataworkinggroup.org/pdf/mwg_guidance.pdf
