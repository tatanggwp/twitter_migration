# twitter_migration

Download all of your tweets and pictures from Twitter using API v2.

I tried getting my tweets using the feature
[download an archive of your data](https://help.twitter.com/en/managing-your-account/how-to-download-your-twitter-archive)
provided by Twitter but it didn't work for me.
This isn't surprising, given the mass exodus of users
after Elon Musk took control of the company.

Since my needs are simple, I figured it would be straightforward to write a script using their latest API.
That is what is presented here.

## Features Implemented

All I was interested in was downloading the following to my local drive:

* all the content that I've written
* full 280 chars for each tweet
* all the picture files that I've uploaded to Twitter

I wasn't interested in the following, so the provided scripts won't download:

* liked tweets
* videos
* threaded replies
* list of followers/followed accounts

Two scripts are provided:

### download_tweets

Downloads tweets in JSON files: `tweets_001.json`, `tweets_002.json`, ...

Each file contains 100 tweets at the most.
Each JSON file also includes URL's to their pictures. They reference
`https://pbs.twimg.com/media`.
These files aren't downloaded in this step.

Because this script is calling Twitter's API it needs the Bearer Token.

### generate_html

This script uses the JSON files downloaded in the previous step.
Since it doesn't make any API calls, it doesn't require Twitter's Bearer Token.
The script downloads pictures under directories `tweets_001`, `tweets_002`, etc.

Twitter used the image sharing service yfrog until the summer of 2011. Because yfrog was shut down,
most images up to that year aren't available to download.

HTML is generated at this point in files `tweets_001.html`, `tweets_002.html`, ...

Shortened URL's, using `t.co`, `bit.ly`, `tinyurl.com` are expanded.

In the JSON file, when a tweet has photos, the API inserts the URL of the photo into the body of the tweet.
This is redundant and annoying so the script removes them.

Retweets in the JSON file are truncated; the script just displays them and doesn't reconstruct them.

## Installation

```bash
git clone https://github.com/bcarreno/twitter_migration
cd twitter_migration
bundle install
```

## Authentication Via API Key

Using your regular Twitter account, visit the [Twitter Developer Portal](https://developer.twitter.com/en/portal)
and generate a Twitter app with an authentication token.
The name of the app doesn't matter since you're not going to use it.
The only thing you need from this step is the Bearer Token.

## Usage

```bash
# setup authentication
export BEARER_TOKEN="ReplaceThisWithYourOwnToken"
# download tweets as JSON files: tweets_001.json, tweets_002.json, ...
./download_tweets mytwitterhandle

# download pictures under directories tweets_001, tweets_002, etc,
# and build HTML files: tweets_001.html, tweets_002.html, ...
./generate_html
# open tweets_001.html in your browser
```

## Dependencies

The only dependencies defined in the Gemfile are `faraday` and `htmlentities`.

I tested using Ruby 3.0.3 but everything should work with earlier versions as well.

## Sample

As an example, I posted the resulting HTML generated by the scripts on my website:
https://carreno.me/twitter_migration/tweets_001.html