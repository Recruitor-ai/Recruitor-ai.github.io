---
permalink: /feed2.xml
layout: null
---
    <?xml version="1.0" encoding="UTF-8"?>
    <rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
        <channel>
            <title>{{ site.title | xml_escape }}</title>
            <description>{{ site.description | xml_escape }}</description>
            <link>{{ site.url }}{{ site.baseurl }}/</link>
            <atom:link href="{{ "/feed.rbloggers.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
            <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
            <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
            <generator>Jekyll v{{ jekyll.version }}</generator>
            {% for page in pages %}
                <item>
                    <title>{{ page.title | xml_escape }}</title>
                    <author>your_blog_email@host.com (Your Name)</author>
                    <description>{{ page.content | xml_escape }}</description>
                    <pubDate>{{ page.date | date_to_rfc822 }}</pubDate>
                    <link>{{ page.url | prepend: site.baseurl | prepend: site.url }}</link>
                    <guid isPermaLink="true">{{ page.url | prepend: site.baseurl | prepend: site.url }}</guid>
                    {% for tag in page.tags %}
                    <category>{{ tag | xml_escape }}</category>
                    {% endfor %}
                    {% for cat in page.categories %}
                    <category>{{ cat | xml_escape }}</category>
                    {% endfor %}
                </item>
            {% endfor %}
        </channel>
    </rss>
    </code>