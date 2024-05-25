## Sentiment Analysis using NLP
```python
import nltk
from nltk.sentiment import SentimentIntensityAnalyzer

def analyze_sentiment(text):
    # Initialize the SentimentIntensityAnalyzer
    sid = SentimentIntensityAnalyzer()

    # Get the sentiment scores
    sentiment_scores = sid.polarity_scores(text)

    # Determine sentiment polarity based on the compound score
    if sentiment_scores['compound'] >= 0.05:
        return 'Positive'
    elif sentiment_scores['compound'] <= -0.05:
        return 'Negative'
    else:
        return 'Neutral'

# Example usage
text_to_analyze = ["I love the new product! It's amazing.","I don't like chinese fast food", "I loves to play badminton", "I like chocolate cake","I don't have any friends","I am drinking water","I am walking on flowers","The sun is shining, and the weather is beautiful.","It's disheartening to see so much conflict in the world.","The meeting is scheduled for 2:00 PM.","The data analysis report is due next week.","I appreciate your kindness and support.","I'm disappointed with the outcome of the project."]
for text in text_to_analyze:
    sentiment = analyze_sentiment(text)
    print(f"The sentiment of the text is {sentiment}.")
```
### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/4b146be7-60ec-408c-b197-16f4d40bf944)
