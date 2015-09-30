---
layout: post
title: "Sample GET/POST using Google AppEngine Java HttpConnection"
date: 2013-02-16 20:26
comments: true
categories: [Java, GAE]
---

A sample code on basic usage of HTTP GET/POST is always helpful. 

I wrote once for [AFNetworking](http://samwize.com/2012/10/25/simple-get-post-afnetworking/) (for iOS), which I subsequently referred to it myself. Hence, I am now writing for Google App Engine (Java).

Google provided [documentation](https://developers.google.com/appengine/docs/java/urlfetch/usingjavanet) on http connection, but it is incomplete. Here's my version.

<!-- more -->

## GET ##

In my example, I want to get a JSON response for a Comment resource. 

I showed how I set my custom headers, especially the Content-Type header. Then I read the response body by using `BufferedReader`, finally parse the string to `JSONObject`.

```java
    try {
        URL url = new URL("https://api.myserver.com/v1/comments");
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestProperty("X-Custom-Header", "xxx");
        connection.setRequestProperty("Content-Type", "application/json");
        
        if (connection.getResponseCode() == HttpURLConnection.HTTP_OK) {
            // OK
            BufferedReader reader = new BufferedReader(new InputStreamReader(url.openStream()));
            StringBuffer res = new StringBuffer(); 
            String line;
            while ((line = reader.readLine()) != null) {
                res.append(line);
            }
            reader.close();
            
            JSONObject jsonObj = new JSONObject(res);
            String count = jsonObj.getInt("count");
            ...

        } else {
            // Server returned HTTP error code.
        }
        
    } catch (Exception e) {
    }
```

## POST ##

Similarly, this is how you POST data.

```java
    try {
        URL url = new URL("https://api.myserver.com/v1/comments");
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestProperty("X-Custom-Header", "xxx");
        connection.setRequestProperty("Content-Type", "application/json");
        
        // POST the http body data
        connection.setDoOutput(true);
        connection.setRequestMethod("POST");
        OutputStreamWriter writer = new OutputStreamWriter(connection.getOutputStream());
        writer.write("{\"comment\": \"awesome tutorial\"}");
        writer.close();

        if (connection.getResponseCode() == HttpURLConnection.HTTP_OK) {
            // OK
            BufferedReader reader = new BufferedReader(new InputStreamReader(url.openStream()));
            StringBuffer res = new StringBuffer(); 
            String line;
            while ((line = reader.readLine()) != null) {
                res.append(line);
            }
            reader.close();
            
            JSONObject jsonObj = new JSONObject(res);
            String count = jsonObj.getInt("count");
            ...

        } else {
            // Server returned HTTP error code.
        }
        
    } catch (Exception e) {
    }
```

## Pitfalls ##

Google AppEngine always append to your [User-Agent header](http://stackoverflow.com/questions/13226598/how-to-remove-the-google-app-engine-default-header-when-fetching-a-url), so as to identify the http request is from a particular AppEngine app. This is a bane because if you are crawling some website, they could easily block you since you can't spoof your user-agent.




