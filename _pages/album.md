---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
permalink: /album/
layout: album
---
<section class="jumbotron text-center">
<div class="container">
<h1>Album example</h1>
<p class="lead text-muted">Something short and leading about the collection below—its contents, the creator, etc. Make it short and sweet, but not too short so folks don’t simply skip over it entirely.</p>
<p>
<a href="#" class="btn btn-primary my-2">Main call to action</a>
<a href="#" class="btn btn-secondary my-2">Secondary action</a>
</p>
</div>
</section>
<div class="album py-5 bg-light">
<div class="container">
<div class="row">
{% for vid in site.data.vids %}
      <div class="col-sm-3">
<div class="card mb-4 shadow-sm">
<video width="100%" controls="">
<source src="/video/{{ vid.link }}" type="video/mp4">
Your browser does not support HTML video.
</video>
<div class="card-body">
<h2>{{ vid.name }}</h2>
<p class="card-text">{{ vid.description }}<br />press the play button for a demo</p>
<div class="d-flex justify-content-between align-items-center">
<div class="btn-group">
  <input type="checkbox" id="vidid" name="checkbox1" value="1">
   <label for="checkbox1">&nbsp; Use</label><br>
</div>
<small class="text-muted">9 mins</small>
</div>
</div>
</div>
</div>
  {% endfor %}

</div>
</div>
</div>