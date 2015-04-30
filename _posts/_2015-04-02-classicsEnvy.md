---
title:			"Classics Envy"
author:		Maxim Romanov
layout:		post
image:		/images/thematic_covers/greek_laptop.jpg
imagecredit:	"Unknown Maker. Grave Naiskos of an Enthroned Woman with an Attendant, about 100 B.C.,
			Marble; 94.6 x 120.7 x 21.6 cm (37 1/4 x 47 1/2 x 8 1/2 in.). 
			<a href='http://www.getty.edu/art/collection/objects/7009/unknown-maker-grave-naiskos-of-an-enthroned-woman-with-an-attendant-east-greek-about-100-bc/' target='_blank'>
			The J. Paul Getty Museum, Villa Collection, Malibu, California</a>."
categories:
  - Coding
  - Teaching
tags:
  - Blogpost
  - Mapping
---

# Classics Envy

“Envy is not a very good thing. Yet envy is precisely what an early Islamicist feels when he reads Roger Bagnall and Bruce Frier’s *The Demography of Roman Egypt*.” [^fn1] These words stuck in my head since the very moment I read them and over the past two years of working among and with the classicists my classics envy has been growing—on top of 300 original census declarations that were at the disposal of of the above mentioned scholars, there are way too many things to envy, especially when it comes to all things digital.

{% highlight R %}

# R

library(ggplot2)
library(maps)
library(mapdata)
library(rgeos)
library(maptools)
library(mapproj)
library(PBSmapping)
library(data.table)

xlim=c(-12,55); ylim=c(20,60)

worldmap=map_data("world")
setnames(worldmap,c("X","Y","PID","POS","region","subregion"))
worldmap=clipPolys(worldmap,xlim=xlim,ylim=ylim,keepExtra=TRUE)

dataFolder="" # ideally, full path to the folder
csvName=paste0(dataFolder,"pleiades-locations-20150316.csv")
locsRaw=read.csv(csvName,stringsAsFactors=F,header=T,sep=',')
# url: http://atlantides.org/downloads/pleiades/dumps/
# ---: download the latest csv, unzip 

periods=rbind(
  c("archaic","750-550BC"),
  c("classical","550-330BC"),
  c("hellenistic-republican","330-30BC"),
  c("roman","30BC-300CE"),
  c("late-antique","300-640CE")
  )

features=rbind(
  c("","locations"),
  c("settlement","settlements"),
  c("fort","forts"),
  c("temple","temples"),
  c("villa","villas"),
  c("station","stations"),
  c("theatre","theatres"),
  c("amphitheatre","amphitheatres"),
  c("church","churches"),
  c("bridge","bridges"),
  c("bath","baths"),
  c("cemetery","cemeteries"),
  c("plaza","plazas"),
  c("arch","archs")
)

land="grey"; water="grey80"; bgColor="grey80"
locPleiades=geom_point(data=locsRaw,color="grey70",alpha=.75,size=1,aes(y=reprLat,x=reprLong))

for (i in 1:nrow(features)) {
  locs=locsRaw[ with(locsRaw, grepl(features[i,1],featureTypes)),]
  for (ii in 1:nrow(periods)) {
    locPer=locs[ with(locs,grepl(periods[ii,1],timePeriodsKeys)),]
    locPer=geom_point(data=locPer,color="red",alpha=.75,size=1,aes(y=reprLat,x=reprLong))
    
    dataLabel="Data: Pleiades Project"
    fName=paste0(dataFolder,"Pleiades_",features[i,2],sprintf("%02d",ii),".png")
    header=paste0(features[i,2]," in the ",periods[ii,1]," period (",periods[ii,2],")")
    
    p=ggplot()+
      coord_map(xlim=xlim,ylim=ylim)+
      geom_polygon(data=worldmap,aes(X,Y,group=PID),size=0.1,colour=land,fill=water,alpha=1)+
      annotate("text",x=-11,y=21,hjust=0,label=dataLabel,size=3,color="grey40")+
      annotate("text",x=54,y=59,hjust=1,label=header,size=5,color="grey40")+ 
      locPleiades+ locPer+ labs(y="",x="")+theme_grey()
    
    ggsave(file=fName,plot=p,dpi=600,width=7,height=6)
  }
}

{% endhighlight %}

## Using Image Magick to animate maps
The fastest and easiest way to animate the results is to use [**ImageMagick**](http://www.imagemagick.org/), a free command-line utility. The following command will take all **.png** files whose names begin with **Pleiades\_Settle** and convert them into an animated GIF file **Pleiades\_Settlements.gif**, which will play continuously (`-loop 0`), with each frame downsized (`-resize 1200x900`) and paused for .75 of a second (`-delay 75`).

```
convert -resize 1200x900 -delay 75 -loop 0 Pleiades_Settle*.png Pleiades_Settlements.gif
```



## Footnotes

[^fn1]: al-Qādī, Wadād. “Population Census and Land Surveys under the Umayyads (41-132/661-750).” Der Islam 83, no. 2 (2006), p. 341