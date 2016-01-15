---
layout: default
title: Gallery 
header: Pages
group: navigation
---
{% include JB/setup %}

<!-- Gallery Last days in HDU -->
<h4>Last days in HDU</h4>
<div>
{% for image in site.static_files %}
	    {% if image.path contains '/assets/images/LastDaysInHDU' and (image.extname == '.jpg' or image.extname == '.png') %}
			<a class="LastDaysInHDU-link" href="{{ site.baseurl }}{{ image.path }}" data-lightbox="LastDaysInHDU-set" data-title=""><img class="LastDaysInHDU-image" src="{{ site.baseurl }}{{ image.path }}" alt="Last days in HDU" style = "width:250px"/></a>
		{% endif %}
{% endfor %}
</div>

<hr/>

<!-- Gallery Flowers -->
<h4>Flowers</h4>
<div>
{% for image in site.static_files %}
	    {% if image.path contains '/assets/images/Flowers' and (image.extname == '.jpg' or image.extname == '.png') %}
			<a class="Flowers-link" href="{{ site.baseurl }}{{ image.path }}" data-lightbox="Flowers-set" data-title=""><img class="Flowers-image" src="{{ site.baseurl }}{{ image.path }}" alt="Flowers" style = "width:250px"/></a>
		{% endif %}
{% endfor %}
</div>

