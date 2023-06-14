---
layout: post
title: "Updating Bible API"
date: 2023-06-14
categories: jekyll update
tags: featured
image: /assets/article_images/2014-08-29-welcome-to-jekyll/desktop.JPG
---

Over the past couple of days, I've been juggling various responsibilities such as school work, personal life, and my project, which has limited my progress. However, I did manage to make some important changes to the code. In our previous discussion, we addressed the issue of missing entries (verses) in the API. To find a solution, I scoured the internet and stumbled upon a SQL file that contains all the verses in the Bible, complete with unique identifiers. This meant that I needed to create a database to store these verses.

To simplify the process, I opted to use Prisma as an Object-Relational Mapping (ORM) library and Railway to facilitate the population of a PostgreSQL database. After successfully inserting all the data into the database, I was delighted to discover that there were approximately 31,140 entries. This means that we no longer have to worry about missing verses going forward.

![Progress](/assets/images/database1.JPG)

Now that the database is in place, we need to update our API accordingly. With the utilization of Prisma, we can easily query the PostgreSQL database using Prisma's relational query syntax. If you're interested, you can find the updated query and the syntax details [here](https://www.prisma.io/docs/concepts/components/prisma-client/relation-queries).

{% highlight javascript %}
`app.get("/getChapter", async (req, res) => {
    res.set("Access-Control-Allow-Origin", "\*");
    try {
        const { book, chapter } = req.query;
        console.log(book);
        console.log(chapter);
        const chapters = await prisma.bible.findMany({
            where: {
                short_label: book,
                chapter: Number(chapter),
                },
            orderBy: {
                id: "asc",
            },
            });
        }
        ...
    }
    ...
)`
{% endhighlight %}

There are still a lot to be done in order for this project to be considered done. Here are some of the things that are left:

- User authentication to store their progress
- Navigation in tabs
- Styling
