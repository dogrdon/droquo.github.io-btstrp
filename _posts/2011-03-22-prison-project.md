---
layout: project_post
title: "Department of Justice Data Analysis"
description: ""
category: project
tags: [R, data analysis]
---
{% include JB/setup %}

<h3>A Look at the US Prison System: Inmate Demographics circa 2004</h3>

<p>The data we looked at below was pulled from the United States Department of Justice’s National Corrections Reporting Program, 2004, a survey that “gathers data on prisoners entering and leaving the custody or supervision of state and federal authorities.” [source: Department of Justice. National Corrections Reporting Program, 2004 [United States] Codebook. Inter-University Consortium for Political and Social Research, Ann Arbor, MI]</p>

<p>The full data set contains 100 variables and ~1.3 million records. For this exploration, we pulled the following variables: Year of birth, Race, Gender, Hispanic Origin, Education (last year of school completed), prior jail time (in months), and prior prison time (also in months). Through this process, we wanted to get a better idea of the demographics of the US Prison system (as of 2004. Since they only do this survey once every 7 years, this is the most recent).</p>

<a style="margin-left: 43%; padding: 4px; " class="source" href="https://explore.data.gov/Law-Enforcement-Courts-and-Prisons/National-Corrections-Reporting-Program-2004-United/v26k-cyk8">Get the dataset</a>
<br/>

<!-- TABBED CONTENT BELOW -->


  <ul class="nav nav-tabs">
    <li class="active"><a href="#tabs-1" data-toggle="tab">Exploration</a></li>
    <li><a href="#tabs-2" data-toggle="tab">Two States</a></li>
    <li><a href="#tabs-3" data-toggle="tab">Scale</a></li>
    <li><a href="#tabs-4" data-toggle="tab">Final Pass</a></li>
  </ul>
  
<div class="tab-content">  
<!--EXPLORATORY TAB-->
	<div id="tabs-1" class="tab-pane active">

<a class="internal-link" href="#r1">Jump to this section's R code</a><br><br>
    <strong>Race and Incarceration</strong>

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r1-1.png"/>
</div>
    <p>Our first plot shows the number of incarcerated by race and gender. We see
