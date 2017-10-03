# TPrasad_HW5_Unit5
Thejas Prasad  
10/2/2017  



### Assignment Unit 4

Backstory: Your client is expecting a baby soon. However, he is not sure what to name the child. Being out of the loop, he hires you to help him figure out recently popular names. He provides for you raw data in order to help you make a decision. 

File yob2016.txt is a series of popular children’s names born in the year 2016 in the United States. It consists of three columns with a first name, a gender, and the amount of children given that name

File yob2015.txt is similar to yob2016, it contains names, gender, and total children given that name for the year 2015.

Following R code will import the raw data in to R data frames, convert the raw data to a tidy data. Then merge the 2 data frames of years 2016 & 2015 popular names. Finally generates the top 10 girl names and writes it to a .csv file.





```r
# Import the yob2016.txt raw data file into R and assigne it to a data frame 'df'

yob2016= read.table("yob2016.txt",sep = ";") 

df = data.frame("FirstName" = yob2016$V1, "Gender" = yob2016$V2, "AmountOfChildren" = yob2016$V3)

# Summary of the dataframe 'df'

summary(df)
```

```
##    FirstName     Gender    AmountOfChildren 
##  Aalijah:    2   F:18758   Min.   :    5.0  
##  Aaliyan:    2   M:14111   1st Qu.:    7.0  
##  Aamari :    2             Median :   12.0  
##  Aarian :    2             Mean   :  110.7  
##  Aarin  :    2             3rd Qu.:   30.0  
##  Aaris  :    2             Max.   :19414.0  
##  (Other):32857
```

```r
# Structure of the dataframe 'df'

str(df)
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ FirstName       : Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ Gender          : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ AmountOfChildren: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

```r
# Code to identify and display the name which has three y’s at the end of the name. 

library(sqldf)
```

```
## Loading required package: gsubfn
```

```
## Loading required package: proto
```

```
## Warning in doTryCatch(return(expr), name, parentenv, handler): unable to load shared object '/Library/Frameworks/R.framework/Resources/modules//R_X11.so':
##   dlopen(/Library/Frameworks/R.framework/Resources/modules//R_X11.so, 6): Library not loaded: /opt/X11/lib/libSM.6.dylib
##   Referenced from: /Library/Frameworks/R.framework/Resources/modules//R_X11.so
##   Reason: image not found
```

```
## Could not load tcltk.  Will use slower R code instead.
```

```
## Loading required package: RSQLite
```

```r
redundant_name = (sqldf("SELECT * FROM df WHERE FirstName like '%yyy'"))
redundant_name 
```

```
##   FirstName Gender AmountOfChildren
## 1  Fionayyy      F             1547
```

```r
# Remove this particular observation ( name which has three y’s at the end of the name), as it’s redundant. Save the remaining dataset as an object: y2016

y2016 = df[df$FirstName != "Fionayyy", ]

str(y2016)
```

```
## 'data.frame':	32868 obs. of  3 variables:
##  $ FirstName       : Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ Gender          : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ AmountOfChildren: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

```r
# Import the file yob2015.txt into R. give the dataframe human-readable column names. Assign this to dataframe to y2015.

yob2015= read.table("yob2015.txt",sep = ",") 

y2015 = data.frame("FirstName" = yob2015$V1, "Gender" = yob2015$V2, "AmountOfChildren" = yob2015$V3)


# Display the summary of df.
summary(y2015)
```

```
##    FirstName     Gender    AmountOfChildren 
##  Aalijah:    2   F:19054   Min.   :    5.0  
##  Aaliyah:    2   M:14009   1st Qu.:    7.0  
##  Aamari :    2             Median :   11.0  
##  Aarian :    2             Mean   :  111.4  
##  Aarion :    2             3rd Qu.:   30.0  
##  Aaron  :    2             Max.   :20415.0  
##  (Other):33051
```

```r
# Structure of df
str(y2015)
```

```
## 'data.frame':	33063 obs. of  3 variables:
##  $ FirstName       : Factor w/ 30553 levels "Aaban","Aabha",..: 9474 22858 26680 3771 12170 20927 344 9453 6252 11404 ...
##  $ Gender          : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ AmountOfChildren: int  20415 19638 17381 16340 15574 14871 12371 11766 11381 10283 ...
```

```r
# Interesting facts about the last 10 rows.

tail(y2015, 10)
```

```
##       FirstName Gender AmountOfChildren
## 33054      Ziyu      M                5
## 33055      Zoel      M                5
## 33056     Zohar      M                5
## 33057    Zolton      M                5
## 33058      Zyah      M                5
## 33059    Zykell      M                5
## 33060    Zyking      M                5
## 33061     Zykir      M                5
## 33062     Zyrus      M                5
## 33063      Zyus      M                5
```

```r
# All names start with 'Z'
# All are Male names
# No of childeren having these names are 5
```

Merge y2016 and y2015 by the Name column, assign it to final.The client only cares about names that have data for both 2016 and 2015, there should be no NA values in either of your amount of children rows after merging.


```r
final = merge(y2016, y2015, by = c("FirstName" , "Gender"), na.rm=TRUE)



# Create a new column called “Total” in final that adds the amount of children in 2015 and 2016 together. 

final = data.frame (final, "Total" = final$AmountOfChildren.x + final$AmountOfChildren.y)



# Total number of people who were given popular names in those two years combined
sum(final$Total)
```

```
## [1] 7239231
```

```r
# Sort the data by Total.

final_sort = final[order(final$Total, decreasing = TRUE), ]



# The top 10 most popular names
final_sort[1:10, c('FirstName')]
```

```
##  [1] Emma     Olivia   Noah     Liam     Sophia   Ava      Mason   
##  [8] William  Jacob    Isabella
## 30295 Levels: Aaban Aabha Aabid Aabir Aabriella Aadam Aadarsh ... Zyva
```

The client is expecting a girl! Omit boys and give the top 10 most popular girl’s names. Assign this to object girl.


```r
girl = final_sort[final_sort$Gender == "F", ]


girl_srt = girl[order(girl$Total, decreasing = TRUE), ]
head(girl_srt$FirstName, 10)
```

```
##  [1] Emma      Olivia    Sophia    Ava       Isabella  Mia       Charlotte
##  [8] Abigail   Emily     Harper   
## 30295 Levels: Aaban Aabha Aabid Aabir Aabriella Aadam Aadarsh ... Zyva
```

```r
girl_10 = data.frame(head(girl_srt[, c(1,5)], 10))

girl_10
```

```
##       FirstName Total
## 8290       Emma 39829
## 19886    Olivia 38884
## 23273    Sophia 33451
## 3252        Ava 32577
## 10682  Isabella 30296
## 18247       Mia 29237
## 5493  Charlotte 24411
## 277     Abigail 24070
## 8273      Emily 22692
## 9980     Harper 21016
```

Write these top 10 girl Names and their Totals to a CSV file called itsagirl.csv. Do not include row labels. Leave out columns other than Name and Total.


```r
write.csv(girl_10 , file = "itsagirl.csv", row.names = FALSE)
```



###End Of the Document



