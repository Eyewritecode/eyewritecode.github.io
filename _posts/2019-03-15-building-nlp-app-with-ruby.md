---
layout: post
title:  "Building a sentiment analysis classifier with Ruby"
date:   2019-03-15 19:00:00 +0700
categories: [ruby, nlp]
---

## Background story

A few weeks ago, a friend of mine who runs a coding bootcamp in Kyoto reached out to me about making a live coding to their 2019 class. 

The initial idea was to introduce GeoLocation apps because this is what i'm working on these days but i didn't think it would be *fun* enough to the students and mostly because it would involve lots of ***javascript*** while the bootcamp focuses a lot on `Ruby.`

My second thought was NLP. I have a deep interest in NLP and had never tried to build anything *NLP* with Ruby, so i saw this as an opportunity for me to find what's out there in Ruby world when it comes to Natural Language Processing.

After a few research, i found this [Ruby NLP](https://github.com/diasks2/ruby-nlp) repository and as its description says; 
> A collection of links to Ruby Natural Language Processing (NLP) libraries, tools and software

It aggregates different repos by NLP task.

## Sentiment Analysis

Since the goal of this presentation was to get the audience hooked on NLP, Sentiment analysis felt like a good fit for this as it's easier for anyone to relate to its applications (i.e Social media monitoring, Competitor analysis, etc...)

Ruby has gems that can help you do this with ease but i chose the long path that is building your own sentiment classifier.

### Data

Just like any classification task, i needed data to train my model. Luckily i found a sentiment analysis dataset online that had 100 examples (50 +, 50 -) enough to build our classifier for demo purpose but it's obvious that you'd need more, should you decide to run this in prod.

### Code

I used the [classifier](https://github.com/cardmagic/classifier) ruby gem. It allows you to choose a classification algorithm between Naive Bayes and LSI. Below is the classifier using Bayes algorithm.

Full working version can be found [here](https://github.com/eyewritecode/Ruby-Sentiment-Analysis)

{% highlight ruby %}
require "classifier"
require "csv"

class SentimentClassifier
  def initialize
    @sentiment_classifier = Classifier::Bayes.new("Negative", "Positive")
    @positive_data = []
    @negative_data = []
  end

  def preprocess
    data = CSV.read("data.csv", headers: true)
    data.each do |row|
      row["Label"] == "1" ? @positive_data.push(row["Text"]) : @negative_data.push(row["Text"])
    end
  end

  def train_classifier
    @negative_data.map do |n|
      @sentiment_classifier.train_negative(n)
    end
    @positive_data.map do |p|
      @sentiment_classifier.train_positive(p)
    end
  end
  
  def predict(text)
    preprocess
    train_classifier
    @sentiment_classifier.classify(text)
  end
end
{% endhighlight %}

Now if you were to do real NLP work, this wouldn't be the best approach. You would have better results leveraging `JRuby` or better yet, LEARN PYTHON :)
