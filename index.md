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


```
## Error: cannot change working directory
```

```
## Warning: cannot open compressed file 'markdown data.Rdata', probable
## reason 'No such file or directory'
```

```
## Error: cannot open the connection
```


## Data

Data were provided by the Coursera Data Science Specialization and Swiftkey. They included text from
Twitter, news websites and blogs. I used this data to train my model and make predictions. 

{img: "CourseTrackLogo.jpg"}
{img: "swiktKeyLogo.jpg"}


## Tokenization

First we tokenize the data. I tokenized different n-grams separately. These are bigrams.


```
## Warning: cannot open file 'en_US/en_US.twitter.txt': No such file or
## directory
```

```
## Error: cannot open the connection
```

```
## Error: object 'twitter' not found
```


```r
tweetTokens <- NGramTokenizer(twitter[tweets], control)
```

```
## Error: object 'twitter' not found
```

```r
tweetTokens[1:10]
```

```
## Error: object 'tweetTokens' not found
```

Then we add up multiple instances of the same phrase.


```r
tweetCounts <- count(tweetTokens)
```

```
## Error: object 'tweetTokens' not found
```

```r
tweetFilter <- grep(profanity, tweetCounts$x)
```

```
## Error: object 'tweetCounts' not found
```

```r
tweetCounts <- tweetCounts[-tweetFilter,]
```

```
## Error: object 'tweetCounts' not found
```

```r
head(tweetCounts)
```

```
## Error: object 'tweetCounts' not found
```


## Convert to Data Frame

The last thing was to convert the counts to a data frame for easier searching. I had help in this from
StackOverflow: http://stackoverflow.com/questions/7069076/split-column-at-delimiter-in-data-frame


```r
tweetDF <- data.frame(do.call('rbind', strsplit(as.character(tweetCounts$x),' ',fixed=TRUE)))
```

```
## Error: object 'tweetCounts' not found
```

```r
tweetDF <- cbind(tweetDF, tweetCounts$freq)
```

```
## Error: object 'tweetDF' not found
```

```r
head(tweetDF)
```

```
## Error: object 'tweetDF' not found
```


### The Function

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
## Error: could not find function "next.word"
```

Still some kinks to work out. =)


