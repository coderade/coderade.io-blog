+++
date = "2017-09-24T13:47:08+02:00"
tags = ["android", "mobile"]
categories = ["android"]
title = "Converting graphic files to WebP format on Android Studio 2.3"
description = "Using WebP graphic files instead of JPG or PNG files can help you dramatically shrink the size of your Android package application."
meta_image = "images/posts/android/converting-graphic-files-to-webp-format/webp-png-comparison.png"
+++

For all who don't know the webP format, Android has supported this image format
for many years.

As I said on my [previous](/post/android/inspecting-apk-files-after-packaging)
article: the WebP files are smaller than JPGs or PNGs and like PNGs, they support transparency.
But creating them files has always been a bit of a chore.

Now, with Android Studio 2.3, you can convert your existing graphical files into
WebP files very easily.

On this article, I'll show how to convert your created image files to the WebP format
on the new version of the [Android Studio](https://developer.android.com/studio/index.html).
So if you haven't already switched for the new version, I think that it's
definitely time to do so now, since that this new version added
several quality improvements across the IDE
[comparing](https://android-developers.googleblog.com/2017/03/android-studio-2-3.html)
with older versions.

For this article, I'll work in a version of my project called
[CoffeeMenuSample](https://github.com/coderade/CoffeeMenuSample). This is the
same project that I used on my previous article and also is available on my GitHub.

It displays a list of data in a recycler view and each data item is associated
with an image file.
And, as my [previous](/post/android/inspecting-apk-files-after-packaging)
article, when I demonstrated the APK Analyzer tool, I found that my largest space
usage in the APK was from those image files.

## Converting a single image file to WebP

I'll show you first how to convert a single image file to WebP.

I'll do that with the JPG file that's in the drawable resource directory on
my project.

So, with the Android Studio running (of course :sweat_smile:), I'm going to right
click on that JPG file and choose Find Usages (CTRL + F7 on Linux/Windows,
  CMD + F7 on Mac) and I find a reference to the JPG file that is in the `activity_detail.xml`
  file.

![](/images/posts/android/converting-graphic-files-to-webp-format/webp-activity_xml-context.png)

On the Design tab for this file, I'll resize that to fit the screen and then
shrink down that window so it gets a little bit bigger so we can see how the
image looks like:

![](/images/posts/android/converting-graphic-files-to-webp-format/design-tab-activity-detail-xml.png)

Now, I'll go back to the project window and I'll right click on the file and
down at the bottom of the menu I'll choose Convert to WebP.

![](/images/posts/android/converting-graphic-files-to-webp-format/as-convert-to-webp.png)

A window will be opened with many options available:

![](/images/posts/android/converting-graphic-files-to-webp-format/convert-webp-options.png)

**Lossy encoding** means that you can shrink down the image quite a bit. You can
also choose **lossless encoding** and that will retain the image's resolution but
the file size won't shrink as much and more importantly, your app will now have
a minimum SDK of API 18.

This will work for the sample app, which has a minimum SDK of API 18, but for
this article I'll go back to lossy encoding.

You also have control on this screen of the encoding quality.

A good ideia is leave the **Preview/inspect each converted image before saving**
option selected so I can preview the image before I save the change.

There's some other options down here, but I'll leave those at their default selections
and click OK.

This will takes me to the preview screen and we see the original file on the left
and the new file that's going to be created on the right:

![](/images/posts/android/converting-graphic-files-to-webp-format/webp-preview-ajust-window.png)

And to my eye they look pretty much the same. We can move this slider to the left
to reduce the image quality and the difference between the images will be shown
in the center.

For this example I'll keep the default of 75% and then I'll click finish.

Then, there's the result. My Android Studio Event log logged the message:

![](/images/posts/android/converting-graphic-files-to-webp-format/webp-log.png)

showing that the image size has been reduced to a very considerable size. (In this
case I saved 507.8 KB :open_mouth:)

The WebP file has been created and if we look in the design environment again,
it looks pretty much the same:

![](/images/posts/android/converting-graphic-files-to-webp-format/webp-image-converted.png)

Now that we learned how to convert the images to a webP format and the result has
been approved (at least on my side), we can do now a mass conversion.

## Doing a mass conversion of image files to WebP

In my assets directory I'll click and select all of my files.
I will click on the first, held down the shift key and click on the last.

![](/images/posts/android/converting-graphic-files-to-webp-format/selecting-all-assets-files.png)

Then, once again, I'll right click and choose Convert to WebP, accept
all the default settings and click OK.


And now because I'm converting multiple files, I can click Next and Previous to
move through the files and see what they'll look like.
Once I liked my settings, I'll click **Accept All** and all of my files have been
converted to WebP.

The Android Studio will show a notification saying that we saved 3.3 MB after the
files conversion.

![](/images/posts/android/converting-graphic-files-to-webp-format/files-converted-webp.png)

This is really insane, if you think that we're talking about a mobile app.


Before you test this on a device, I need to make a change in one of the Java files,
because we changed the format of all the dummy images.

I'll go to my Java directory, to my base package, to the sample sub package and
I'll open the [SampleDataProvider.java](https://github.com/coderade/CoffeeMenuSample/blob/master/app/src/main/java/com/coderade/android/coffeemenusample/sample/SampleDataProvider.java) file.

We can notice that where I created the sample data to the project, there are
references to those old JPG files scattered throughout this file.

```java
static {
        dataItemList = new ArrayList<>();
        dataItemMap = new HashMap<>();

        addItem(new DataItem(null, "Hamburger", "Entrees",
                "The best burger in the world",
                4, 5, "hamburger.jpg"));
        //...//        
    }
```

We can press Ctrl + R on Windows or Cmd + R on Mac to replace all
the .jpg references it with .webp and then click on the `Replace All` button.

The result will be:

```java
static {
        dataItemList = new ArrayList<>();
        dataItemMap = new HashMap<>();

        addItem(new DataItem(null, "Hamburger", "Entrees",
                "The best burger in the world",
                4, 5, "hamburger.webp"));
        //...//        
    }
```

Before I deploy the application, we need to go back to the device and uninstall
the application.

In this way we are making sure I'm starting with a fresh data set.
In this example application the data that's provided by that Java class is cached
in a database and I want to start over without anything on the device.

And I'll run the app again and the result is that it looks exactly the same as it
did before.

If you are testing the app, you can move around and see all of the data and the
associated images.

But what I got with these changes? And about the resulting size of the application?
Well, to validate this just like I did in my [previous](/post/android/inspecting-apk-files-after-packaging)
article, I'll going to build an APK file to compare it with a old apk file which
I created before the graphical file conversion.


## Analyzing the APK file to compare the assets size

To understand how to build a APK file and how to use the APK Analyzer on the
Android Studio, please check my previous article:
[Inspect APK files with the APK Analyzer on Android Studio](/post/android/inspecting-apk-files-after-packaging).

To do this analysis, I'll choose Build and then Build APK on the Android Studio
menu. This will create a new APK for the app.

Then, once again, I'll choose Build, Analyze APK, and I'll choose my old APK
which I had created in my previous article, before the jpgs file conversion to webP files.

![](/images/posts/android/converting-graphic-files-to-webp-format/old-apk-file.png)

We can see that this APK file has a RAW File Size of **5.5 MB** and a Download
Size of **4.9MB**.

Then, I'll click the `Compare With` button and I'll navigate back to choose the
new APK file that I just build.

**Here's my comparison:**

![](/images/posts/android/converting-graphic-files-to-webp-format/old-app.apk-vs-new-app-apk.png)

We can see that my `assets` directory has shrunk down from 2.5 Megabytes all the
way to 635.1KB and the `res` directory has shrunk down from 879.5 Kilobytes to
370.6KB.

This application isn't a huge application, but you can see that with a simple files
conversion we can make a significant difference.


## Converting the webP files back to a PNG format

Finally, we need to understand how you can return to a more conventional graphic
format if we want to do that.

I'll go back to Android Studio and I'll choose the WebP file in my resources
directory.

I'll right click on it and now the menu choice says: `Convert to PNG`.

![](/images/posts/android/converting-graphic-files-to-webp-format/convert-webp-to-png.png)

I'll select that and when I'm prompted:

![](/images/posts/android/converting-graphic-files-to-webp-format/keep-files-AS-png.png)

I'll say No, I want to keep both versions of the file.

So now we can compare the PNG and a WebP files:

![](/images/posts/android/converting-graphic-files-to-webp-format/webp-png-comparison.png)

We can look on the on the png file and it shows that it is now 1.19M and if we check
the WebP file it shows that it's less than 60K. So that's a massive difference.

So, as we can see on this article, using WebP graphic files instead of JPG or PNG
files can help you dramatically shrink the size of your Android package application.

Many thanks for your reading and please take a look at the other blog posts when you
have a time.
