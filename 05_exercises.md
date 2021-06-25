---
title: 'Weekly Exercises #5'
author: "Yike Zhou"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
library(ggimage)
library(scales)
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.

```r
beets_graph <- garden_harvest%>%
  filter(vegetable=="beets")%>%
  group_by(variety,date)%>%
  summarize(total_harvest=sum(weight))%>%
  mutate(wt_lbs = total_harvest*0.00220462,cum_harvest = cumsum(wt_lbs))%>%
  ggplot(aes(y=cum_harvest,x=date,color=variety))+
  geom_line()
ggplotly(beets_graph,
         tooltip = c("text","x"))
```

<!--html_preserve--><div id="htmlwidget-49c3a406d186605fa9f5" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-49c3a406d186605fa9f5">{"x":{"data":[{"x":[18450,18451,18463,18470,18487],"y":[0.13668644,0.3196699,0.55556424,0.88405262,7.0217147],"text":["date: 2020-07-07","date: 2020-07-08","date: 2020-07-20","date: 2020-07-27","date: 2020-08-13"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"Gourmet Golden","legendgroup":"Gourmet Golden","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18424,18431,18432,18434],"y":[0.01763696,0.07275246,0.09700328,0.22266662],"text":["date: 2020-06-11","date: 2020-06-18","date: 2020-06-19","date: 2020-06-21"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,186,56,1)","dash":"solid"},"hoveron":"points","name":"leaves","legendgroup":"leaves","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18450,18452,18455,18461,18470,18473,18487],"y":[0.0220462,0.17416498,0.37037616,0.7495708,0.85759718,1.0802638,6.38678414],"text":["date: 2020-07-07","date: 2020-07-09","date: 2020-07-12","date: 2020-07-18","date: 2020-07-27","date: 2020-07-30","date: 2020-08-13"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(97,156,255,1)","dash":"solid"},"hoveron":"points","name":"Sweet Merlin","legendgroup":"Sweet Merlin","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":26.2283105022831,"r":7.30593607305936,"b":40.1826484018265,"l":31.4155251141553},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18420.85,18490.15],"tickmode":"array","ticktext":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"tickvals":[18428,18444,18458,18475,18489],"categoryorder":"array","categoryarray":["Jun 15","Jul 01","Jul 15","Aug 01","Aug 15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"date","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.332566927,7.371918587],"tickmode":"array","ticktext":["0","2","4","6"],"tickvals":[0,2,4,6],"categoryorder":"array","categoryarray":["0","2","4","6"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"cum_harvest","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.93503937007874},"annotations":[{"text":"variety","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1d0a37632575":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"1d0a37632575","visdat":{"1d0a37632575":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
vegetable_graph <- garden_harvest%>%
  select(vegetable, date, weight)%>%
  mutate(wt_lbs=weight*0.00220462)%>%
  filter(vegetable%in%c("lettuce", "radish","spinach","beets","kale","peas","chives","strawberries","asparagus","Swiss chard"))%>%
  group_by(vegetable, date)%>%
  summarize(daily_wt_lbs=sum(wt_lbs))%>%
  mutate(cum_wt_lbs=cumsum(daily_wt_lbs), max_cumwt_lb = max(cum_wt_lbs), vegetable = str_to_title(vegetable))%>%
  ggplot(aes(x=date,
             y=cum_wt_lbs,
             color=fct_reorder(vegetable, max_cumwt_lb,.fun = max)))+
  ggtitle("cumulative weight of 10 vegetables")+
  labs(x="", y="")+
  geom_line()+
  theme(panel.grid.major.x = element_blank(),panel.grid.minor.x = element_blank(),legend.title=element_blank())
ggplotly(vegetable_graph,
         tooltip = c("text","x"))
```

<!--html_preserve--><div id="htmlwidget-22f32c0584c03882c41a" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-22f32c0584c03882c41a">{"x":{"data":[{"x":[18430],"y":[0.01763696],"text":"date: 2020-06-17","type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"Chives","legendgroup":"Chives","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18433],"y":[0.0440924],"text":"date: 2020-06-20","type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(216,144,0,1)","dash":"solid"},"hoveron":"points","name":"Asparagus","legendgroup":"Asparagus","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18419,18424,18426,18433,18434,18446,18450,18456,18470],"y":[0.07936632,0.22707586,0.34392072,0.37919464,0.46076558,0.65477214,0.7495708,0.8598018,0.94578198],"text":["date: 2020-06-06","date: 2020-06-11","date: 2020-06-13","date: 2020-06-20","date: 2020-06-21","date: 2020-07-03","date: 2020-07-07","date: 2020-07-13","date: 2020-07-27"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(163,165,0,1)","dash":"solid"},"hoveron":"points","name":"Radish","legendgroup":"Radish","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18431,18435,18439,18454,18456,18460,18462,18467,18470,18474],"y":[0.0881848,0.13007258,0.16755112,0.33730686,0.52469956,0.71870612,0.80027706,1.00530672,1.25442878,1.30513504],"text":["date: 2020-06-18","date: 2020-06-22","date: 2020-06-26","date: 2020-07-11","date: 2020-07-13","date: 2020-07-17","date: 2020-07-19","date: 2020-07-24","date: 2020-07-27","date: 2020-07-31"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(57,182,0,1)","dash":"solid"},"hoveron":"points","name":"Strawberries","legendgroup":"Strawberries","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18424,18426,18430,18431,18432,18433,18434,18435,18436,18438,18440,18441,18443,18445,18454,18458,18464,18474,18478,18491,18492],"y":[0.01984158,0.05070626,0.17857422,0.3086468,0.43651476,0.49163026,0.7605939,0.84216484,0.93255426,0.9810559,1.1133331,1.33159048,1.50796008,1.543234,1.58512178,1.67110196,1.71739898,1.7857422,1.88274548,1.94888408,2.03486426],"text":["date: 2020-06-11","date: 2020-06-13","date: 2020-06-17","date: 2020-06-18","date: 2020-06-19","date: 2020-06-20","date: 2020-06-21","date: 2020-06-22","date: 2020-06-23","date: 2020-06-25","date: 2020-06-27","date: 2020-06-28","date: 2020-06-30","date: 2020-07-02","date: 2020-07-11","date: 2020-07-15","date: 2020-07-21","date: 2020-07-31","date: 2020-08-04","date: 2020-08-17","date: 2020-08-18"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,191,125,1)","dash":"solid"},"hoveron":"points","name":"Spinach","legendgroup":"Spinach","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18426,18434,18457,18461,18462,18463,18468,18472,18477,18482,18489,18491,18498,18517,18525,18530,18536,18537,18539,18547],"y":[0.0220462,0.1543234,0.43651476,0.57099658,0.82011864,1.10231,1.36906902,1.98636262,2.83073208,3.50314118,3.6596692,4.04106846,4.299009,4.53710796,4.89646102,4.95819038,5.23817712,5.55784702,5.6438272,5.94586014],"text":["date: 2020-06-13","date: 2020-06-21","date: 2020-07-14","date: 2020-07-18","date: 2020-07-19","date: 2020-07-20","date: 2020-07-25","date: 2020-07-29","date: 2020-08-03","date: 2020-08-08","date: 2020-08-15","date: 2020-08-17","date: 2020-08-24","date: 2020-09-12","date: 2020-09-20","date: 2020-09-25","date: 2020-10-01","date: 2020-10-02","date: 2020-10-04","date: 2020-10-12"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,191,196,1)","dash":"solid"},"hoveron":"points","name":"Kale","legendgroup":"Kale","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18434,18443,18451,18459,18463,18466,18483,18484,18487,18509,18514,18530,18532,18535,18545,18552],"y":[0.07054784,0.14109568,0.3527392,0.41667318,0.80909554,1.83644846,2.5022437,3.18347128,4.32325982,4.88764254,5.3902959,5.44320678,5.95467862,6.14868518,6.19939144,6.88282364],"text":["date: 2020-06-21","date: 2020-06-30","date: 2020-07-08","date: 2020-07-16","date: 2020-07-20","date: 2020-07-23","date: 2020-08-09","date: 2020-08-10","date: 2020-08-13","date: 2020-09-04","date: 2020-09-09","date: 2020-09-25","date: 2020-09-27","date: 2020-09-30","date: 2020-10-10","date: 2020-10-17"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,176,246,1)","dash":"solid"},"hoveron":"points","name":"Swiss Chard","legendgroup":"Swiss Chard","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18419,18421,18422,18424,18426,18430,18431,18432,18433,18434,18435,18437,18438,18440,18441,18442,18444,18445,18446,18447,18449,18450,18451,18452,18454,18455,18456,18459,18463,18465,18466,18467,18469,18470,18471,18472,18473,18474,18477,18478,18480,18481,18485,18486,18488,18489,18490,18491,18492,18494,18497,18498,18501,18502,18521,18531,18532,18542],"y":[0.0440924,0.0771617,0.0992079,0.12566334,0.16755112,0.27337288,0.37699002,0.54674576,0.63493056,0.84436946,0.99869286,1.2676565,1.3337951,1.64464652,1.88935934,2.2487124,2.3809896,2.69845488,3.65305534,3.97713448,4.39380766,4.57017726,4.82150394,4.95598576,5.13015074,5.3131342,5.732012,5.86649382,6.13766208,6.18836834,6.47496894,6.51024286,6.68881708,6.90707446,7.10769488,7.26863214,7.47586642,7.71176076,7.85506106,7.97851978,8.19457254,8.23646032,8.43928536,8.60022262,8.77659222,8.90005094,8.99925884,9.14696838,9.33877032,10.26691534,10.51162816,10.80704724,10.83791192,11.02530462,11.04294158,11.25238048,11.55882266,11.5963012],"text":["date: 2020-06-06","date: 2020-06-08","date: 2020-06-09","date: 2020-06-11","date: 2020-06-13","date: 2020-06-17","date: 2020-06-18","date: 2020-06-19","date: 2020-06-20","date: 2020-06-21","date: 2020-06-22","date: 2020-06-24","date: 2020-06-25","date: 2020-06-27","date: 2020-06-28","date: 2020-06-29","date: 2020-07-01","date: 2020-07-02","date: 2020-07-03","date: 2020-07-04","date: 2020-07-06","date: 2020-07-07","date: 2020-07-08","date: 2020-07-09","date: 2020-07-11","date: 2020-07-12","date: 2020-07-13","date: 2020-07-16","date: 2020-07-20","date: 2020-07-22","date: 2020-07-23","date: 2020-07-24","date: 2020-07-26","date: 2020-07-27","date: 2020-07-28","date: 2020-07-29","date: 2020-07-30","date: 2020-07-31","date: 2020-08-03","date: 2020-08-04","date: 2020-08-06","date: 2020-08-07","date: 2020-08-11","date: 2020-08-12","date: 2020-08-14","date: 2020-08-15","date: 2020-08-16","date: 2020-08-17","date: 2020-08-18","date: 2020-08-20","date: 2020-08-23","date: 2020-08-24","date: 2020-08-27","date: 2020-08-28","date: 2020-09-16","date: 2020-09-26","date: 2020-09-27","date: 2020-10-07"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(149,144,255,1)","dash":"solid"},"hoveron":"points","name":"Lettuce","legendgroup":"Lettuce","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18424,18431,18432,18434,18450,18451,18452,18455,18461,18463,18470,18473,18487],"y":[0.01763696,0.07275246,0.09700328,0.22266662,0.38139926,0.56438272,0.7165015,0.91271268,1.29190732,1.52780166,1.96431642,2.18698304,13.63116546],"text":["date: 2020-06-11","date: 2020-06-18","date: 2020-06-19","date: 2020-06-21","date: 2020-07-07","date: 2020-07-08","date: 2020-07-09","date: 2020-07-12","date: 2020-07-18","date: 2020-07-20","date: 2020-07-27","date: 2020-07-30","date: 2020-08-13"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(231,107,243,1)","dash":"solid"},"hoveron":"points","name":"Beets","legendgroup":"Beets","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[18430,18433,18435,18436,18437,18439,18440,18441,18442,18445,18447,18449,18451,18456,18457,18462,18463],"y":[0.28439598,0.76720776,0.89728034,1.34922744,1.42418452,2.36114802,3.09528648,4.84355014,7.45822946,10.85554888,12.49137692,13.55179914,14.27270988,14.36089468,15.97688114,16.28552794,17.02628026],"text":["date: 2020-06-17","date: 2020-06-20","date: 2020-06-22","date: 2020-06-23","date: 2020-06-24","date: 2020-06-26","date: 2020-06-27","date: 2020-06-28","date: 2020-06-29","date: 2020-07-02","date: 2020-07-04","date: 2020-07-06","date: 2020-07-08","date: 2020-07-13","date: 2020-07-14","date: 2020-07-19","date: 2020-07-20"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(255,98,188,1)","dash":"solid"},"hoveron":"points","name":"Peas","legendgroup":"Peas","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":22.648401826484},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"cumulative weight of 10 vegetables","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[18412.35,18558.65],"tickmode":"array","ticktext":["Jun","Jul","Aug","Sep","Oct"],"tickvals":[18414,18444,18475,18506,18536],"categoryorder":"array","categoryarray":["Jun","Jul","Aug","Sep","Oct"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.832795205,17.876712425],"tickmode":"array","ticktext":["0","5","10","15"],"tickvals":[0,5,10,15],"categoryorder":"array","categoryarray":["0","5","10","15"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":1},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1d0a2f818115":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"1d0a2f818115","visdat":{"1d0a2f818115":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).

I want to tell the total average delay time of trains departing from Paris Lyon station over years.

```r
train_anim <- small_trains %>% 
  filter(departure_station == "PARIS LYON") %>% 
  group_by(year, departure_station, arrival_station) %>% 
  summarize(total_avg_delay_all_departing = sum(avg_delay_all_departing)) %>% 
  filter(!arrival_station %in% c("LAUSANNE", "BARCELONA", "ZURICH", "ITALIE")) %>%  # They are missing in some of the years
  na.omit() %>%
  ggplot(aes(x = total_avg_delay_all_departing, 
             y = fct_reorder(arrival_station,
                             total_avg_delay_all_departing,
                             .desc = FALSE))) +
  geom_col() +
  labs(title = "Total avgerage delay departing from Paris Lyon over years",
       subtitle = "Year: {closest_state}",
       x = "",
       y = "") +
  transition_states(year)

anim_save("2_train.gif", train_anim)
```

```r
knitr::include_graphics("2_train.gif")
```

![](2_train.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
tomato_harvest <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>% 
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb))
```

```r
tomato_anim <- tomato_harvest %>% 
  ggplot(aes(x = date, 
             y = cum_harvest_lb)) +
  geom_area(aes(fill = fct_reorder(.f = variety, 
                                   .x = cum_harvest_lb, 
                                   .fun = max,
                                   .desc = FALSE)), 
            position = "stack") +
  labs(title = "Cumulative harvest of tomato varieties over time",
       x = "Date",
       y = "Cum harvest (lb)",
       fill = "Variety") +
  transition_reveal(date)

anim_save("3_tomato.gif", tomato_anim)
```

```r
knitr::include_graphics("3_tomato.gif")
```

![](3_tomato.gif)<!-- -->


## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
 
 I prefer this to the static map, cause this animated map can show the moving path of the bicycle in more vivid way that I can follow up with much better.

```r
mallorca <- get_stamenmap(
  bbox = c(left = 2.2, bottom = 39.4, right = 2.9, top = 39.8),
  maptype = "terrain",
  zoom = 11)

bike_image <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png"

bike_anim <- 
  ggmap(mallorca) +
  geom_point(data = mallorca_bike_day7,
            aes(x = lon, y = lat),
            color = "red") +
  geom_path(data = mallorca_bike_day7,
            aes(x = lon, y = lat, color = ele),
            size = 4) +
  geom_image(data = mallorca_bike_day7 %>% 
               mutate(image = bike_image),
             aes(image = image)) +
  labs(title = "Mallorca bike route",
       subtitle = "Time: {frame_along}") +
  scale_color_viridis_c(option = "magma") +
  theme_map() +
  theme(legend.background = element_blank()) +
  transition_reveal(time)

anim_save("4_bike.gif", bike_anim)
```
  

```r
knitr::include_graphics("4_bike.gif")
```

![](4_bike.gif)<!-- -->

  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_all <- bind_rows(panama_swim, panama_bike, panama_run)

 swim_image <- "https://github.com/llendway/animation_and_interactivity/blob/master/swimmer.jpg"
 bike_image <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png"
run_image <- "https://github.com/llendway/animation_and_interactivity/blob/master/runner.png"

panama <- get_stamenmap(
  bbox = c(left = -79.6, bottom = 8.91, right = -79.5, top = 8.99),
  maptype = "terrain",
  zoom = 14)

panama_anim <- 
  ggmap(panama) +
  geom_point(data = panama_all,
             aes(x = lon, y = lat,col = event),
             size = 3) +
  geom_path(data = panama_all,
            aes(x = lon, y = lat ),
            size = .6) +
  labs(title = "Panama triathlete route",
       subtitle = "Time: {frame_along}") +
  scale_color_manual(values=c("red1", "sienna2", "dodgerblue3")) +
  theme_map() +
  theme(legend.background = element_blank()) +
  transition_reveal(time)

anim_save("5_panama.gif", panama_anim)
```

```r
knitr::include_graphics("5_panama.gif")
```

![](5_panama.gif)<!-- -->

  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  
  Comment: The growth rate of new cases in most states first soars to the top and then start dropping as the accumulated cases become larger and larger. 
 

```r
covid_lag <- covid19 %>% 
  group_by(state) %>% 
  mutate(case_lag_7day = lag(cases, 7, order_by = date)) %>% 
  replace_na(list(case_lag_7day = 0)) %>% 
  mutate(new_case = cases - case_lag_7day) %>% 
  filter(cases >= 20)

covid_gganim <- covid_lag %>% 
  ggplot(aes(x = cases, y = new_case, group = state)) +
  geom_point() +
  geom_path() +
  geom_text(aes(label = state), check_overlap = TRUE) +
  scale_x_log10(labels = comma) +
  scale_y_log10(labels = comma) +
  labs(title = "Trajectory of US COVID-19 Confirmed Cases",
       subtitle = "Date: {frame_along}",
       x = "Totel Confirmed Cases",
       y = "New Confirmed Cases (in the Past Week)") +
  transition_reveal(date)

animate(covid_gganim, nframes = 200, duration = 30)
anim_save("6_covid.gif", covid_gganim)
```

```r
knitr::include_graphics("6_covid.gif")
```

![](6_covid.gif)<!-- -->
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  
Comment: We can see the gradual increase of number of cases reported across the United States as the color changes from blue(smallest number of cases) to yellow(largest number of cases).We can also notice that the number of cases reported on the both Northwestern corner and the Northeastern corner of the United States is relatively smaller than the other states.


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")

cases_10000 <- covid19 %>% 
  mutate(state = str_to_lower(state))%>%
  left_join(census_pop_est_2018,
            by = "state") %>% 
  mutate(day = wday(date, label = TRUE)) %>% 
  filter(day == "Fri") %>% 
  mutate(cases_per_10000 = (cases/est_pop_2018)*10000)

covid_gganim2 <-
  cases_10000 %>% 
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = cases_per_10000,
               group = date)) +
  expand_limits(x = states_map$long, y = states_map$lat) +
  labs(title = "cumulative cases per 10000 people over time",
       subtitle = "Date: {closest_state}",
       fill = "Cases per 10000") +
  scale_fill_viridis_c(option = "plasma") +
  theme_map() +
  theme(legend.background = element_blank()) +
  transition_states(date)

#animate(covid_gganim2, duration = 15)
anim_save("7_covid.gif", covid_gganim2)
```

```r
knitr::include_graphics("7_covid.gif")
```

![](7_covid.gif)<!-- -->

## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.



