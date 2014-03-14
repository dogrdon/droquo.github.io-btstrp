---
layout: post
category : information design
title : "The Special Collection in the Age of Digital Reproduction"
description : "Preliminary thoughts on sharing digital representations of special collections and rare books."
tags : [libraries, digital books, techne, design, user experience]
---
{% include JB/setup %}

Where I work, a common focus is on how to put books like the one below online for the public to view from a web browser. 

<div class="figure"><img class="blog_post" src="/assets/images/posts/2014/03/andreae_vesalii_thomas_fisher.jpg" title="Andreas Vesalius Title Page Title page (De humani corporis fabrica)" alt="Image of title page to Andreas Vasalius' De humani corporis fabrica, one of the most influential books on human anatomy, published in 1543"/>
<div class="figcaption"><a href="https://www.flickr.com/photos/thomasfisherlibrary/12288507873/">Title page (De humani corporis fabrica)</a> from the Thomas Fisher Rare Book Library <a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by/4.0/80x15.png"/></a></div></div>

While making the initial explorations for a recent project involving the digitization of some rare books, I wondered what it is that makes for a satisfying user experience when it comes to handling digitized books, particulary rare books. These books are in and of themselves as much works of art and craftsmenship as they are vehicles for information. 

Putting the book and its conveyence into historical perspective is <a href="http://www.theatlantic.com/technology/archive/2014/02/life-before-and-after-page-numbers/284002/" target="_blank"> an article from the Atlantic </a> that discusses how paginated books only make up a part of the history of written texts. And as we move into a time of increasing digital reading material, the page marker gives way to other forms of breaking up and pointing to parts of a text.

In the context of the cultural institution (to say nothing of e-books and e-readers), there are already a few different open source book viewers that take on the display of texts for which the text is not the only important feature. What I've found so far (as possible client end solutions for sharing our digital collections) are: <a href="https://openlibrary.org/dev/docs/bookreader" target="_blank">The Internet Archive/OpenLibrary BookReader</a>; <a href="https://github.com/LibraryOfCongress/wdl-viewer" target="_blank"> The Library of Congress' World Digital Library Viewer</a>; <a href="http://www.wellcome.ac.uk/News/Media-office/Press-releases/2014/WTP055613.htm" target="_blank">The Wellcome Library Player</a>; and <a href="http://open.blogs.nytimes.com/2010/03/27/a-new-view-introducing-doc-viewer-2-0/" target="_blank">The New York Times/DocumentCloud Document Viewer</a> (only to name a few). They all appear to be great options and all appear to have fantastic documentation and demos on their sites.

<div class="figure">
	<div class="figcaption" style="width: 480px; display:block; margin: 0 auto;">The Internet Archive/OpenLibrary BookReader</div>
<iframe src="http://www.archive.org/stream/abroadcranethoma00craniala?ui=embed" width="480px" height="480px" style="display:block; margin: 0 auto;" allowfullscreen="allowfullscreen">&nbsp;</iframe>
</div>

<div class="figure">
	<div class="figcaption" style="width: 480px; display:block; margin: 0 auto;">The Library of Congress' World Digital Library Viewer</div>
<iframe src="http://www.wdl.org/en/item/211/view/1/1/" width="480px" height="480px" style="display:block; margin: 0 auto;" allowfullscreen="allowfullscreen">&nbsp;</iframe>
</div>

<div class="figure">
<div class="wellcomePlayer" data-uri="http://wellcomelibrary.org/package/b18021013" data-assetsequenceindex="0" data-assetindex="0" data-zoom="-0.8052,0,2.6105,1.4068" data-config="/service/playerconfig" style="width:480px; height:480px; background-color: #000; display: block; margin: 0 auto;" ></div>
<script type="text/javascript" id="embedWellcomePlayer" src="http://wellcomelibrary.org/spas/player/build/wellcomeplayer/js/embed.js"></script><script type="text/javascript">/* wordpress fix */</script>
<div class="figcaption">The Wellcome Library Player</div>
</div>

Each of these, and other viewers online, have their indivdual design decisions as they were all built with slightly different general use cases in mind. Some have the book paging facsimile, showing both pages side by side and approximating the movenment of pages being turned, some do not. Some offer a paging animation clicking on the pages themselves, some only buttons in a control panel, making for a more fluid mobile/touchscreen experience. Most of them zoom. What we might be seeing though is the settling of a grammar for interacting with digital book displays.

And they do share a few commonalities that point towards the future of viewing books (or paintings, or documents or even videos and audio files online). Many of them have full text search. Most have the option to view pages as thumbnails to more easily browse through an entire book at a glance. Some provide windows for tables of contents and object metadata.

Though in reviewing these different viewers, I am asking myself what it is that would make a book in digital format true to its historical character, true to it's character as a physical, primarily analog artificat, but also useful and compelling in a contemporary, digital experience. 

While I acknowledge the desire to have the same interaction at the digital level as I would at the analog level, I don't feel, for instance, that the page turning approximation in some of the players really added very much to the experience. Instead, I almost feel as if it removes and distracts from the experience. //Perhaps it's me thinking the entire time, with my particular point of view and the fact that ultimately I will be the one responsible for implementing this solution technologically: "How would one get this to work on a code level?" And in that, I am distracted and, while drawn in by the marvel of seeing th pages turn digitally, I am no longer paying any attention to the work being represented. It's there, but I am too caught up in the technology.//

Sometimes being caught up in the technology is the goal. It is as noted in another blog post I came across recently about UI experiences: <a href="http://joshtimonen.com/post/79486778102/my-brain-has-no-space-for-your-user-interface" target="_blank">"Sometimes their efforts create UIs that scream “Look at how advanced our software is! Come join us in the future!”</a>. As though some UIs exist to shore up underwhelming content. But for special collections, rare books that compel in and of themselves, we want to grant access and accentuate their best features (seeingt he texture of the paper and the printing impressions, seeing the images and illuminations, being able, of course, to read the text) without calling too much attention to the technology used to display them.    

When making design desisions, no matter what, you have to leave out somethings, and include things at the expense of other features or affordibilities. Often these decisions come down come down to a matter of time, money, or conviction and persuassion by certain stakeholders. So I am not sure if I am correct on this or not, or if there is any correct answer. But with that I'll make sure to document what follows. 

