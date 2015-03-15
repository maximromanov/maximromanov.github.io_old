---
layout: page
title: al-Thurayyā
description: "a Gazetteer of the Classical Islamic World"
permalink: /althurayya/
---

<section class="post-topmatter">

<section class="share"> 
<a class="icon-twitter" href="http://twitter.com/share?text=About Maxim Romanov&amp;url=http://maximromanov.github.io/about/"
onclick="window.open(this.href, 'twitter-share', 'width=550,height=235');return false;">
<span class="hidden">Twitter</span>
</a>
<a class="icon-facebook" href="https://www.facebook.com/sharer/sharer.php?u=http://maximromanov.github.io/about/"
onclick="window.open(this.href, 'facebook-share','width=580,height=296');return false;">
<span class="hidden">Facebook</span>
</a>
<a class="icon-google-plus" href="https://plus.google.com/share?url=http://maximromanov.github.io/about/"
onclick="window.open(this.href, 'google-plus-share', 'width=490,height=530');return false;">
<span class="hidden">Google+</span>
</a>
</section>
</section>

_al-Thurayyā_ is a Gazetter of the Classical Islamic World, and a part and parcel of another project in progress—**_Ṣūraŧ al-arḍ_**, a geospatial model of the Classical Islamic World (being developed in collaboration with Cameron Jackson, a Senior, double majoring in Arabic and Computer Science @ Tufts University).

<center>[Latest working version of *al-Thurayyā*]( {{ site.url }}/projects/althurayya_02/ )</center>

## Blog posts on major milestones of the project: 

{% for tag in site.tags order:ascending %}
{% if tag[0] == 'al-Thurayyā' %}
<ul class="post-list">
{% assign pages_list = tag[1] %}  
{% for post in pages_list %}
{% if post.title != null %}
{% if group == null or group == post.group %}
<li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}<span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></a></li>
{% endif %}
{% endif %}
{% endfor %}
{% assign pages_list = nil %}
{% assign group = nil %}
</ul>
{% endif %}
{% endfor %}

## Main goals of the project:

* to develop a digital gazetteer of Islamic places (_al-Thurayyā_, as a supplement to [pleiades.stoa.org](http://pleiades.stoa.org/)); in collaboration with the team of researchers at the ERC project “The Early Islamic Empire at Work: The View from the Regions Toward the Centre” (PI Stefan Heidemann, Universität Hamburg). The working demos the gazetteer are available at this website (links above)
* to adapt computational framework of [Orbis](http://orbis.stanford.edu/), the Stanford geospatial Network Model of the Roman World (developed by Elijah Meeks and Walter Scheidel), for the historical and geographical data classical available for the Islamic world. The “Islamic Orbis” (_Ṣūraŧ al-arḍ_) will help us to better understand spatial connections within the Islamic world, to visually study geographical and travel literature, and, most importantly, to study ample data from biographical collections by tracing geographies of different social and religious groups.