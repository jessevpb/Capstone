---
title       : Word Prediction Algorithm
subtitle    : Data Science Specialization Capstone Project
author      : Jesse Peterson-Brandt
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## My Application

My word prediction app takes a phrase of any length and predicts the next word using a back-off model.
It begins by using up to four words and, if it does not find that exact phrase, works down to 
predicting based on just the last word.



## Data

Data were provided by the Coursera Data Science Specialization and Swiftkey. They included text from
Twitter, news websites and blogs. I used this data to train my model and make predictions. 

![Data Science Specialization](CourseTrackLogo.jpg)
<img src="swiftkeyLogo.png" width = "400px" height = "90px" />

---
## Tokenization
First we tokenize the data. I tokenized different n-grams separately. These are bigrams.




```r
tweetTokens <- NGramTokenizer(twitter, control)
tweetTokens[1:10]
```

```
##  [1] "How are"    "are you"    "you Btw"    "Btw thanks" "thanks for"
##  [6] "for the"    "the RT"     "RT You"     "You gonna"  "gonna be"
```



Then we add up multiple instances of the same phrase.


```r
tweetCount[300:303,]
```

```
##                 x freq
## 301 for advancing    1
## 302  for dropping    1
## 303   for Holiday    1
## 304      for long    1
```

---
## Convert to Data Frame

The last thing was to convert the counts to a data frame for easier searching. I had help in this from
StackOverflow: http://stackoverflow.com/questions/7069076/split-column-at-delimiter-in-data-frame


```r
tweetDF <- data.frame(do.call('rbind', strsplit(as.character(tweetCount$x),' ',fixed=TRUE)))
tweetDF <- cbind(tweetDF, tweetCount$freq)
tweetDF[50:55,]
```

```
##    X1       X2 tweetCount$freq
## 50  a    great               2
## 51  a  liberal               1
## 52  a      lot               1
## 53  A      LOT               1
## 54  a millenia               1
## 55  a    movie               1
```

---
## The Function

My function searches for up to a four-word phrase. I found my predictions to be understandable but very generic.
Often the function returns "the", "and" and "to" or similar high-usage words.
Here's a sampling of the code:

```r
        if(length(words) == 4 ) {
            wordMatch <- quinCountdf[Reduce(intersect, list(
                grep(paste("^", words[1], "$", sep = ""), quinCountdf$X1),
                grep(paste("^", words[2], "$", sep = ""), quinCountdf$X2),
                grep(paste("^", words[3], "$", sep = ""), quinCountdf$X3),
                grep(paste("^", words[4], "$", sep = ""), quinCountdf$X4))),]
```

and here's the code running:




```r
next.word("this course was the")
```

```
## [1] "first"
```

Still some kinks to work out. =)


