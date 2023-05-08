---
icon: gear
---

![](../static/ehs.png)

# Extracting Hardcoded Subs

Follow these steps to manually extract hardcoded subs and change them to softcoded ones. This is quite a long method, so I will make another guide for a more better and faster one soon.

!!!danger I prepared screenshots to go with these steps, but dumbhead me lost them all when I did a pc reset so I will make new ones and add them later at some point.

!!!

---

## Required Softwares

- [ffmpeg](https://ffmpeg.org/)
- [Adobe Bridge](https://filecr.com/search/?query=Adobe%20Bridge)
- [Subtitle Edit](https://www.nikse.dk/subtitleedit)

---

## Process

=== Step 1: 
You can use anyformat for the video but, I will convert the video to mp4 format first.

DO:
```
 ffmpeg -i videoinmkv.mkv -c:v copy -c:a copy videotomp4.mp4
```

===

=== Step 2: 
Crop the video with the subs part only.
For a 1980*1280 video I did the following crop:

DO: 
```
ffmpeg -i video.mp4 -filter:v "crop=1850:250:0:830" -c:a copy video-cropped.mp4
```
===

=== Step 3:
 Now, add a timestamp to your cropped video.
DO: 
```
ffmpeg -i video-cropped.mp4 -vf "drawtext=fontfile=PlusJakartaSansRegular.ttf:text='%{pts \: hms}': x=0: y=40: 
fontsize=56:fontcolor=yellow: box=1: boxcolor=black" -c:a copy video-cropped-timestamps.mp4
```

!!!
I used `PlusJakartaSansRegular` font but you can use any other font and make sure the font file is properly installed.
!!!

===

=== Step 4:
 Create image files for every second (for long movies create img files every 2 or even 3 secs).

DO for every 1 secs: 
```
ffmpeg -i video-cropped-timestamps.mp4 -start_number 1 -vf fps=1 video-%04d.jpg
```
DO for every 2 secs: 
```
ffmpeg -i video-cropped-timestamps.mp4 -start_number 1 -vf fps=1/2 video-%04d.jpg
```
DO for every 3 secs: 
```
ffmpeg -i video-cropped-timestamps.mp4 -start_number 1 -vf fps=1/3 video-%04d.jpg
```
===

!!!
PS: I had a 48 min long video for which I created images every 3 sec and got 969 images. That's a lot of images to go through so do not even try every 1 or 2 sec for long videos.
!!!

=== Step 5:
 Now we will seperate the duplicate and empty image files using Adobe Bridge. Use the label feature of adobe bridge to label the images apart from the duplicate and empty ones and do `sort by label`. Then select all the images and download them to a different folder.

Change the mode to `Filmstrip` (top-mid) and use arrow keys to go through the images. When you see subs add label using Ctrl + 6. Do this for all images excluding empty and duplicate ones.
===

=== Step 6:
 Now select all the images, right click and print them all to one pdf. (Make sure to uncheck `fit picture to frame` otherwise the photo will be cropped which we really don't want).
===

=== Step 7:
 Then, you can either use an OCR tool that can extract text from the whole pdf, but such tools are hard to find/are locked behind paywall. What I like to do is split the pdf into multiple pdfs with 60 pages each using Adobe acrobat tool online and use google docs to extract the text.

!!!warning Warning
Google docs has 60 page ocr limit. 
!!!

[Adobe Acrobat tool](https://www.adobe.com/acrobat/online/split-pdf.html)
===

=== Step 8:
 Upload all the pdfs to google drive, do right click and `open with docs`. Opening with google docs will automatically extract the sub text along with the timestamp. Open each file and copy all the text to a notepad. Repeat for all pdfs.
===

=== Step 9:
 Now it's time to correct the subs. For each timestamp move the subs inline (timestamps and the correspoding subs on the same line). 
 
 Doing that manually will take a lot of time so you can automate it with python using regex or something of your choice. I used the substitution function in this [Regex101](https://regex101.com) website, so didn't had to write any code.

After arranging it like said so, make a new txt file and save the timestamps only, and make another txt and save the text only. Donot change the format.
===

=== Step 10:
 Now, we're going to use `Subtitle Edit` program to make the srt file. Go to `File> Import> Plain text` and open the text only file. Make sure `One line is one subtitle` checkbox is selected and then click ok. 
 
 Now again, go to import and import the time only file with `Time Code` option. Then go to tools and and choose `adjust durations`, check the `recalcuate` option and click ok. You will now get a base subtitle file that you can work with. 
===

=== Step 11:
 Correct, correct and correct! Use the waveform feature to correct. Trust me this tool will make it 100 times easier. You will now have to give time to correct the timing and any missing text and after done save it as `something.srt`. 
===