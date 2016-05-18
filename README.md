```
 _ __  _   _ ___ _ __ ___   __ _ _ __
| '_ \| | | / __| '_ ` _ \ / _` | '_ \
| |_) | |_| \__ \ | | | | | (_| | |_) |
| .__/ \__, |___/_| |_| |_|\__,_| .__/
|_|    |___/                    |_|
```

[![PyPI](https://img.shields.io/pypi/v/pysmap.svg)](https://pypi.python.org/pypi/pysmap) [![PyPI](https://img.shields.io/pypi/dm/pysmap.svg)](https://pypi.python.org/pypi/pysmap) [![PyPI](https://img.shields.io/pypi/l/pysmap.svg)](https://github.com/SMAPPNYU/pysmap/blob/master/LICENSE)

:snake: pysmap is a high level toolkit for dealing with twitter data it also has a higher level interface for [smappdragon](https://github.com/SMAPPNYU/smappdragon). it has functionality from the old toolkit and functionality from our old util library smappPy.
- [twitterutil](#twitterutil)
    - [smapp_collection](#smapp_collection)
        - [get_tweets_containing](#get_tweets_containing)
        - [get_top_terms](#get_top_terms)
        - [get_tweet_texts](#get_tweet_texts)
        - [get_date_range](#get_date_range)
        - [get_geo_enabled](#get_geo_enabled)
        - [get_non_geo_enabled](#get_non_geo_enabled)
        - [get_top_entities](#get_top_entities)
        - [get_top_hashtags](#get_top_hashtags)
        - [get_top_urls](#get_top_urls)
        - [get_top_mentions](#get_top_mentions)
        - [get_top_media](#get_top_media)
        - [get_top_symbols](#get_top_symbols)
        - [count_tweet_terms](#count_tweet_terms)
        - [exclude_retweets](#exclude_retweets)
        - [tweets_with_user_location](#tweets_with_user_location)
        - [limit_number_of_tweets](#limit_number_of_tweets)
        - [tweet_language_is](#tweet_language_is)
        - [user_language_is](#user_language_is)
- [viz](#viz)
    - [plots](#plots)

#installation

`pip install pysmap`

`pip install pysmap --upgrade`

#twitterutil

the package with an array of twitter tools.

#smapp_collection

this is the smapp_collection class, an abstraction of smappdragon collections.

abstract:
```python
from pysmap import SmappCollection

collection = SmappCollection(DATA_TYPE, OTHER_INPUTS)
```

practical:
```python
from pysmap import SmappCollection

collection = SmappCollection('bson', '/path/to/my/bson/file.bson')
# or
collection = SmappCollection('mongo', 'superhost.bio.nyu.edu', 27574, smappReadWriteUserName, 'PASSWORD', 'GERMANY_ELECTION_2015_Nagler', 'tweets_1')
# or
collection = SmappCollection('json', '/path/to/my/file.json')
# or
collection = SmappCollection('csv', '/path/to/my/csv/file.csv')
```

*returns* a collection object that you can use to call methods below on

#iterate through tweets

iterate through the tweets in the collection you've made.

abstract:
```python
for tweet in collection:
    print(tweet)
```

practical:
```python
for tweet in collection.get_tweets_containing('cat').tweet_language_is('fr'):
    print(tweet)
```

#get_tweets_containing

gets tweets containing the specified term.

abstract:
```python
collection.get_tweets_containing(TERM)
```

practical:
```python
collection.get_tweets_containing('cats')
```

*returns* a collections which will filter out any tweets that do no have the specified term

#count_tweet_terms

counts the number of tweets that contain all these terms

abstract:
```python
collection.count_tweet_terms(TERM_1, TERM_2, ETC)
```

practical:
```python
count = collection.count_tweet_terms('cats', 'dogs')
print(count)
```

*returns* an integer value that counts all the tweets containing the terms

#get_top_terms

counts thet top words in a collection, [english stop words](https://github.com/Alir3z4/stop-words/blob/25c6a0aea665871e887f155b883e950c3743ce50/english.txt) are automatically included, otherwise you can specify your own set of stopwords with python stop-wrods. the stopwords are words taht get ignored and dwill not return in the final counts

abstract:
```python
collection.count_tweet_terms(MUMBER_OF_TERMS, LIST_OF_STOP_WORDS)
```

practical:
```python
count = collection.count_tweet_terms(5)
#or
count = collection.count_tweet_terms(5, ['blah', 'it', 'cat'])
print(count)
```

*note* `LIST_OF_STOP_WORDS` is optional, it is set to englis hby default

*returns* a dictionary that has all the top_X terms 

#get_tweet_texts

returns a new collection where the only key will be tweets.

abstract:
```python
for text in collection.get_tweet_texts():
    print(text)
```

practical:
```python
for text in collection.get_tweet_texts():
    print(text)
```

*returns* an iterator that returns just the text of each tweet

#get_date_range

gets tweets in a date range specified by python datmetime objects

abstract:
```python
collection.get_date_range(START, END)
```

practical:
```python
from datetime import datetime
collection.get_date_range(datetime(2014,1,30), datetime(2014,4,30))
```

*returns* a collection that will only return tweets from the specified datetime range

#tweet_language_is

only returns tweets where the language is the specified one

abstract:
```python
collection.tweet_language_is(LANGUAGE_CODE)
```

practical:
```python
#get tweets in english
collection.tweet_language_is('en')
```

*returns* a collection where all the tweets have their text language as the specified language

#user_language_is

only returns tweets where the user's specified language is the specified one

abstract:
```python
collection.user_language_is(LANGUAGE_CODE)
```

practical:
```python
collection.user_language_is('en')
```

*returns* a collection where all the tweets will come from users whose specified language matches the input

#exclude_retweets

exclueds retweets from your collection

abstract:
```python
collection.exclude_retweets()
```

practical:
```python
collection.exclude_retweets()
```

*returns* a collection where there are no retweets

#tweets_with_user_location

returns tweets that have a user location

abstract:
```python
collection.tweets_with_user_location(PLACE_TERM)
```

practical:
```python
collection.tweets_with_user_location('CA')
```

*returns* a collection where the places field of that tweet has the specified place

#get_geo_enabled

returns only geotagged tweets

abstract:
```python
collection.get_geo_enabled()
```

practical:
```python
collection.get_geo_enabled()
```

*returns* a collection that only produces geo tagged tweets

#get_non_geo_enabled

returns only non geotagged tweets

abstract:
```python
collection.get_non_geo_enabled()
```

practical:
```python
collection.get_non_geo_enabled()
```

*returns* a collection that only produces non geo tagged tweets

#limit_number_of_tweets

limits the # of tweets a collection can output

abstract:
```python
collection.limit_number_of_tweets(LIMIT_NUMEBER)
```

practical:
```python
collection.limit_number_of_tweets(145)
```

*returns* a collection that is limited on terms of the number of tweets it can output

#get_top_entities

returns the top twitter entites from a tweet object, you can [read about twitter entities here](https://dev.twitter.com/overview/api/entities-in-twitter-objects)

abstract:
```python
collection.top_entities({'ENTITY_FIELD':NUMBER_OF_TOP_TERMS, 'ENTITY_FIELD':NUMBER_OF_TOP_TERMS, 'ENTITY_FIELD':NUMBER_OF_TOP_TERMS})
```

practical:
```python
collection.top_entities({'user_mentions':5, 'media':3, 'hashtags':5, 'urls':0, 'user_mentions':2, 'symbols':2})
# or
collection.top_entities({'hashtags':5})
```

*returns* a dictionary containing tho requested entities and the counts for each entity

input:
```python
print collection.top_entities({'user_mentions':5, 'media':3, 'hashtags':5})
```

output:
```
{
        "hashtags": {
                "JadeHelm": 118, 
                "pjnet": 26, 
                "jadehelm": 111, 
                "falseflag": 32, 
                "2a": 26
        },
        "user_mentions": {
                "1619936671": 41, 
                "27234909": 56, 
                "733417892": 121, 
                "10228272": 75, 
                "233498836": 58
        }, 
        "media": {
                "https://t.co/ORaTXOM2oX": 55, 
                "https://t.co/pAfigDPcNc": 27, 
                "https://t.co/TH8TmGuYww": 24
        }
}
```

*returns* a dictionary filled with the top terms you requested

note: passing 0 to a field like `'hashtags':0` returns all the hashtags

note: no support for extended entities, retweet entities, user entites, or direct message entities.

note: if not enough entity objects are returned they get filled into the dictionary with null like so:

```
{
    "symbols": {
            "0": null, 
            "1": null, 
            "hould": 1
    }
}
```


#get_top_hashtags

get the top hashtags from a collection

abstract:
```python
collection.get_top_hashtags(NUMBER_TOP)
```

practical:
```python
hashtags = collection.get_top_hashtags(5)
print(hashtags)
```

*returns* the top hashtags as a dictionary

#get_top_urls

get the top urls from a collection

abstract:
```python
collection.get_top_urls(NUMBER_TOP)
```

practical:
```python
urls = collection.get_top_urls(6)
print(urls)
```

*returns* the top urls from a collection

#get_top_mentions

get the top mentions from a collection (these are @ mentions)

abstract:
```python
collection.get_top_mentions(NUMBER_TOP)
```

practical:
```python
mentions = collection.get_top_mentions(40)
```

*returns* the top @ mentions from a collection

#get_top_media

get the top media url references

abstract:
```python
collection.get_top_media(NUMBER_TOP)
```

practical:
```python
media = collection.get_top_media(3)
print(media)
```

*returns* the top media urls from a collection

#get_top_symbols

get the top symbols in a collection

abstract:
```python
collection.get_top_symbols(NUMBER_TOP)
```

practical:
```python
symbols = collection.get_top_symbols(10)
print(symbols)
```

*returns* the top symbols from a collection the number of top symbols depends on how man yspecified for input

#contributors

you might ask the difference between, pysmap and smappdragon. pysmap is easier to use but less flexible/more rigid in its implementation. smappdragon is a flexible tool fro programmers to use, you can build arbitray filters for data, pysmap is just a set of filters.

methods on smappdragon are lower level and more general. whereas methods on pysmap would be specific and rigid. so for example on smappdragon, you could [get all the entities](https://github.com/SMAPPNYU/smappdragon#top_entities), on pysmap you would have to ask for hashtags, mentions, etc. (which are all entities).

another example, something like [apply_labels](https://github.com/SMAPPNYU/smapp-toolkit#apply_labels) would go on smappdragon, not pysmap.

#viz 

a set of visualization tools, basically ways to graph and visualize a [SmappCollection](#smapp_collection)

#plots

a set of graph tools

#bar_graph_tweet_field_grouped_by_period

*NOT READY*

a tool that can be used to create generalized bar graphs from a smapp collection an various tweet data.

abstract:
```python
from pysmap import plots

plots.bar_graph_tweet_field_grouped_by_period(SMAPP_COLLECTION, TWEET_FIELD, TWEET_FIELD_VALUES_TO_MATCH, CUSTOM_FILTER_FUNCTION, SLICE_PERIOD, START_DATE, END_DATE, OUTPUT_FILE_PATH)
```

practical:
```python
from pysmap import SmappCollection, plots

collection = SmappCollection('json', 'docs/tweet_collection.json')
output_path = 'doc/output_graph.html'

def custom_filter(tweet):
    return True

plots.bar_graph_tweet_field_grouped_by_period(collection, 'user.lang', ['en', 'fr', 'es'], custom_filter, 'weeks', datetime(2015,9,1), datetime(2015,11,30), output_path)
```

*returns* an outputted html graph file and opens the graph in default

#author

[yvan](https://github.com/yvan)