here the predominance of White and Black males in the US prison system.
What is alarming, however is that Black inmates are almost equal in
population to White inmates, despite the fact that the black population is
only ~12% of the total US population as of 2004. [<a class="source" href=" 
  http://www.census.gov/population/www/projections/downloadablefiles.html">source</a>]
</p>
<p> Another point of interest is the relatively low representation of females in the
prison system for non-white or non-black populations.
</p>

<strong>Education and Incarceration</strong>

<div class="crop">
  <img class="project-image" src="/assets/images/projects/prison/r1-2.png"/>
</div>

<p>The next dimension we wanted to explore was the education level of
inmates.</p>
<p>Here we see that a large concentration of the US Prison population stopping
their education past the 6th grade. It would be interesting to explore in
further detail why it is that this is such a turning point for most inmates and
what correlation can be drawn here with other factors that lead to criminal
lifestyles. We also see that a lot of inmates’ education ends in the 1st grade
or the 3rd grade, but not in the 2nd grade. Again, an interesting place to
focus with further exploration.</p>

<strong>Age and Race</strong>
<div class="crop"> 
  <img class="project-image" src="/assets/images/projects/prison/r1-3.png"/>
</div>
<p>Next we wanted to explore the distribution of age for each racial category.
Below is a “box and whisker” plot that shows us that the highest
concentration of inmates for any race are individuals born between 1960 and
1980, though Black inmates represent some of the oldest inmates with
Whites following closely behind.</p>

<strong>Age and Gender</strong>
<div class="crop">
  <img class="project-image" src="/assets/images/projects/prison/r1-4.png"/>
</div>

<p>We also wanted to observe the distribution of inmate age as it relates to
gender. Here we see that the male population seems to represent a more
even distribution of ages than women.</p>

<strong>Visualization Failures</strong>
<p>The above visualization is clearly not as compelling as the one preceding it. We
wanted to create a “box and whisker” plot involving gender, trying first to
incorporate all dimensions at once, but ran into a lot of complications and
strange results:</p>

<div class="crop">
  <img class="project-image" src="/assets/images/projects/prison/r1-5.png"/>
</div>

<p>Despite numerous attempts to manipulate the data in real time with R, we
got far more errors and complications than visual products.
As we also had data involving the prior years an inmate spent in prison, we
wanted to map that against age of the prison population.</p>
  
  
<p>This however, is fairly cluttered and any attempts to limit the data to
meaningful categories were met with endless complaints from our R
installation.</p>

<div class="crop">
  <img class="project-image" src="/assets/images/projects/prison/r1-6.png"/>
</div>

<p>Here we see a higher concentration of inmates with less prior months of
imprisonment, though we see a point where the concentration increases
after a certain number of months. While the original data set the values of
9999.5, 9999.8, and 9999.9 as NA data, the set was cleared of this
information and there still remained a large number of records that appear
to have values higher than that (which can be no less than ~833 years). We
must assume that there is something wrong with the data we have collected
here or some other factor that’s leading us to improbable results.</p>

<hr>
<h4>Intro Exploration R code:</h4>
<a name="r1"></a>
{% highlight r %}

library(ggplot2)

#connect to the data
data &gt;- read.table("http://www-personal.umich.edu/~asgor/prison.txt", header=TRUE, sep="\t")
attach(data)

#open our car library because we'll need to recode some columns
library(car)

#columns like race and gender are encoded as numbers and we want to also have a nominal encoding for them

nomgender = recode(gender, "c(1) = 'Male'; c(2) = 'Female'; else = NA")
nomrace = recode(race, "c(1) = 'White'; c(2) = 'Black'; c(3) = 'American Indian'; c(4) = 'Asian'; c(5) = 'Pacific Islander'; c(6) = 'Other'; else = NA")

#and we want to recode the education column to replace their numerical encoding or NA to 'NA' so as not to through the charting off.

newedu = recode(education, 'c(98,99) = NA')


# 1 Highest grade completed and gender
qplot(newedu, data = data, fill = nomgender, xlab = "Highest Grade of Completion in School")

# 2 Race and Gender
qplot(nomrace, data = data, fill = nomgender, xlab = "Race of Inmate")

#3 boxplot of race and birthyear
qplot(nomrace, birthyear, data = data,  ylim = c(1900,2000), geom = "boxplot")

#4   
qplot(nomgender, birthyear, data = data)

#some problematic input that got poor results
# Would really like to lose the NA
qplot(nomrace, birthyear, data = data,  ylim = c(1900,2000), fill = nomgender, geom = "boxplot")
qplot(nomrace, birthyear, data = data,  ylim = c(1900,2000), fill = na.omit(nomgender), geom = "boxplot")
qplot(nomrace, birthyear, data = data,  ylim = c(1900,2000), fill = nomgender, na.rm=TRUE, geom = "boxplot")

# previous jail time/prison time - R really complained about all of this, usually no product
priors_prison &gt;- recode(prior_prison_months, 'c(9999.9, 9999.8, 9999.5)=NA')
priors_jail &gt;- recode(prior_jail_months, 'c(9999.9, 9999.8, 9999.5)=NA')
priors_p &gt;- na.omit(priors_prison)
priors_j &gt;- na.omit(priors_jail)
priors_prison_less &gt;- ddply(data, .(nomgender), subset, priors_prison < mean(priors_prison))
qplot(priors_prison, nomgender, data = data, xlim=c(0,1200))
qplot(birthyear, priors_prison, data = data, xlim=c(1900,2000))

{% endhighlight %}

  </div>

<!--END OF EXPLORATORY TAB-->

<!--TWO STATES TAB-->
  <div id="tabs-2" class="tab-pane">
<a class="internal-link" href="#r2">Jump to this section's R code</a><br><br>
<strong>Tale of Two Prison Systems: Georgia and Michigan</strong>

<p>For this exploration, we wanted to take a look at what two different prison systems looked like compared to each other; Michigan State Prisons and Georgia State Prisons. We made this selection largely on an anecdotal suspicion that there might be a difference between prisons in the north and prisons in the south (Are Southern prisons “harsher”?).</p> 

<p>We were lead to Michigan and Georgia because each state as a similar state population as of the time the data was collected (2004 – Mich: 10,112,620, Geog: 8,829,383) and both states mostly had data in all of the columns we were interested in (gender, race, Hispanic origin, education level, total months spent in prison – it was hard to find any two states that had at least 3 of 4). Finally, both states yielded roughly the same number of records each (Mich = 281,502 inmate records; Geog = 211104 inmate records).</p>

<p>The first set of comparisons we wanted to see were if there were longer sentences in either prison system:</p>
<h4> Georgia </h4>
<div class="crop">
  <img class="project-image" src="/assets/images/projects/prison/r2-1.png"/>
</div>
<h4> Michigan </h4>
<div class="crop">
  <img class="project-image" src="/assets/images/projects/prison/r2-2.png"/>
</div>

<p>However we ran into our first mismatch of data; while there appears to be longer sentences in general in Michigan, this is probably due to the fact that this data was not so well documented in the Georgia case.</p>

<p>Next we figured we should gather some of the compared demographics before kludging dimensions together.</p>

Next we look at <strong>gender distributions</strong> alone:

<h4> Georgia </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-3.png"/>
</div>
<h4> Michigan </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-4.png"/>
</div>

<p>If you squint your eyes, there appears to be a larger percentage of women that make up the total population of the Georgia prisons over the Michigan prisons. Though we’d want to compare this further against the total population of each state.</p>

<p>Next we look at how <strong>gender and total months</strong> of each prison sentence looked for each state.</p>


<h4> Georgia </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-5.png"/>
</div>
<h4> Michigan </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-6.png"/>
</div>


<p>Again, overlooking the differences in data collected for total months, we see that in both cases, women tend to have shorter sentences than men in either state, at about the same proportion to the total of each state.</p>

<p>Next we explored the dimensions of race and education as they mapped to the gender and total sentence dimensions we’ve already seen.</p>

<strong>Education by Gender</strong>

<h4> Georgia </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-7.png"/>
</div>
<h4> Michigan </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-8.png"/>
</div>

<p>Here we cannot help but notice that on the whole, and perhaps by a non-trivial amount, there seems to be far more individuals in the prison system who almost graduated from high school in Michigan. As we can see since the higher NA data in Georgia might not account for the difference, as was the case with total sentences, we might have a phenomenon of interest here. Though we would again want to take a look at numbers for the general population of each state, it is possible that Michigan has a higher education rate than Georgia on the whole. Or there are some discrepencies in the collection of the data in the first place.</p>

<strong>Race and Gender Distributions</strong>

<h4> Georgia </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-9.png"/>
</div>
<h4> Michigan </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-10.png"/>
</div>

<p>In both states we see a predominance of white and black populations, with rather similar proportions for women and men in each race category. Though what stands out is the higher ration of blacks to whites in Georgia prisons as opposed to Michigan prisons. Perhaps a result of the general demographics of each state, but a difference worth noting and could perhaps buck the general trend of race distributions in the general population. As we recall the national average was to have slightly more White prisoners than all other racial categories, and here that average is not reflected in either state.</p>

<strong>Education, Total Months and Gender</strong>

<h4> Georgia </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-11.png"/>
</div>
<h4> Michigan </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-12.png"/>
</div>

<p>Looking back at the factors that interested us in the first education plot, we notice that there are slightly longer sentences for Michigan individuals with the 11th grade education, though this could be a result of the higher number of records. Interesting to note here is the way in which the average of males with a college degree seem to get longer sentences and that women with only some High School in Michigan appear to have slightly longer sentences than men, which goes against the overwhelming trend of men having generally longer sentences.</p>

<strong>Race, Total Months and Gender</strong> 

<h4> Georgia </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-13.png"/>
</div>
<h4> Michigan </h4>
<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r2-14.png"/>
</div> 

<p>While skewed again based on the imbalanced number of records. The only thing that really stands out here is that the small population of Asian Females imprisoned in Michigan had longer sentences than those in Georgia.</p> 

<strong>Conclusion</strong>

<p>While we might not have been able to pin down any set of narratives that cleanly separate the Michigan and Georgia prison systems. We did observe some interesting items we’d want to follow up on. Namely, are there more women and blacks, per capita, imprisoned in Georgia? As well as whether we can hunt down some data that let us know if there are longer sentences, shorter sentences or on the whole same sentence lengths in Georgia as compared to Michigan. Finally, we noticed that there was an interesting phenomenon in there being perhaps a more well-educated prison population in Michigan than Georgia, on the average. But further investgation of the data integrity is still needed.</p>


<hr>
<h4>Two States R code:</h4>
<a name="r2"></a>
{%highlight r%}
#Read Prison data in:
> data &gt;- read.table("/Users/<yournamehere>/desktop/618/prison.txt", header=TRUE, sep="\t")
> library(ggplot2)
#using the gdata library for modifying numeric unknowns to NA values for easier processing
> library(gdata)


##HW3 Key to Data:

V8 - Birthyear
V9 - Gender
V10 - Race
V11 - Hispanic Origin
V12 - Education
V15 - Year of Prison Admission
V60 - Total Prison Time
V57 - Age at Admission
V58 - Age at Release 
V94 - State ID 
V41 - AWOL or Escape
V36 - Location of Sentences Served

Georgia Pop. (2004) = 8,829,383
Michigan Pop. (2004) = 10,112,620
ggiaas Pop. (2004) = 22,490,022
(source Infoplease.com)

#get the data into two buckets, Michigan Data and 

##"Georgia (211104 records) v. Michigan (281502 records)"

#subsetting the data by the two states
> michdata &gt;- data[data$V94 == 26,] 
> ggiadata &gt;- data[data$V94 == 13,]

#processing the unknown numeric values to NA values
> totalmom&gt;-unknownToNA(x=michdata$V60, unknown=c(9999.5, 9999.8))
> totalmog&gt;-unknownToNA(x=ggiadata$V60, unknown=c(9999.5, 9999.8))

> educationm&gt;-unknownToNA(x=michdata$V12, unknown=c(95,98,99))
> educationg&gt;-unknownToNA(x=ggiadata$V12, unknown=c(95,98,99))

> michrace&gt;-unknownToNA(x=michdata$V10, unknown=c(8, 9))
> ggiarace&gt;-unknownToNA(x=ggiadata$V10, unknown=c(8, 9))

> genderm&gt;-unknownToNA(x=michdata$V9, unknown=c(8, 9))
> genderg&gt;-unknownToNA(x=ggiadata$V9, unknown=c(8, 9))


#modifying the levels for the total months
> wtotalmom = cut(totalmom,pretty(totalmom,3))
> wtotalmog = cut(totalmog,pretty(totalmog,3))

> wtotalmog &gt;- factor(wtotalmog)
> wtotalmom &gt;- factor(wtotalmom)
> levels(wtotalmog) &gt;- c("0-50", "50-100", "100-150")
> levels(wtotalmom) &gt;- c("0-100", "100-200", "200-300", "300-400")

#taking a look at those in table form
> table(wtotalmom)
wtotalmom
  (0,100] (100,200] (200,300] (300,400] 
    28709      3506       481         9 
> table(wtotalmog)
wtotalmog
   (0,50]  (50,100] (100,150] 
    18023       150        12 

#and in plot form
> qplot(wtotalmog, data=ggiadata, na.rm = TRUE, xlab="Total Months in Prison per Inmate (Georgia)")
> qplot(wtotalmom, data=michdata, na.rm = TRUE, xlab="Total Months in Prison per Inmate (Michigan)")

#modifying the levels for the education vector
> weducationm &gt;- factor(educationm)
> weducationg &gt;- factor(educationg)
> levels(weducationm) &gt;- c("&gt;= 8th Grade", "Some High School", "9th Grade", "10th Grade", "11th Grade", "12th or GED", "Some College", "College Degree", "Special/Ungraded")
> levels(weducationg) &gt;- c("&gt;= 8th Grade", "Some High School", "9th Grade", "10th Grade", "11th Grade", "12th or GED", "Some College", "College Degree", "Special/Ungraded")

#and for race
> fmichrace &gt;- factor(michrace)
> fggiarace &gt;- factor(ggiarace)
> levels(fmichrace) &gt;- c("White", "Black", "Native", "Asian", "Pacific", "Other")
> levels(fggiarace) &gt;- c("White", "Black", "Native", "Asian", "Pacific", "Other")

#and gender
> fgenderm &gt;- factor(genderm)
> fgenderg &gt;- factor(genderg)
> fgenderg &gt;- factor(genderg)
> levels(fgenderm) &gt;- c("Male", "Female")
> levels(fgenderg) &gt;- c("Male", "Female")
> table(fgenderm)
fgenderm
  Male Female 
 34122   2824 
> table(fgenderg)
fgenderg
  Male Female 
 34304   4130


#checking the Hispanic data
> michhisp&gt;-unknownToNA(x=michdata$V11, unknown=c(5,8,9))
> ggiahisp&gt;-unknownToNA(x=ggiadata$V11, unknown=c(5,8,9))
> fmichhisp &gt;-factor(michhisp)
> fggiahisp &gt;-factor(ggiahisp)
> qplot(fmichhisp, data=michdata, na.rm = TRUE, xlab="Inmates by Hispanic Pop. (Michigan)")

#looks like our hispanic data for michigan is largely made up of NA

#creating pie charts for gender distribution

>  pie &gt;- ggplot(michdata, aes(x = factor(1), fill = factor(genderm))) + geom_bar(width = 1)
>  pie + coord_polar(theta = "y")
>  pie &gt;- ggplot(michdata, aes(x = factor(1), fill = fgenderm)) + geom_bar(width = 1)
> pie + coord_polar(theta = "y")
>  pie &gt;- ggplot(michdata, aes(x = factor(1), fill = fgenderm)) + geom_bar(width = 1)
> pie + coord_polar(theta = "y") + opts(title = "Michigan Inmate Gender Dist.")
>  pie &gt;- ggplot(ggiadata, aes(x = factor(1), fill = fgenderg)) + geom_bar(width = 1)
> pie + coord_polar(theta = "y") + opts(title = "Georgia Inmate Gender Dist.")
>
   
#creating new data frames out of our new data 

> ggiadata2 &gt;- data.frame(fgenderg, totalmog, fggiarace, fggiahisp, weducationg)
> michdata2 &gt;- data.frame(fgenderm, totalmom, fmichrace, fmichhisp, weducationm)

##getting two dimensional data: 

#gender&totalmonths
qplot(fgenderg, totalmog, data = ggiadata2, na.rm = TRUE, geom="boxplot", fill=fgenderg) + opts(title = "Gender and Total Months in Prison (Georgia)")
qplot(fgenderm, totalmom, data = michdata2, na.rm = TRUE, geom="boxplot", fill=fgenderm) + opts(title = "Gender and Total Months in Prison (Michigan)")

#education&totalmonths w/ gender
> qplot(weducationg, totalmog, data = ggiadata2, na.rm = TRUE, geom="boxplot", fill=fgenderg, ylab="Total Months", xlab="Highest Grade Completed in School") + opts(title = "Education, Gender and Total Months in Prison (Georgia)", axis.text.x=theme_text(angle=45, hjust=1))

> qplot(weducationm, totalmom, data = michdata2, na.rm = TRUE, geom="boxplot", fill=fgenderm, ylab="Total Months", xlab="Highest Grade Completed in School") + opts(title = "Education, Gender and Total Months in Prison (Michigan)", axis.text.x=theme_text(angle=45, hjust=1))

#Race, Gender and Totalmos
qplot(fmichrace, totalmom, data = michdata2, na.rm = TRUE, geom="boxplot", fill=fgenderm, ylab="Total Months", xlab="Race Declared in Survey") + opts(title = "Race, Gender and Total Months in Prison (Michigan)", axis.text.x=theme_text(angle=45, hjust=1))

> qplot(fggiarace, totalmog, data = ggiadata2, na.rm = TRUE, geom="boxplot", fill=fgenderg, ylab="Total Months", xlab="Race Declared in Survey") + opts(title = "Race, Gender and Total Months in Prison (Georgia)", axis.text.x=theme_text(angle=45, hjust=1))

#Education Gender and Total Months
>qplot(weducationg, data = ggiadata2, na.rm = TRUE, fill=fgenderg, xlab="Highest Grade Completed") + opts(title = "Education Categories by Gender (Georgia)", axis.text.x=theme_text(angle=45, hjust=1))

> qplot(weducationm, data = michdata2, na.rm = TRUE, fill=fgenderm, xlab="Highest Grade Completed") + opts(title = "Education Categories by Gender (Michigan)", axis.text.x=theme_text(angle=45, hjust=1))

{%endhighlight%}

  </div>
  <!-- END OF TWO STATES -->
  <div id="tabs-3" class="tab-pane">
    <a class="internal-link" href="#r3">Jump to this section's R code</a><br><br>
    <p>For this installment, we wanted to inspect the relationship between Total Years Michigan inmates spent prior to their current sentence and the total years of their current sentence against their level of education and race.</p>  


    <strong>Scale</strong>

    <p>One scale issue we had was that the total prior years data is in months, but the total current sentence is in years. We used simple arithmetic, dividing the months to years by dividing each entry by 12 (totalprior&gt;-(totalmom/12)). Next, we found that leaving the year values as they were lead to difficulty in separating out the most important data, the education and race trends: </p>

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r3-1.png"/>
</div>

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r3-2.png"/>
</div> 

<p>So we altered the scale by removing some of the outliers of on the total current year axis (michdata2=michdata2[ttyr&lt;30,]).</p> 

<strong>The Plots</strong>

<p>On each plot, <strong>totalprior</strong> is the total years prior to the current sentence and <strong>ttyr</strong> is total years spent during the current sentence. </p>

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r3-3.png"/>
</div> 

<p>As we see here, as the total prior years increase there is a general trend for less of a current sentence for each race, though this trend runs slightly differently for each race. For blacks and whites, there is a steeper decrease than for Asian and Native inmates.</p>

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r3-4.png"/>
</div> 

<p>With respect to education, we see a more interesting trend: for inmates with some college, there is an increasing trend for total current years as total prior years increase, this is curious and we might want to know more as to why. For all other educational levels, there is a decreasing trend as we saw in the race plot.</p>
<br />
<hr>
<h4>Scale R code:</h4>
<a name="r3"></a>

{%highlight r%}
#Read Prison data in:
> data &gt;- read.table("/Users/<thiscouldbeyou>/desktop/618/prison.txt", header=TRUE, sep="\t")

#Libraries we'll use to prepare and plot data
> library(gdata)
> library(ggplot2)

#get only the Michigan Data
> michdata &gt;- data[data$V94 == 26,] 

#Clean out the unknowns *set them to NA*
> ageadmitm&gt;-unknownToNA(x=michdata$V57, unknown=c(99.5, 99.8))
> agereleasem&gt;-unknownToNA(x=michdata$V58, unknown=c(88.8, 99.5, 99.8))
> educationm&gt;-unknownToNA(x=michdata$V12, unknown=c(95,98,99))
> michrace&gt;-unknownToNA(x=michdata$V10, unknown=c(8, 9))
> totalmom&gt;-unknownToNA(x=michdata$V60, unknown=c(9999.5, 9999.8))

#prepare a couple extra columns

#totalmom is the total # of months each prisoner served prior to current sentence, we want those in years to scale with our Total Current Sentence Data, so we divide each entry by 12.
> totalprior &gt;- (totalmom/12)

#total year is the age at release from prison - the age of admission to prison
ttyr&gt;-(agereleasem - ageadmitm)


#factoring and creating levels for our categorical vectors, education and race
> weducationm &gt;- factor(educationm)
> levels(weducationm) &gt;- c("&gt;= 8th Grade", "Some High School", "9th Grade", "10th Grade", "11th Grade", "12th or GED", "Some College", "College Degree", "Special/Ungraded")

> fmichrace &gt;- factor(michrace)
> levels(fmichrace) &gt;- c("White", "Black", "Native", "Asian", "Pacific", "Other")


#create a new data frame of our new data
> michdata2&gt;-data.frame(weducationm, fmichrace, ageadmitm, agereleasem, ttyr, totalprior)

#alter the scale by only taking records where total current years are less than 30, since beyond this point there are only a few outliers
michdata2=michdata2[ttyr<30,]

#our plots, how does Education plot w Total Current Years and Total prior years?
> plot&gt;-ggplot(michdata2, aes(totalprior, ttyr, group=weducationm, xlab = "Total Prior Years", ylab = "Total Current Years")) + geom_point(color="grey") + opts(title="Total Years Prior, Total Current Sentence and Education - Michigan Prisons")
> plot+geom_smooth(aes(color=weducationm), method="lm", size=1, se=F)

#same question with race
> plot&gt;-ggplot(michdata2, aes(totalprior, ttyr, group=fmichrace, xlab = "Total Prior Years", ylab = "Total Current Years")) + geom_point(color="grey") + opts(title="Total Years Prior, Total Current Sentence and Race - Michigan Prisons")
> plot+geom_smooth(aes(color=fmichrace), method="lm", size=1, se=F)


{%endhighlight%}
  </div> 
  <div id="tabs-4" class="tab-pane">

    <a class="internal-link" href="#r4">Jump to this section's R code</a><br><br>

<p><strong>Data Processing Problem:</strong> Previously we’ve only been able to look at one or two states at a time. This is because our dataset is massive (1.3 million records) and a lot of the records have holes in some of the demographics we wanted to look at. We wanted to clean up our dataset such that we did not have so many records with holes in them. Previously, the size of this dataset has been a barrier to making nation wide visualizations that had any polish to them.</p>

<p><strong>Data Processing Solution:</strong> Create a routine for cleaning out records that are incomplete and saving the new data frame to a new text file:</p>

{%highlight r%}
data &gt;- read.table("http://www-personal.umich.edu/~asgor/prison.txt", header=TRUE, sep="\t")

library(gdata)

birthyr&gt;-unknownToNA(x=data$V8, unknown=c(9995, 9998, 9999)) 
gender&gt;-unknownToNA(x=data$V9, unknown=c(5, 8, 9)) 
race&gt;-unknownToNA(x=data$V10, unknown=c(8, 9)) 
hispanic&gt;-unknownToNA(x=data$V11, unknown=c(5, 8, 9))
education&gt;-unknownToNA(x=data$V12, unknown=c(95,98,99))  
admissionyr&gt;-unknownToNA(x=data$V15, unknown=c(8888, 9995, 9998, 9999))  
totpriormo&gt;-unknownToNA(x=data$V60, unknown=c(9999.5, 9999.8))  
admitage&gt;-unknownToNA(x=data$V57, unknown=c(99.5, 99.8))  
relage&gt;-unknownToNA(x=data$V58, unknown=c(88.8, 99.5, 99.8)) 
stateID &gt;- data$V94

data2 &gt;- data.frame(birthyr, gender, race, hispanic, education, admissionyr, totpriormo, admitage, relage, stateID)
mydata &gt;- na.omit(data2)

#write new data to new file using MASS Library and write.matrix function
library(MASS)
write.matrix(mydata, file = "/Users/quomo/Desktop/618/prison_noNA.txt", sep="\t")
{%endhighlight%}

<p>Now we have a considerably smaller dataset that still represents each one of the states included in the data, about 160,000 records (keep in mind that a lot of states did not have any records in the original data, so there were only about 30 states included in the data set):

    



    <br /></p>

{%highlight r%}

> table(data$stateID)

   Alabama                 California                   Colorado 
   8591                      27745                       6831 
   Florida                    Georgia                     Hawaii 
   121                        299                          2318 
   Iowa                       Kentucky                  Louisiana 
   8192                       1301                       2060 
   Maryland                 Michigan                  Minnesota 
   694                        27407                      23506 
  Missouri                   Nebraska                  Nevada 
  41                           6366                        361 
  New Hampshire           New Jersey               New York 
  9911                          3366                        7436 
  North Carolina             North Dakota           Oklahoma 
  11030                         2963                       1431 
  Oregon                       Pennsylvania            Rhode Island 
  9665                          0                             0 
  South Carolina            South Dakota            Tennessee 
  0                               0                              0 
  Texas                        Utah                          Virginia 
  0                               0                              0 
  Washington                West Virginia            Wisconsin 
  0                               0                               0 
> 

{%endhighlight%}

<p><strong>Metadata Problem:</strong> It is not a new problem for our data that all of the demographic data has been encoded numerically (1 = male, 2 = female, 1 = white, 2 = black etc.).</p>

<p><strong>Metadata Solution:</strong> As with previous assignments, we went through the following steps to reassign nominal values to the demographic and state data:</p> 


{%highlight r%}

data$stateID&gt;-factor(data$stateID)
data$race&gt;-factor(data$race)
data$education&gt;-factor(data$education)
data$gender&gt;-factor(data$gender)

levels(data$stateID) &gt;- c("Alabama", "California", "Colorado", "Florida", "Georgia", "Hawaii", "Iowa", "Kentucky", "Louisiana", "Maryland","Michigan", "Minnesota", "Missouri", "Nebraska","Nevada", "New Hampshire", "New Jersey", "New York","North Carolina", "North Dakota", "Oklahoma", "Oregon","Pennsylvania", "Rhode Island", "South Carolina", "South Dakota","Tennessee", "Texas", "Utah", "Virginia", "Washington", "West Virginia", "Wisconsin", "California Youth Authority")

levels(data$race) &gt;- c("White", "Black", "Native", "Asian", "Pacific", "Other")

levels(data$education) &gt;- c("Up to 8th Grade", "Some High School", "9th Grade", "10th Grade", "11th Grade", "12th or GED", "Some College", "College Degree", "Special/Ungraded")

levels(data$gender) &gt;- c("Male", "Female")

> head(data)
birthyr gender   race hispanic    education admissionyr totpriormo admitage relage
1    1940   Male  Black        2 Some College        2004        2.4     63.8   64.3
2    1955 Female  Black        2   10th Grade        2000       28.7     44.8   48.6
4    1952   Male  White        2  12th or GED        2002       34.9     50.1   51.5
5    1951   Male Native        2  12th or GED        1999        0.9     48.3   53.4
6    1954   Male  Black        2  12th or GED        2003       60.7     49.4   49.9
7    1954   Male  Black        2  12th or GED        2004       66.5     50.1   50.3
  stateID
1 Alabama
2 Alabama
4 Alabama
5 Alabama
6 Alabama
7 Alabama
> 
{%endhighlight%}

<p><strong>Summary Problem:</strong> Realizing that we have data that is not equally represented from state to state and that a lot of the inmate populations in each state have very disproportionate demographics (i.e. there are a lot of white and black males in prison, which tend to drown out women and other racial groups in absolute numbers), we wanted to take the approach of looking at our data nationwide, while being able to see more of the relative distribution rather than the absolute distribution.</p> 

<p><strong>Solution:</strong> To do this, we decided to approach the data with applying a density analysis.</p> 

<p>Our first attempt looking at distribution of race for each year and inmate is admitted to prison was interesting, but largely uninformative. While we see an interesting shape for each racial category, it’s still hard to tell exactly what we are looking at.</p>

{%highlight r%}
p &gt;- ggplot(data, aes(admissionyr))
p + stat_density(aes(ymax = ..density.., ymin = 0), fill = "pink", colour = "grey40", geom = "ribbon", position = "identity")
{%endhighlight%}

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r4-1.png"/>
</div>

<p>Conceptually, we wanted to see something more like a steam graph, where each race (or gender, or education level) were laid next to each other, reflecting the entire population and then the relative distribution of each demographic category to the full population as mapped against some continuous value such as the year of admission, the age of each inmate at their admission or the age of each inmate at their release.</p> 

<p>We also saw that there was a considerable spike in each of the plots around the 1990s, so we decided to strip the years prior to 1992 off since there is low representation for those years in our data.</p>

{%highlight r%}
data  &gt;- data[data$admissionyr > 1991,]
{%endhighlight%}

<p>We then came up with the following using facet_wrap() to create small multiples of the data for each state:</p> 

{%highlight r%}
qplot(data$admissionyr, ..density.., data = data, geom = "density", fill = race, position = "fill", xlab = "Year of Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Race per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))
{%endhighlight%}

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r4-2.png"/>
</div>

<p>Though not perfect, we rather liked this plot the most of all the plots we’ve generated so far. Clearly we end up seeing some of the holes in the data here; Florida, Georgia and Missouri seem to have rather unrepresentative data.</p> 

<p>We decided to render similar charts for the different continuous and demographic data we had:</p>

{%highlight r%}
qplot(data$admitage, ..density.., data = data, geom = "density", fill = education, position = "fill", xlab = "Age of Inmate at Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Education per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))
{%endhighlight%}

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r4-3.png"/>
</div>

{%highlight r%}
qplot(data$relage, ..density.., data = data, geom = "density", fill = gender, position = "fill", xlab = "Age of Inmate at Release from Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Gender per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))
{%endhighlight%}

<div class="crop">  
  <img class="project-image" src="/assets/images/projects/prison/r4-4.png"/>
</div>

<p><strong>Analysis of Stat and Geom used:</strong> Again, because we are dealing with states and dimensions that are not represented equally across the data, looking at what we have in terms of absolute numbers did not allow for easily being able to isolate trends and categories. </p>

<p>The density approach, in R, is both geom and stat as it is a procedure of extracting and visualizing the relative distribution of a category within a defined population. In each of the above plots, the defined populations are the states.</p>

<p>We added the <pre><code> position = “fill”</code></pre> so that we might be able to set each distribution next to each other, similar to a steam graph, instead of layering each category one on top of the other. This is because the number of categories we are comparing (especially for education) are too much to, again, be able to isolate and analyze in a single category if they are all piled one on top of another.</p>

<p>Again, while this approach demonstrates a few gaping holes in the data we selected, we still get a view on some interesting trends. For example, in the education by age of admission above, we see that there is a tendency across states for there to be a larger distribution of inmates with an 8th grade or less education and that this population tends to make up a lot of the inmates that are admitted at age 60+. In the gender plot below it, we see that in general, male prisoners tend to be older on their release (excepting Maryland and Alabama; Maryland only has 694 records in this data, but there is a bit more for Alabama - ~8000).</p>

<hr>
<h4>Final Pass R code:</h4>
<a name="r4"></a>

{%highlight r%}
From Area Plot to Density Plot, seeing distribution without absolute numbers getting in the way: As we go through our data, we realize that not all states are represented fully across all the dimensions we have decided to visualize. some states have more records than others and we can also assume that in each state, there might be holes in the race, education or gender data that are not the same as other states. So far we have also only seen at most 2 states at a time. We now want to take a look at all the states at a glance.

data &gt;- read.table("http://www-personal.umich.edu/~asgor/prison.txt", header=TRUE, sep="\t")


birthyr&gt;-unknownToNA(x=data$V8, unknown=c(9995, 9998, 9999)) 
gender&gt;-unknownToNA(x=data$V9, unknown=c(5, 8, 9)) 
race&gt;-unknownToNA(x=data$V10, unknown=c(8, 9)) 
hispanic&gt;-unknownToNA(x=data$V11, unknown=c(5, 8, 9))
education&gt;-unknownToNA(x=data$V12, unknown=c(95,98,99))  
admissionyr&gt;-unknownToNA(x=data$V15, unknown=c(8888, 9995, 9998, 9999))  
totpriormo&gt;-unknownToNA(x=data$V60, unknown=c(9999.5, 9999.8))  
admitage&gt;-unknownToNA(x=data$V57, unknown=c(99.5, 99.8))  
relage&gt;-unknownToNA(x=data$V58, unknown=c(88.8, 99.5, 99.8)) 
stateID &gt;- data$V94

data2 &gt;- data.frame(birthyr, gender, race, hispanic, education, admissionyr, totpriormo, admitage, relage, stateID)
mydata &gt;- na.omit(data2)

#write new data to new file using MASS Library and write.matrix function

write.matrix(mydata, file = "/Users/quomo/Desktop/618/prison_noNA.txt", sep="\t")

#now we can feed in our data already processed.

data &gt;- read.table("/Users/quomo/desktop/618/prison_noNA.txt", header=TRUE, sep="\t")

#make sure that our categorical data is represented in Nominal form

data$stateID&gt;-factor(data$stateID)
data$race&gt;-factor(data$race)
data$education&gt;-factor(data$education)
data$gender&gt;-factor(data$gender)

levels(data$stateID) &gt;- c("Alabama", "California", "Colorado", "Florida", "Georgia", "Hawaii", "Iowa", "Kentucky", "Louisiana", "Maryland","Michigan", "Minnesota", "Missouri", "Nebraska","Nevada", "New Hampshire", "New Jersey", "New York","North Carolina", "North Dakota", "Oklahoma", "Oregon","Pennsylvania", "Rhode Island", "South Carolina", "South Dakota","Tennessee", "Texas", "Utah", "Virginia", "Washington", "West Virginia", "Wisconsin", "California Youth Authority")

levels(data$race) &gt;- c("White", "Black", "Native", "Asian", "Pacific", "Other")

levels(data$education) &gt;- c("Up to 8th Grade", "Some High School", "9th Grade", "10th Grade", "11th Grade", "12th or GED", "Some College", "College Degree", "Special/Ungraded")

levels(data$gender) &gt;- c("Male", "Female")

#first plot
p &gt;- ggplot(data, aes(admissionyr))
p + stat_density(aes(ymax = ..density.., ymin = 0), fill = "pink", colour = "grey40", geom = "ribbon", position = "identity") + facet_grid(. ~ race) + coord_flip()


#strip off the years prior to 1992 since we have very little data to support those years.

data&gt;-data[data$admissionyr > 1991,]


#plots we ended up with
qplot(data$admissionyr, ..density.., data = data, geom = "density", fill = race, position = "fill", xlab = "Year of Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Race per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$admitage, ..density.., data = data, geom = "density", fill = race, position = "fill", xlab = "Age of Inmate at Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Race per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$relage, ..density.., data = data, geom = "density", fill = race, position = "fill", xlab = "Age of Inmate at Release from Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Race per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$admissionyr, ..density.., data = data, geom = "density", fill = education, position = "fill", xlab = "Year of Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Education per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$admitage, ..density.., data = data, geom = "density", fill = education, position = "fill", xlab = "Age of Inmate at Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Education per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$relage, ..density.., data = data, geom = "density", fill = education, position = "fill", xlab = "Age of Inmate at Release from Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Education per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$admissionyr, ..density.., data = data, geom = "density", fill = gender, position = "fill", xlab = "Year of Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Gender per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$admitage, ..density.., data = data, geom = "density", fill = gender, position = "fill", xlab = "Age of Inmate at Admission to Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Gender per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))

qplot(data$relage, ..density.., data = data, geom = "density", fill = gender, position = "fill", xlab = "Age of Inmate at Release from Prison") + facet_wrap(~ stateID, ncol = 5) + opts(title="Distribution of Inmate Gender per State 1992-2004", axis.text.x = theme_text(angle=45, hjust=1))


#density plots, a start:
qplot(admissionyr, data = data2, geom="density", fill = factor(gender))
> qplot(admissionyr, data = data2, geom="density", fill = factor(gender), alpha=I(0.2))
> qplot(admissionyr, data = data2, geom="density", fill = factor(race), alpha=I(0.2))
> 5
[1] 5
> qplot(admissionyr, data = data2, geom="density", fill = factor(race), alpha=I(0.5))
> qplot(admissionyr, data = data2, geom="density", fill = factor(education), alpha=I(0.5))


###something interesting, density @ http://had.co.nz/ggplot2/stat_density.html

p &gt;- ggplot(data2, aes(admissionyr))
p + stat_density(aes(ymax = ..density.., ymin = -..density..), fill = "grey50", colour = "grey50", geom = "ribbon", position = "identity") + facet_grid(. ~ race) + coord_flip()

> p &gt;- ggplot(data2, aes(admitage))
> p + stat_density(aes(ymax = ..density.., ymin = -..density..), fill = "grey50", colour = "black", geom = "ribbon", position = "stack")

> qplot(data2$admitage, ..density.., data = data2, geom = "density", fill = race, position = "fill")
> qplot(data2$admitage, ..density.., data = data2, geom = "density", fill = factor(race), position = "fill")


> ggplot(data2, aes(x=admitage)) + stat_density(aes(ymax=..density.., ymin = -..density..), fill = "grey50", position = "stack") + facet_grid(. ~ stateID)
> ggplot(data2, aes(x=admitage)) + geom_density(aes(ymax=..density.., ymin = -..density..), fill = "grey50", position = "stack") + facet_grid(. ~ stateID)


> ggplot(data, aes(x=admitage)) + geom_density(aes(ymax=..density.., ymin = -..density..), fill = "grey50", position = "stack") + facet_grid(. ~ stateID)
> ggplot(data, aes(x=admitage)) + geom_density(aes(ymax=..density.., ymin = -..density..), fill = "grey50", position = "stack") + facet_wrap(~ stateID, ncol=7)

> qplot(data$admitage, ..density.., data = data, geom = "density", fill = factor(race), position = "fill") + facet_wrap(~ stateID, ncol = 5)

> qplot(data$admissionyr, ..density.., data = data, geom = "density", fill = factor(race), position = "fill") + facet_wrap(~ stateID, ncol = 5)
>

{%endhighlight%}


</div> <!-- I am what now? -->
</div><!-- end of tab content -->
