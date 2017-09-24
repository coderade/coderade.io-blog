+++

date = "2017-09-22T13:47:08+02:00"
tags = ["android", "mobile"]
categories = ["android"]
title = "Inspect APK files with the APK Analyzer on Android Studio"
description = "If your APK file is getting too large, this tool can help you figure out where you might be able to reduce the APK and where there are possible optimizations."
meta_image = "images/posts/android/inspecting-apk-files-after-packaging/apk-analyzer-dex-methods.png"
+++

The Android Studio 2.2 added a new tool called the
[APK Analyzer](https://developer.android.com/studio/build/apk-analyzer.html).

It lets you look at packaged apps, those APK files that are in zip archive format
that you use to deploy an application.

Using the APK Analyzer, you can see inside
the APK file and see what files and directories are largest so you can decide how
you can shrink down that APK.

## The example application

![](/images/posts/android/inspecting-apk-files-after-packaging/app.png)

I'll show you how to use the Apk Analyzer tool with an existing project that is available on my
[GitHub](https://github.com/coderade/CoffeeMenuSample), where you can clone or
download and try to follow the below steps on your own Android Studio.

The application uses a RecyclerView to display a list of data. Each data item is
associated with an image file and when you click on the item, you see the image
file in a larger size.

Now in order to accomplish this, all of those image files are actually being
packaged with the application.

I'll open my Android Studio and from the welcome screen and open the CoffeeMenuSample
project which you can download on the above
[link](https://github.com/coderade/CoffeeMenuSample).

Then, I'll go to my Project window to the application and then to the assets directory
and we'll see a whole bunch of JPG files, as we can see on the below image.

![](/images/posts/android/inspecting-apk-files-after-packaging/assets-folder-coffeemenusample.png)

There will also be a copy of one of those JPG files in the drawable directory under
resources and that's used for Design Time.

## Building the APK file

Now I'm going to build an APK file. I'll go to the menu and choose Build, Build APK.

![](/images/posts/android/inspecting-apk-files-after-packaging/build-apk.png)

That will creates a debugged version of the application.

I'll click on Show in Explorer when the notification appears and that shows me
the location of the APK file.


## Analyzing the APK file

Now that we created the .apk file, we can use the APK Analyzer.

First, we need to stop the application if it's running and then go to the menu
and choose Build, Analyze APK.

![](/images/posts/android/inspecting-apk-files-after-packaging/using-apk-analyzer.png)

When presented with the directory tree, it defaults to selecting that new APK file.
It's called `app-debug.apk`.

I'll click OK and here's the breakdown of the APK file:

![](/images/posts/android/inspecting-apk-files-after-packaging/apk-analyzer.png)

It shows me the sizes of each of the directories and I can immediately see that
my assets directory is taking up more space than just about anything.

And I'll click on the tree icon over here and that expands and it shows
me the sizes of the actual files.

![](/images/posts/android/inspecting-apk-files-after-packaging/assets-apk-analyzer.png)

As there is many images with the JPG extension, for this example we could convert
these images to [WebP](https://developer.android.com/studio/write/convert-webp.html)
files.

WebP files are smaller than JPGs or PNGs, and like PNGs, they support transparency,
but I think that this kind of information may stick to an upcoming article,
so we wont' mix up the things.

### **Checking the .dex file information**

The Analyzer will also show you information about your dex file.

![](/images/posts/android/inspecting-apk-files-after-packaging/apk-analyzer-dex.png)

This is where your Java classes are compiled in an Android app.

The dex file is broken down by Java package and it includes Java code not just
for your custom application but for all of the libraries that your application
depends on.

And this includes a lot of code from the core Java Runtime.
To find your own files for the project, you can go to your package.

For this example, mine starts with `com.coderade.android` and I'll drill down until I see
my actual classes.

![](/images/posts/android/inspecting-apk-files-after-packaging/apk-analyzer-dex-methods.png)

Here we can see the listing here of the Defined Methods.
This can be a hint for ways you could optimize your application and you can keep
on clicking to find calls to various methods.

So that's the APK Analyzer. If your APK file is getting too large, this tool can
help you figure out where you might be able to reduce the APK and
where there are possible optimizations.
