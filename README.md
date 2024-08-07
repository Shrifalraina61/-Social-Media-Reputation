# -Social-Media-Reputation
import tweepy
import facebook
import requests
from textblob import TextBlob

# API credentials (replace with your own credentials)
twitter_consumer_key = 'your_twitter_consumer_key'
twitter_consumer_secret = 'your_twitter_consumer_secret'
twitter_access_token = 'your_twitter_access_token'
twitter_access_token_secret = 'your_twitter_access_token_secret'

facebook_access_token = 'your_facebook_access_token'
instagram_access_token = 'your_instagram_access_token'
instagram_account_id = 'your_instagram_account_id'  # Replace with your Instagram account ID

# Initialize Twitter API
twitter_auth = tweepy.OAuth1UserHandler(twitter_consumer_key, twitter_consumer_secret, twitter_access_token, twitter_access_token_secret)
twitter_api = tweepy.API(twitter_auth)

# Initialize Facebook API
facebook_graph = facebook.GraphAPI(access_token=facebook_access_token)

# Function to post to Twitter, Facebook, and Instagram
def post_to_social_media(content):
    try:
        # Post to Twitter
        twitter_api.update_status(content)
        print("Posted to Twitter")

        # Post to Facebook
        facebook_graph.put_object(parent_object='me', connection_name='feed', message=content)
        print("Posted to Facebook")

        # Post to Instagram (using Facebook Graph API for Instagram)
        instagram_url = f'https://graph.facebook.com/v12.0/{instagram_account_id}/media'
        params = {
            'caption': content,
            'access_token': instagram_access_token
        }
        response = requests.post(instagram_url, params=params)
        if response.ok:
            print("Posted to Instagram")
        else:
            print("Failed to post to Instagram:", response.text)
    except Exception as e:
        print(f"Error posting to social media: {e}")

# Function to analyze sentiment
def analyze_sentiment(post_content):
    analysis = TextBlob(post_content)
    sentiment_score = analysis.sentiment.polarity
    return sentiment_score

# Function to monitor and manage posts
def monitor_and_manage_posts(posts):
    for post in posts:
        sentiment_score = analyze_sentiment(post['content'])
        print(f"Sentiment Score for post {post['id']}: {sentiment_score}")
        if sentiment_score < -0.5:
            # Automated removal of negative posts
            remove_post(post['id'])

# Function to remove a post (dummy implementation)
def remove_post(post_id):
    print(f"Removing post with ID: {post_id}")
    # Logic to remove post from social media platforms
    # This is a placeholder; actual implementation will depend on API capabilities.

# Example usage
if __name__ == '__main__':
    # Content to post
    content = "Check out our latest updates!"
    post_to_social_media(content)

    # Simulated posts to monitor
    posts = [
        {'id': '12345', 'content': 'This is terrible!'},
        {'id': '67890', 'content': 'Great update!'}
    ]
    monitor_and_manage_posts(posts)

