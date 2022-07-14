# Analyzing the Tennis Shoe Market: Fit and Sentiment Analysis

## Project Motivation
As an avid tennis player with worn out outsoles in my shoes, I went online to Tennis Warehouse to find a new pair of shoes, and found myself a bit frustrated with the current shoe offerings available. I have a slightly wide foot with a medium high arch, and it seemed that many of the shoes from the top brands were either too narrow, low arched, or running long or short on length. It's as if none of these brands were catering to me at all. I also noticed a lot of negative reviews citing comfort issues, toe jamming (ouch), faulty laces, and more. With all this in mind, I decided to combine my passions for tennis, tennis gear, and data to better understand how each brand compares across shoe fit and customer sentiment.

## Project Goals
Identify which brands are the best for wider, medium arched feet like mine. Also, I'd like to aggregate the reviews to firstly determine which brands have the highest average rating, and then run some NLP to determine the sentiments of the reviews. I want to know which brand has the highest percentage of positive reviews, and which has the highest percentage of negative reviews. I will use a Tableau dashboard, linked below, as the end product to help visually produce these insights.

## Tableau Dashboard
**TL;DR?** - Check out the dashboard below!

**[Link to Tableau Dashboard](https://public.tableau.com/app/profile/r.prabhu/viz/TennisShoeFitandSentimentAnalysis/Dashboard1)**

## Data Acquisition

To acquire the data needed for the Tableau dashboard, I will be scraping from Tennis Warehouse, an online store selling tennis gear, apparel, and accessories. I will only scrape shoe data for Adidas, Asics, Babolat, New Balance, and Nike shoes. Below are the main attributes of shoe data that I will be scraping:
- **Shoe Name, Price, Rating, # of Reviews**
  
  ![Shoe Page](https://user-images.githubusercontent.com/100224330/178817689-fe823fdf-e797-483c-b001-6d18f03e9e05.png)
<br>

- **Fit Details**

  ![image](https://user-images.githubusercontent.com/100224330/178818293-a4d84b68-3048-46a7-acab-25462c0980f0.png)
  
- **Reviews**

  ![image](https://user-images.githubusercontent.com/100224330/178818446-e377ee52-6976-4618-bf44-75d9061faace.png)


After scraping the required data, there is some data cleaning I need to perform. Much of the data cleaning efforts go toward cleaning string values to remove unwanted characters, converting strings to numeric types, and grouping values for the fit detail columns (i.e. Slightly Low and Low arch = Low).

## Comparing Fits
How does each brand stack up across length, width, and arch measurements? To create the charts below, I loaded the data into Tableau and created calculated fields that weight each fit column's values. For example, medium width is the midpoint at 0.5, snug is 0.25, and wide is 0.75.

![image](https://user-images.githubusercontent.com/100224330/178840466-8eb5e36d-f645-46ea-8890-81022e741571.png)

- Based on the above chart, we can see that New Balance and Asics are the best when it comes to consistency in length, as they on average have true fitting shoes. Width and arch comparisons jump out at me more, as New Balance appears to be the most wide-foot friendly, with Nike, Babolat and Adidas having more narrower options that skews them to be the left of the medium width midpoint. As for the arches, it's surprising that all brands are to the left of medium, indicating a prevalence of lower arched shoes across all brands. **For my feet, it looks like I'm most likely to find the right pair with Asics or New Balance**. 

## Distribution of Ratings
First off, let's check how the ratings are distributed. Ratings are assigned on a 1-5 star basis.

![image](https://user-images.githubusercontent.com/100224330/178824277-113ae8ab-43eb-4d57-afaf-bb42da67bda1.png)

5 star ratings make up the vast majority of the reviews, while 1-3 star reviews are somewhat uniformly distributed.

## Rating and Review Comparison by Brand
Which brand has the most reviews? Which brand has the highest average rating?

![image](https://user-images.githubusercontent.com/100224330/178860372-7c6d4a37-cc9a-4c1c-baf7-cacb632034ec.png)

For added context, here are the number of shoes currently offered by each brand

![image](https://user-images.githubusercontent.com/100224330/178827182-a07ad549-3b26-4714-b9b9-54062a86b15a.png)

Based on the products and reviews currently listed on Tennis Warehouse, it stands out that Nike has the highest number of reviews and the most shoe offerings, yet the lowest average customer rating of 3.3 stars. Let's see if we get similar results when running a sentiment analysis on the customer reviews.

## Customer Review Sentiment Analysis
For this sentiment analysis, I leverage a RoBERTa transformer model, that is pretrained on analyzing the sentiments of tweets. Transformer models often do a better job than other NLP models at picking up on context. So for example, when a customer writes "comfortable" preceeded by the words "not very," this should do a better job at assigning an appropriate negativity score for the review. Given that the model is already pretrained, we are essentially leveraging transfer learning in applying the pretrained model's knowledge over to this dataset. See the link below for the RoBERTa documentation.

**[Link to RoBERTa documentation](https://huggingface.co/docs/transformers/model_doc/roberta)**

After tokenizing the reviews and fitting them into the model, we can return a dataframe that contains the negativity, neutral, and positive scores for each review. Below is the breakdown of how the reviews have been classified.

![image](https://user-images.githubusercontent.com/100224330/178847465-a89cef7f-659b-491d-a219-b61b2de839b0.png)

The reviews are mostly positive overall, which makes sense given that most of the reviews were between 4 and 5 stars. As expected, the positivity and negativity scores also increase/decrease respectively with the customer rating.

![image](https://user-images.githubusercontent.com/100224330/178851270-3dd05634-990a-4e6b-9e0c-40235e7c4984.png) ![image](https://user-images.githubusercontent.com/100224330/178851458-31d9edc6-0cd7-41e7-a279-39784c951460.png)



Now we can bring the brands into the picture and figure out which brand has the most negative reviews, and which brand has the most positive reviews.


![image](https://user-images.githubusercontent.com/100224330/178860773-1cffbb85-186a-4d19-9a25-fe80d79dae0a.png)

From a cumulative standpoint, Nike has the highest amount of positive reviews, however Asics has the highest percentage of positive reviews. For negativity, Nike also leads the way from percentage-wise and cumulatively. These sentiments match up pretty well with the average star ratings, as it was Asics that had the highest average rating and Nike with the lowest average rating. To dig into the specific reviews labeled as positive, neutral, and negative, see the Tableau dashboard link below

**[Link to Tableau Dashboard](https://public.tableau.com/app/profile/r.prabhu/viz/TennisShoeFitandSentimentAnalysis/Dashboard1)**

## Next Steps
On my personal shoe-buying journey, I will likely be ordering both Asics and New Balance shoes to try out, and hopefully one of them fits me just right. For tennis, I have experience wearing Asics, Nike, and Babolat, and Asics has been the most consistent in providing the right fit. Outside of tennis, I am a fan of Nike shoes, which is what makes this a bummer seeing that most, if not all, of their offerings present some fit issues for me.

As for how to further iterate this project, I'd like to revisit this or take up more advanced NLP projects as I get more comfortable with the concepts. I'd be interested to better understand which words, or n-grams are most prevalent in the reviews, and which ones are the true drivers behind the sentiment scores. Also, I could make this a predictive exercise in trying to predict which customers would buy the shoes again. This would be a change of perspective, going from the consumer side to the business side. Ideally, there are loyal customers that have very positive reviews, and we can send promotional emails to these customers when a new version of the shoe is released.
