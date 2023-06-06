---
layout: post
title: "Project: Bible Tracker"
date: 2023-06-06
categories: jekyll update
tags: featured
image: /assets/article_images/2014-08-29-welcome-to-jekyll/desktop.JPG
---

One day, while I was having my bible reading time, I thought it would be a good project to make a web application that tracks the user's progress on the bible. Korean bible specifically.

As I embarked on my mission to develop a web application centered around the Korean Bible, I quickly realized that finding a reliable KRV Bible API was no easy task. The scarcity of available resources posed a significant challenge. Having that said, I wanted to find a solution that would help me to achieve my goal.

After extensive research, I managed to locate a suitable KRV Bible API and connected it to the front end of my web application. However, I encountered CORS restrictions that prevented direct data retrieval from the API on the client side.

To address this issue, I created a proxy server to act as an intermediary. This server fetched data from the API, modified the CORS restrictions, and seamlessly passed it to the client side.
{% highlight javascript %}
{% raw %}
app.get("/quote.php", async (req, res) => {
res.set("Access-Control-Allow-Origin", "\*");
try {
const { kor, book, verseRange } = req.query;
const apiUrl = `http://ibibles.net/quote.php?${kor}-${book}/${verseRange}:1-100`;
console.log(apiUrl);
const response = await fetch(apiUrl);
const data = await response.text();
console.log(data);
res.send(data);
} catch (error) {
console.error("Error:", error);
res.status(500).send("Internal Server Error");
}
});
{% endraw %}
{% endhighlight %}
Implementing the proxy server required careful attention, but it enabled me to overcome the CORS restrictions and retrieve the KRV Bible data for presentation to users.
{% highlight javascript %}
{% raw %}
useEffect(() => {
async function fetchBook() {
console.log({ book })
let response
response = await fetch(
`http://localhost:8080/quote.php?kor=kor&book=${book}&verseRange=${chapter}`,
{ mode: 'cors' }
)
const res = await response.text()
return { \_\_html: res }
}
fetchBook().then((res) => setData(res))
console.log(allChapters)
// eslint-disable-next-line react-hooks/exhaustive-deps
}, [chapter, book])
{% endraw %}
{% endhighlight %}
Funnily the API that I found had some missing verses, so maybe I should look into creating my own API.

With the foundation in place, I was able to create a very simplistic app of a KRV bible.

As of right now it looks like this:
![Progress](/assets/images/progress1.JPG)

Things I will be looking forward to implement in the future are:

- User authentication
- Tracking user progress
- Making more reliable API
