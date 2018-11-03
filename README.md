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


   




