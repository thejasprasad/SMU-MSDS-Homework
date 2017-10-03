Requirement: Your client is expecting a baby soon. However, he is not sure what to name the child. Being out of the loop, he hires you to help him figure out recently popular names. He provides for you raw data in order to help you make a decision. 

File yob2016.txt is a series of popular children’s names born in the year 2016 in the United States. It consists of three columns with a first name, a gender, and the amount of children given that name

File yob2015.txt is similar to yob2016, it contains names, gender, and total children given that name for the year 2015.

R code will import the raw data in to R data frames, convert the raw data to a tidy data. Then merge the 2 data frames of years 2016 & 2015 popular names. Finally generates the top 10 girl names and writes it to a .csv file.

Below are the variable names and its descriptions used in this R code.

df : Dataframe that stores file yob2016.txt

y2016 : Dataframe that stores file dataframe without the redundant name.

y2015 : Dataframe that stores file yob2015.txt

final : Dataframe that has the merged data from y2016 & y2015 by FirstName and Gender. It also has a new column called “Total” that adds the amount of children in 2015 and 2016 together. 

final_sort : Has the sorted data of final data frame by column Total in the decending order.

girl : Dataframe which has only girl names.

girl_srt:  Has the sorted data of girl data frame by column Total in the decending order.

girl_10 : Dataframe which has only to 10 girl names and Total columns.

itsagirl.csv : .csv file generated from the data frame girl_10.