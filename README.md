# Data Science Academy Project: ChatGPT Tweet's Sentiment Analysis

## Project Description

ChatGPT is the AI-based conversational system offered by **OpenAI**. It was launched at the end of 2022 and has since received attention from the general public who finally realized how AI has evolved and is already among us.

Sentiment about ChatGPT has divided opinions. While some recognize that ChatGPT can be a useful tool for the daily lives of various activities, others express fear of being replaced by AI.

But which sentiment prevails on social media? Answering this question is your job in this project. Through the analysis of Tweets about ChatGPT, you must build an analysis process that allows you to identify the sentiment that predominates, especially on Twitter, about ChatGPT. This project could be extended to reflect sentiment on other topics, for example.

## Data dictionary

1. Datetime:  Clearly the time a tweet was created.
Description: Date and time of tweet creation.
Data Type: Datetime (or String representing datetime). Let's assume Datetime for analysis purposes, but note it could be string initially.
Possible Values: "2023-10-27 10:30:00 UTC", "2023-10-26T15:45:22Z" (ISO formats). Consider time zones - likely UTC for Twitter data.
Notes: Timezone considerations, format variations.

2. Tweet Id: Unique identifier for each tweet.
Description: Unique ID for the tweet on Twitter.
Data Type: String (or Integer, but often represented as string to avoid overflow/precision issues). String is safer.
Possible Values: "1717171717...", Long numeric strings.
Notes: Crucial for identifying unique tweets. Used in APIs.

3. Text:  The actual content of the tweet.
Description: Tweet's text content.
Data Type: String.
Possible Values: "Just tried ChatGPT, amazing!", "Is anyone else having issues with ChatGPT today?", Varied text content.
Notes: Might contain hashtags, mentions, URLs. Needs cleaning and processing for NLP tasks.

4. Username:  The user's handle.
Description: Twitter handle (screen name) of the user.
Data Type: String.
Possible Values: "@elonmusk", "@OpenAI", "@TechCrunch". Starts with "@" usually.
Notes: Used to identify users on Twitter.

5. Permalink: Direct URL to the tweet.
Description: Permanent link/URL to view the tweet directly on Twitter.
Data Type: String.
Possible Values: "https://twitter.com/username/status/1234567890". Standard Twitter URL format.
Notes: Useful for direct access and sharing.

6. User:  User object/dictionary - Hmm, this is interesting, we already have Username. What's User?  It's likely more detailed user info. Let's assume it's structured data because it's called "User" not just "UserId"
Description: Detailed information about the tweet's author. Likely a nested structure.
Data Type: Dictionary/JSON object (or perhaps a separate table if relational, but less likely in a flat dataset). Let's assume dictionary/JSON for now.
Possible Values: {'id': '123...', 'name': 'Elon Musk', 'followers_count': 100000000, ...} Think about common user profile info.
Notes: Likely contains more than just username (like follower count, profile description, etc.). Need to emphasize it's nested data. Clarify relationship with Username - Username is likely a key within the User object. Actually, scratch that, Username is probably just the string handle, and User is the whole user object, including the handle.

7. Outlinks: URLs mentioned in the tweet going outwards to other sites.
Description: URLs included in the tweet text that link to external websites.
Data Type: List/Array of Strings. (A tweet might have multiple outlinks).
Possible Values: ["https://openai.com/blog", "https://example.com/news"]. List of URLs.
Notes: Useful for link analysis, understanding tweet context beyond Twitter.

8. CountLinks: Number of outlinks - Simple count of the previous column.
Description: Number of URLs in the tweet text linking to external websites.
Data Type: Integer.
Possible Values: 0, 1, 2, 3... Non-negative integers.
Notes: Redundant if Outlinks is available, but convenient for quick analysis. Point out redundancy, but explain utility.

9. ReplyCount, RetweetCount, LikeCount, QuoteCount:  Engagement metrics.
Description: Number of replies, retweets, likes, and quote tweets received.
Data Type: Integer.
Possible Values: 0, 10, 100, 1000... Non-negative integers.
Notes: Key engagement metrics. Clearly explain each one's meaning.

10. ConversationId: ID for the entire conversation thread.
Description: ID of the conversation thread this tweet belongs to. Tweets in the same thread share the same ID.
Data Type: String (or Integer). String again, safer for IDs.
Possible Values: "12345...", "56789...". Unique ID per thread.
Notes: Connects replies, quotes, etc. into conversations. Explain its purpose for thread analysis.

11. Language:  Language of the tweet.
Description: Language of the tweet content, detected by Twitter.
Data Type: String (or language code). String is more descriptive.
Possible Values: "en" (English), "es" (Spanish), "fr" (French), "und" (Undetermined). ISO language codes, maybe full language names.
Notes: Language detection. Might be imperfect. "und" is important.

12. Source: Platform/app used to tweet.
Description: Source application or platform used to post the tweet (e.g., Twitter Web App, Twitter for iPhone).
Data Type: String.
Possible Values: "Twitter Web App", "Twitter for Android", "TweetDeck". Platform names.
Notes: Device/platform used. Useful for platform-specific analysis.

13. Media: Media attached to the tweet - Like User, "Media" is plural and suggests structured data.
Description: Information about media (images, videos, GIFs) attached to the tweet.
Data Type: List/Array of Dictionaries/JSON objects. (Multiple media items possible). List of dictionaries.
Possible Values: [{'type': 'photo', 'url': '...'}, {'type': 'video', 'url': '...', 'duration': '...'}]. Think about media types and attributes.
Notes: Presence and details of media. Nested data again, like User. Emphasize list of dictionaries.

14. QuotedTweet:  Details of a quoted tweet - Structured, single, since it's "QuotedTweet" singular.
Description: If the tweet is a quote tweet, this column contains information about the original tweet being quoted.
Data Type: Dictionary/JSON object (or Null if not a quote tweet).
Possible Values: A dictionary similar to the overall tweet structure, but for the quoted tweet, or null/None if not a quote. Recursive structure almost!
Notes: Nested tweet data for quote tweets. Handle null values.

15. MentionedUsers: Users mentioned in the tweet. Plural - list.
Description: List of Twitter users mentioned (@usernames) in the tweet.
Data Type: List/Array of Dictionaries/JSON objects (or maybe just list of usernames as strings - but dictionaries are richer). Let's assume dictionaries to be consistent with User and Media. Dictionaries are better.
Possible Values: [{'username': '@elonmusk', 'id': '...'}, {'username': '@BillGates', 'id': '...'}]. Details about mentioned users.
Notes: Users mentioned in the tweet text. List of user-like objects.

16. hashtag: Hashtags in the tweet. Singular in name, but likely multiple hashtags per tweet, so still a list.  Maybe should have been hashtags? Let's assume it's intended for all hashtags.
Description: Hashtags included in the tweet text (without the '#' symbol).
Data Type: List/Array of Strings.
Possible Values: ["ChatGPT", "AI", "Innovation"]. List of hashtag strings.
Notes: Hashtags extracted. Explain without "#" symbol.

17. hastag_counts:  Counts of each hashtag - Number of times each hashtag appears in the tweet?  Unlikely. More likely just the number of hashtags in the tweet.  Let's assume count of hashtags in the tweet, similar to CountLinks.  Typo in name "hastag" -> "hashtag".
Description: Number of hashtags included in the tweet text.
Data Type: Integer.
Possible Values: 0, 1, 2, 3... Non-negative integer.
Notes: Count of hashtags. Redundant if hashtag is available, but convenient. Redundancy note.










## MIT License

### Copyright (c) [2025] [Vagner Zeizer Carvalho Paes]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


