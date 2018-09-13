---
title: Reddit Sentiment Analysis
header:
  teaser: /assets/images/food-logos-teaser.png
---
*Sentiment analysis on all submissions to r/lawschooladmissions.*

Earlier this year I was sucked into the rabbit hole of the r/lawschooladmissions subreddit. I had a few close friends who were waiting to hear back from schools and as I casually started looking up their odds I discovered this very active community. It felt like everyone was in this together - they would diligently report updates from schools, compare them to when they were posted last year, and they even have a subreddit "dad" who answers many of their questions and offers guidance. But even in this sense of community law school admissions are stressful and there were stretches of very anxious posts as deadlines approached and pressure grew.

I wanted to dedicate this project to the r/lawschooladmissions community and see if I could use Python to gauge how people feel during the admissions process. To do that, I scraped all of the posts and comments from the subreddit and performed a sentiment analysis to see how many are positive versus negative and how that has changed over time.

*Check out my code for this project [here](https://github.com/jenzhenky/reddit_sentiment_analysis).*

### How do you quantify sentiment? 

I scraped all of the posts and comments from r/lawschooladmissions with [Pushshift's](https://pushshift.io/) API for Reddit. There were almost half a million submissions over the last five years - quite an active community! 

To perform sentiment analysis, I used [TextBlob](https://textblob.readthedocs.io/en/dev/), a Natural Language Processing (NLP) library in Python. It has a sentiment property that analyzes a sentence to return the sentence's polarity (how positive or negative it is) and subjectivity. TextBlob calculates this by looking up each word in the sentence in its [lexicon](https://github.com/sloria/TextBlob/blob/eb08c120d364e908646731d60b4e4c6c1712ff63/textblob/en/en-sentiment.xml), which has almost 3,000 entries with definitions and polarity and subjectivity scores. For more on how this works, check out this blog [post](https://planspace.org/20150607-textblob_sentiment/). In this project, I focused on sentiment polarity to categorize each submissions as positive, negative, or neutral. 

### What is the sentiment of this subreddit?

After I calculated sentiment with TextBlob, I plotted the histogram below to visualize the distribution of sentiment. Interestingly, the majority of submissions are neutral or slightly positive. As a sanity check, I took a look at the text of some of these submissions and found that they tend to be questions or informational. The positive skew of these submissions appeared to be due to users being polite or encouraging - what a pleasant surprise!

{% include figure image_path="/assets/images/reddit_sentiment_hist.png" alt="Histogram of sentiment on r/lawschooladmissions" caption="Histogram of sentiment on r/lawschooladmissions" %}

To categorize submissions as positive, negative, and neutral, I had to set sentiment polarity thresholds. I read samples of submissions and found that many neutral submissions scored slightly positive, so I set a higher threshold (0.5) for positives to eliminate these false hits. The negative threshold (-0.1) was set lower than its counterpart because there weren't as many false hits - users tended to not use "negative" words unless they meant them.

While the sentiment property was usually correct, it wasn't perfect. Below are samples of some of the most positive (1.0) and most negative (-1.0) submissions. Most are correctly categorized, but there were incorrect cases involving colloquialisms (e.g., "holy crap congrats!!") or a very positive or negative word (e.g., "r/lasat is your best bet…").

{% include figure image_path="/assets/images/reddit_top.png" alt="Sample of submissions" caption="A sample of some of the most positively and negatively scoring submissions" %}

### How has sentiment changed with time?

Law school admissions are cyclical - most applicants try to get all their applications in before the holidays and will hear back in March and April. To see how sentiment changes over the admissions cycle, I plotted the number of positive and negative submissions over time (below). As expected, there are distinct peaks and March of each year when people hear back from schools. Positive and negative activity tend to peak at the same time, but negative sentiment is notably higher. The peaks have also grown each year, correlating with the increased activity of this subreddit as it has grown more popular. 

{% include figure image_path="/assets/images/reddit_sentiment_count.png" alt="Sentiment on r/lawschooladmissions over time" caption="Positive and Negative Sentiment on r/lawschooladmissions over time" %}

I performed the same analysis on a volume-weighted basis to see if the percentage of positive and negative submissions have changed with time (below). The percentage of positive and negative submissions tend to peak in March, decline in the summer, and start ramping up again in the fall, which is in line with the admissions cycle. Positive and negative sentiment tend to peak at the same time with negative sentiment averaging 3 percentage points above positive sentiment throughout this cycle, implying that feelings run high at the same time but negative feelings tend to run higher.

{% include figure image_path="/assets/images/reddit_sentiment_percentage.png" alt="Volume-weighted Sentiment on r/lawschooladmissions over time" caption="Volume-weighted Positive and Negative Sentiment on r/lawschooladmissions over time" %}

### Next steps - Predicting admissions odds?

To anyone from the r/lawschooladmissions subreddit, I'm sure you're wondering if you can use this type of sentiment analysis to gauge how competitive an admissions cycle is and how your odds are looking. In this project's current state that isn't possible, but with a more refined and complex sentiment algorithm it would be possible to determine the degree of sentiment and perhaps break sentiment down into more informative categories (e.g., anxiety, hopefulness). This would only reflect how people "feel" about the cycle, but it could be a leading indicator of competitiveness and interest. 
