---
permalink: /feed2.xml
layout: null
---
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
	<channel>
		<title>{{ site.name | xml_escape }} - Articles</title>
		<description>{% if site.description %}{{ site.description | xml_escape }}{% endif %}</description>
		<link>{{ site.url }}</link>
		<atom:link href="{{ site.url }}/feed.articles.xml" rel="self" type="application/rss+xml" />
		{% for page in site.pages %}
			{% unless page.link %}
			<item>
				<title>{{ page.title | xml_escape }}</title>
				<pubDate>{{ page.date | date: "%a, %d %b %Y %H:%M:%S %z" }}</pubDate>
				<link href="{{ site.url }}{{ page.url }}" />
				<guid isPermaLink="true">{{ site.url }}{{ page.url }}</guid>
			</item>
			{% endunless %}
		{% endfor %}
	</channel>
</rss>