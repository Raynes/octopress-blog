---
layout: post
title: "Easy JSON parsing in Haskell with Aeson"
date: 2012-11-27 01:06
comments: true
categories: 
- Haskell
- Aeson
---

Yeah, I've been using Haskell. Wanna fight about it? Clojure isn't everything,
you know? Just most of everything. Something you may not know is that Haskell
was actually my first language, but until recently I had not used it for around
4 years! Haskell was the first language that ever made sense to me, but I hopped
to Clojure pretty quickly after that and there were a lot of awesome things I
missed. In particular, I never really *got* applicative functors, monads, etc. I
recently got back into Haskell for a delightful change of pace as well as to
learn these things I missed.

Haskell has more or less been a joy to work with. Two areas I found extremely
hard to get into are JSON parsing and HTTP clients. I'll be writing a blog post
about the latter soon, but this post is going to focus on JSON parsing.

In Clojure, we can just parse a JSON object into a Clojure map. Hash maps in
Clojure are heterogenous in nature and the keys and values can be any type. We
don't have this looseness in Haskell, and the way its type system works means we
have to do things differently.

Usually, you're going to want to define some data types that you will populate
with your parsed JSON. Usually, JSON maps to types very easily. First of all,
lets get some JSON to parse. Let's use some JSON returned by
[refheap](https://www.refheap.com)'s API:

```
$ http https://www.refheap.com/api/paste/1
HTTP/1.1 200 OK
Connection: keep-alive
Content-Length: 226
Content-Type: application/json;charset=utf-8
Date: Tue, 27 Nov 2012 08:39:26 GMT
Server: Jetty(7.6.1.v20120215)

{
    "contents": "(begin)", 
    "date": "2012-01-04T01:44:22.964Z", 
    "fork": null, 
    "language": "Clojure", 
    "lines": 1, 
    "paste-id": "1", 
    "private": false, 
    "random-id": "f1fc1181fb294950ca4df7008", 
    "url": "https://www.refheap.com/paste/1", 
    "user": "raynes"
}
```

This is a simple one-level JSON object.

Aeson uses Text instead of regular strings for everything, so to make things *a
lot* easier on us, we should use a language extension that overloads strings so
that we can just write literal strings and they will work for where Text is
expected:

```haskell
{-# LANGUAGE OverloadedStrings #-}
```

Let's go ahead and throw some imports in to start:

```haskell
import Data.Aeson ((.:), (.:?), decode, FromJSON(..), Value(..))
import Control.Applicative ((<$>), (<*>))
import qualified Data.ByteString.Lazy.Char8 as BS
```

Now here is the interesting part: we need a type to contain our parsed json
info. Take a look at our JSON. It is... a paste! A paste is a thing. It is a
thing that always has certain properties. It maps perfectly to a data type:

```haskell
data Paste = Paste { getLines    :: Integer
                   , getDate     :: String
                   , getID       :: String
                   , getLanguage :: String
                   , getPrivate  :: Bool
                   , getURL      :: String
                   , getUser     :: Maybe String
                   , getBody     :: String
                   } deriving (Show)
```

We defined the `Paste` type with record syntax to make it easy to get pieces of
it out and to remember what each field is. All of this should be self
explanatory. Note that the `user` is wrapped in Maybe because anonymous users
can create pastes and in those cases there wouldn't be a user at all, so it'd be
`Nothing`.

Now here comes the complicated part: we need to define an instance of the
`FromJSON` typeclass for our new `Paste` type. If we do this, we will be able to
decode our json directly to our `Paste` type. Aeson also has a ToJSON typeclass
for if we wanted to write code to convert our `Paste` type back to JSON, but
we're not interested in that. Let's write our instance:

```haskell
instance FromJSON Paste where
  parseJSON (Object v) =
    Paste <$>
    (v .: "lines")     <*>
    (v .: "date")      <*>
    (v .: "paste-id")  <*>
    (v .: "language")  <*>
    (v .: "private")   <*>
    (v .: "url")       <*>
    (v .:? "user")     <*>
    (v .: "contents")
```

Unfortunately, to really understand this example you need to have knowledge of
applicative functors. Explaining those and the above use of them is waaaaaay
beyond the scope of this post. I'll try to make some sense of it though.

Let's break things down. The first question you'll probably ask is "What in
tarnations does `.:` do!?!?!". In this case, it looks inside our object for a
JSON key called `"lines"` and plucks the value out. You'll notice that we also
have `.:?`. The difference between it and `.:` is that `.:?` allows for the key
to not exist or be null. It returns a `Maybe`. Remember that that is precisely
what we need for our possibly non-existing `user` key.

As far as the applicative functor stuff, you can more or less see what is
happening. Picture it like this: each argument to `Paste` is in a box. The
operator magic is taking each argument out of its box and applying `Paste` to
it. The end result is a new `Paste`. If any of the extractions with `.:` or
`.:?` fails, the result will be Nothing and we wont get a paste back. Let's try
this out. First define our JSON string. This needs to be a bytestring:



```haskell
*Blah> :load "blah.hs"
[1 of 1] Compiling Blah             ( blah.hs, interpreted )
Ok, modules loaded: Blah.
*Blah> let json = BS.pack
"{\"lines\":1,\"date\":\"2012-01-04T01:44:22.964Z\",\"paste-id\":\"1\",\"fork\":null,\"random-id\":\"f1fc1181fb294950ca4df7008\",\"language\":\"Clojure\",\"private\":false,\"url\":\"https://www.refheap.com/paste/1\",\"user\":\"raynes\",\"contents\":\"(begin)\"}"
```

Next, we'll decode into our Paste type:

```haskell
*Blah> decode json :: Maybe Paste
Just (Paste {getLines = 1, getDate = "2012-01-04T01:44:22.964Z", getID = "1",
getLanguage = "Clojure", getPrivate = False, getURL =
"https://www.refheap.com/paste/1", getUser = Just "raynes", getBody =
"(begin)"})
```

Awesome! As you can see, our result is wrapped in `Just`. If our JSON didn't
contain necessary data or was somehow not well formed, we'd get back `Nothing`
instead. Let's extract the date from the JSON.

```haskell
*Blah> let (Just x) = decode json :: Maybe Paste
*Blah> getDate x
"2012-01-04T01:44:22.964Z"
```

Fantastic. You've just parsed your first JSON in Haskell. Give yourself a pat on
the back.

Let's do something a little more complicated now. What if we don't want our date
to just be a string? What if we want some sort of date type? What if we want
UTCTime? Let's do that. First, we need to import a few more things. Add these
imports:


```haskell
import Data.Time.Format     (parseTime)
import Data.Time.Clock      (UTCTime)
import System.Locale        (defaultTimeLocale)
import Control.Monad        (liftM)
```

Now, let's write a simple little function for parsing dates in the format that
refheap uses. This example is taken from
[haskheap](https://github.com/Raynes/haskheap) and you do not need to understand
how it works if you don't already know or care, the important part is how we use
it later.

```haskell
parseRHTime :: String -> Maybe UTCTime
parseRHTime = parseTime defaultTimeLocale "%FT%X%QZ"
```

This function takes the string date from refheap's json and returns `Maybe
UTCTime`. It is wrapped in `Maybe` because it could fail if the date isn't
well-formed. Now, let's change our `Paste` type to expect `UTCTime` instead of a
`String`.

```haskell
data Paste = Paste { getLines    :: Integer
                   , getDate     :: Maybe UTCTime
                   , getID       :: String
                   , getLanguage :: String
                   , getPrivate  :: Bool
                   , getURL      :: String
                   , getUser     :: Maybe String
                   , getBody     :: String
                   } deriving (Show)
```

Excellent. Now all we need to do is change our `FromJSON` instance.

```haskell
instance FromJSON Paste where
  parseJSON (Object v) =
    Paste <$>
    (v .: "lines")                  <*>
    liftM parseRHTime (v .: "date") <*>
    (v .: "paste-id")               <*>
    (v .: "language")               <*>
    (v .: "private")                <*>
    (v .: "url")                    <*>
    (v .:? "user")                  <*>
    (v .: "contents")
```

Easy enough, right? The `Parser` type that `.:` returns is a monad,
so we just lift our `parseRHTime` function into it. Now let's parse our JSON
again.

```haskell
*Blah> :load "blah.hs"
[1 of 1] Compiling Blah             ( blah.hs, interpreted )
Ok, modules loaded: Blah.
*Blah> let json = BS.pack "{\"lines\":1,\"date\":\"2012-01-04T01:44:22.964Z\",\"paste-id\":\"1\",\"fork\":null,\"random-id\":\"f1fc1181fb294950ca4df7008\",\"language\":\"Clojure\",\"private\":false,\"url\":\"https://www.refheap.com/paste/1\",\"user\":\"raynes\",\"contents\":\"(begin)\"}"
*Blah> let (Just x) = decode json :: Maybe Paste
*Blah> getDate x
Just 2012-01-04 01:44:22.964 UTC
```

Yay! UTCTime! Wee!!!

So yeah, it's actually pretty simple. Usage of Aeson isn't exactly the most well
documented thing in the world, and if you don't at least understand applicative
functors, the examples can be really intimidating. If none of this makes sense,
go dig into [LYAH](http://learnyouahaskell.com) for a while. In particular, the
typeclasses through to the applicative functors chapters will help immensely.

Okay, so the above decode-straight-to-a-type sort of thing is the most common
way of working with JSON. But you don't always need or want to define a new type
just to grab a bit of information from some JSON. What if our code only cared
about the number of lines in a paste? We could write a `Lines` data type and
then an instance, but is it really worth it? We can just extract that piece
out. Let's rejigger our imports again:

```haskell
import qualified Data.HashMap.Strict as HM
import Data.Attoparsec.Number (Number(..))
```

Now, let's write a function to extract the number of lines from the json.

```haskell
getRHLines :: String -> Integer
getRHLines json =
  case HM.lookup "lines" hm of
    Just (Number (I n)) -> n
    Nothing             -> error "Y U NO HAS NUMBER?"
  where (Just (Object hm)) = decode (BS.pack json) :: Maybe Value
```

This isn't nearly as complicated as it looks. It's mostly just a lot of
destructuring. The meat of the example is that we are decoding the json to
`Maybe Value` instead of our own type. Our JSON is a big JSON object, and aeson
represents this as a hashmap. We can just look the lines key up in it! Note that
the `Number` type here is a wrapper around the `Number` type from attoparsec. It
is just a fancy type for representing different numbers with decent
precision. In this case, it represents an Integer (`I`), so we're destructuring
on that constructor.

Well, that's about all there is to it. There is also a funky trick for
automatically defining instances, but I'm not a big fan of that and it is
outside the scope of this post.

Here is the completed code:

```haskell
{-# LANGUAGE OverloadedStrings #-}
module Blah where

import Data.Aeson             ((.:), (.:?), decode, FromJSON(..), Value(..))
import Control.Applicative    ((<$>), (<*>))
import Data.Time.Format       (parseTime)
import Data.Time.Clock        (UTCTime)
import System.Locale          (defaultTimeLocale)
import Control.Monad          (liftM)
import Data.Attoparsec.Number (Number(..))
import qualified Data.HashMap.Strict as HM
import qualified Data.ByteString.Lazy.Char8 as BS

parseRHTime :: String -> Maybe UTCTime
parseRHTime = parseTime defaultTimeLocale "%FT%X%QZ"

data Paste = Paste { getLines    :: Integer
                   , getDate     :: Maybe UTCTime
                   , getID       :: String
                   , getLanguage :: String
                   , getPrivate  :: Bool
                   , getURL      :: String
                   , getUser     :: Maybe String
                   , getBody     :: String
                   } deriving (Show)

instance FromJSON Paste where
  parseJSON (Object v) =
    Paste <$>
    (v .: "lines")                  <*>
    liftM parseRHTime (v .: "date") <*>
    (v .: "paste-id")               <*>
    (v .: "language")               <*>
    (v .: "private")                <*>
    (v .: "url")                    <*>
    (v .:? "user")                  <*>
    (v .: "contents")

getRHLines :: String -> Integer
getRHLines json =
  case HM.lookup "lines" hm of
    Just (Number (I n)) -> n
    Nothing             -> error "Y U NO HAS NUMBER?"
  where (Just (Object hm)) = decode (BS.pack json) :: Maybe Value
```

Not half bad for 44 lines, eh? It isn't nowhere near as intuitive as doing the
same things in Clojure is, but that's because Clojure doesn't have Haskell's
type system. JSON parsing in Haskell is necessarily complicated, but Aeson does
an excellent job of making it not-that-hard. What Aeson really needs is some
serious beginner-oriented documentation.

This is the first Haskell blog post I've ever written and is based on the first
Haskell code I've written in about 4 years. Please be gentle. If anything is
wrong or can be improved, please let me know via email or a comment! I'll be
writing a blog post about http-conduit soon because I also found it a huge pain
in the ass to work with. Stay tuned.
