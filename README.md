# Useful tools and commands   
[Andreas Windisch](https://www.linkedin.com/in/andreas-windisch-physics/)   

This repo serves as a place where I collect commands that I find useful. Since I do not use those commands 
on a daily basis, I intend to collect some of them here, so I don't have to browse the net to track them down in times of need.
Feel free to add your personal favorite commands by opening new sections in this document. You can also find and connect with me on [LinkedIn](https://www.linkedin.com/in/andreas-windisch-physics/).    
Cheers, Andreas.   


## PDFs and GhostScript
Ghost Script is really nice for dealing with PDFs. Here are two applications that I frequently encounter, and both can be addressed nicely using gs.   
### Merging PDFs with gs
In order to merge several PDFs into one output file, use:
```{bash}
gs -dNOPAUSE -sDEVICE=pdfwrite -sOUTPUTFILE=output.pdf -dBATCH input1.pdf input2.pdf
```
### Rasterizing PDFs into PNGs using gs
This is another useful command to rasterize large PDFs (for example, 3d-plots produced with Wolfram Mathematica) 
into PNGs. The rasterization can be done with a very high resolution, and the resulting size of the PNGs will still be reasonably small.
To rasterize the image, use:
```{bash}
gs -sDEVICE=png16m -dTextAlphaBits=4 -r300 -o output.png input.pdf
``` 

### Compressing PDFs with ghostscript
Sometimes it is necessary to compress pdf files. This can also be done with ghost script. Note that in some cases a particular approach can lead to an increase in file size. That happend to me as well in a particular case, and I had to try various different suggestions till I found one that worked. Here is an example that worked for me:
```{bash}
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```
The quality setting (and thus the rate of compression) in this case is controlled via the -dPDFSETTINGS switch, see [this blog post](https://blog.virtualzone.de/2012/11/how-to-reduce-pdf-file-size-in-linux.html). According to this post, the following settings are available.   
   

| Setting  | quality  |
|:--------:|:--------:|
|\/screen  | low      |
|\/ebook   | moderate |
|\/printer | good     |
|\/prepress| high     |
   

Here is another command that increased the file size in the case where the above command was successful, but it may work in other cases.   

```{bash}
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/default -dNOPAUSE -dQUIET -dBATCH -dDetectDuplicateImages -dCompressFonts=true -r150 -sOutputFile=output.pdf input.pdf
```
Feel free to add your own commands here or send them per email to andreas.windisch@yahoo.com so I can add them for you, in case you prefer that.   

## Using (non-WiFi) HP network scanner with linux command line   
I have a HP all-in-one network scanner/printer (Photosmart C5100) that I use in combination with the command line tool scanimage. For the printer side I just used the webinterface of CUPS to set up the printer. In order to use the network scanner from the command line you have to set it up first.   
```{bash}
#you need to install hplip for your distribution, then run:
hp-setup
```
Running this command positively identified the device on my network. Now make sure that scanimage recognizes the device.   
 
 ```{bash}
scanimage -L
#output:
#device `hpaio:/net/Photosmart_C5100_series?ip=XXX.XXX.XXX.XXX' is a Hewlett-Packard Photosmart_C5100_series all-in-one
```
Note: It could happen, that if the IP address of the scanner/printer changes over time, that you can't reach the scanner/printer on the network. In that case, just remove the device with the outdated IP address using the command   

```{bash}
hp-setup -r
```

Now you can scan directly from the command line. For example, I had to scan a nasty logbook with many pages and I intended to combine everything in a PDF afterwards. Here is what I did.   

```{bash}
#STEP 1: scan page by page to working directory (cd into wd first)
scanimage --resolution 200 > page000.jpg
scanimage --resolution 200 > page001.jpg
...
scanimage --resolution 200 > page110.jpg
```
Now use convert to arrange all the jpgs in one PDF:   
```{bash}
#STEP 2: Build PDF from jpgs
convert *.jpg all_pages.pdf
``` 
Finally compress the PDF to a reasonable size using ghost script, see also the PDF compression section of this document.   

```{bash}
#STEP 3: compress PDF
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/ebook -dNOPAUSE -dQUIET -dBATCH -sOutputFile=all_pages_compressed.pdf all_pages.pdf
```

## ffmpeg and imagemagick
Conversion of avi to mp4:
```{bash}
ffmpeg -i movie1.avi -sameq movie1.mp4

ffmpeg -i movie1.avi -qscale 0 movie1.mp4

ffmpeg -i movie1.avi -vcodec libx264 movie1.mp4

ffmpeg -i movie1.avi -vcodec msmpeg4v2 -acodec copy movie1_compressed.avi 
```
Create movie from pngs. The filename convention here is: pic_00.png, pic_01.png, .....
```{bash}

ffmpeg -framerate 10 -i pic_%02d.png -s:v 800x600 -c:v libx264 -profile:v high -pix_fmt yuv420p movie.mp4

```
Split animated gif into pngs:
```{bash}
convert animated.gif pic_%03d.png
```
Merge pngs into animated gif:
```{bash}
convert -delay 0.1 -loop 0 *.png animated.gif
```   
   
Compress a mp4 movie:
```{bash}
ffmpeg -i input.mp4 -vcodec h264 -acodec mp3 output.mp4
```   
   
Create an animated gif from a mp4 movie (with downscaling and time):   
```{bash}   
ffmpeg -i input.mp4 -r 25 -vf scale=512:-1 -ss 00:00:00 -to 00:00:16 output.gif
```
   
Convert from Matroska format to mp4:   
```{bash}
ffmpeg -i input.mkv -codec copy output.mp4
```   
   




