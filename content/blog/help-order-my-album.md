+++
blogimport = true
categories = ["blog"]
date = "2013-07-08T02:26:00Z"
draft = true
title = "Help Order My Album!"
updated = "2013-07-10T23:07:17.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
And the experiment is over! [Here's the final album ordering](http://erikmcclure.bandcamp.com/album/aurora-theory).

<!--<style type="text/css">
.sortable { 
    list-style-type: none; 
    margin: 0 auto; 
    padding: 0; 
    width: 80%; 
    }
 .sortable li.sortable-dragging{ 
    opacity: .25;
        -webkit-transition: -webkit-transform 0.2s ease-out;
        -moz-transition: -moz-transform 0.2s ease-out;    
    -webkit-transform: scale(0.8); 
    -moz-transform: scale(0.8);
 }
 .sortable li.sortable-over {
    opacity: .25;
 }
 .sortable li {
        list-style: none;
        cursor: move;
        padding: 0 0 0 40px !important;
        margin: 0 !important;
        background: url('https://googledrive.com/host/0B_2aDNVL_NGmQi1kNDNra1VrWms') no-repeat left center;
    }
    .sortable a {
        text-decoration:none;
    }    
    
    [draggable] {
      -moz-user-select: none;
      -khtml-user-select: none;
      -webkit-user-select: none;
      -o-user-select: none;
      user-select: none;
    }
    [draggable] * {
      -moz-user-drag: none;
      -khtml-user-drag: none;
      -webkit-user-drag: none;
      -o-user-drag: none;
      user-drag: none;
    }
    
[draggable] { -moz-user-select: none; -khtml-user-select: none; -webkit-user-select: none; user-select: none; } [draggable] * { -moz-user-drag: none; -khtml-user-drag: none; -webkit-user-drag: none; user-drag: none; }
  </style>{{%blockquote%}}*An Experiment With Maximum-flow*{{%/blockquote%}}I recently completed my first full-length commercial album, which I hope to [sell on bandcamp](http://erikmcclure.bandcamp.com/album/aurora-theory). However, because this album is little more than a collection of songs I made during university, I've wound up with a rather unique problem - I don't know what order to put the songs in! Part of this problem arises from the fact that I have a wide range of genres in this album. You'll find Ambient, Drum'n'bass, Orchestral, Trance, Techno, and even one chiptune-ish song. Because of this, it's hard for me to try and pick a progression of genres that makes sense and gives the album a good sense of flow.

Naturally, I could do what any sensible person would and simply ask my friends to help me sort the songs, but I'm a programmer at heart. So, inevitably, now I'm doing something completely ridiculous: I'll let the internet sort the album! It's also an excuse for me to use a really cool algorithm in a real-world situation.

Below is a list of all 14 songs in the album, each of which may be dragged and dropped by using the handle on the left-hand side. Bandcamp's handy little mini-players will let you listen to each song at your leisure. To prevent contamination of the sample pool, everyone gets a randomized song order (using the Fisher-Yates shuffle), and to prevent abuse, a single IP cannot submit a possible ordering more than once per hour. Don't worry too much about getting the order exactly right - because the initial listing is randomized, if you submit it as is, it will simply vanish in the statistical noise. Focus on ordering songs you feel strongly should come after one another, and these patterns will show up in the results if a lot of people think a particular song should come after another one. If you are interested in the math behind all this, I'll talk about it down below.    
<ul id='sortable' class='sortable'><li id="1"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=1/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="2"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=2/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="3"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=3/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="4"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=4/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="5"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=5/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="6"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=6/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="7"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=7/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="8"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=8/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="9"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=9/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="10"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=10/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="11"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=11/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="12"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=12/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="13"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=13/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li><li id="14"><iframe style="border: 0; width: 100%; height: 42px;" src="http://bandcamp.com/EmbeddedPlayer/album=3490929534/size=small/bgcol=ffffff/linkcol=0687f5/t=14/transparent=true/" seamless>[Aurora Theory by Erik McClure](http://erikmcclure.bandcamp.com/album/aurora-theory)</iframe></li></ul><form id="order_submit_form" action="http://blackspherestudios.com/stats.php" method="post" accept-charset="ISO-8859-1" onsubmit="doOrderSubmit(); return true;"><input type="submit" id="order_submit" value="Submit Ordering">
<input type="hidden" name="order_submit_info" id="order_submit_info" value="-1">
</form>
An album of 14 songs has 14! ways to order the songs - that's 87178291200 possibilities! It's useless to try and sort through this because there is no meaningful information that can be gleaned from it. Instead, when a possible ordering is submitted to the server, it deconstructs it into pairs of songs that come after each other. The first song is always said to come after the 0{{<sup>}}th{{</sup>}} song, or nothing. The other songs are then put in pairs, so an ordering of 7,4,9,2,1,5,10,6,14,13,3,11,8,12 would generate the pairs (0,7),(7,4),(4,9),(9,2) ...etc, which can then be counted. These pairs represent doubletons out of the set of $$\{14\}$$ possible items, so combinatorics tells us that the total number of pairs we have to deal with is $$\binom{14}{2} = 91$$. However, since we're going to be building a directed graph out of this, we actually have to generate seperate edges for (1,7) and (7,1), so that's $$92\cdot2=182$$, plus we need 14 extra edges for linking the 14 vertices to the 0{{<sup>}}th{{</sup>}} song, for a final count of $$182+14=196$$. 

This is much more manageable! But once we've counted up all the submissions and ranked all the pairs, what do we do if, for example, pairs (0,7) and (0,6) are both ranked the same? We can't use both, because they're incompatible with each other. What we do is represent the album order as a walk through the complete graph of 14 songs, or $$K_{14}$$. Technically it's a Hamiltonian Path, which means it touches each vertex exactly once. What this allows us to do is model this as a [maximum-flow problem](http://en.wikipedia.org/wiki/Maximum_flow_problem). Each pair represents a directed edge in our 14 vertex graph, and its ranking yields our capacity. Our 0{{<sup>}}th{{</sup>}} song serves as a source connected to each vertex, and we assign a sink to all vertices, while giving each vertex a demand of 1 so the algorithm is forced to visit all of them. We can then reduce this to a standard maximum flow problem and solve it accordingly. This will yield the optimal album configuration (theoretically speaking, anyway).

Once enough people have submitted potential album orderings, I'll do a second post analyzing the results and describing the solution through the graph in more detail.

<script type="text/javascript" src="https://googledrive.com/host/0B_2aDNVL_NGmeU1ka0lRRGhLdnc"></script>
<script type="text/javascript">    function getRandomInt(min, max) { // unbiased random function between min (inclusive) and max (exclusive) or [min,max)       return Math.floor(Math.random() * (max - min + 1)) + min;     }     function swapElements(obj1, obj2) { // does a proper swap of two elements in the DOM       var temp = document.createElement("div");       obj1.parentNode.insertBefore(temp, obj1);       obj2.parentNode.insertBefore(obj1, obj2);       temp.parentNode.insertBefore(obj2, temp);       temp.parentNode.removeChild(temp);     } function GetXmlHttpObject() {  if (window.XMLHttpRequest) {    return new XMLHttpRequest();  }  if (window.ActiveXObject) {    return new ActiveXObject("Microsoft.XMLHTTP");  }  return null; }  function SendAJAX(url, data, response) { http=GetXmlHttpObject(); if (http==null) {   alert ("Your browser does not support AJAX");   return; }    var params=""; for (var i in data) {   params = params + i + "=" + data[i] + "&"; } if(params.length>0) {   params = params.substring(0, params.length-1); }  http.open("POST", url, true);  //Send the proper header information along with the request http.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); http.setRequestHeader("Content-length", params.length); http.setRequestHeader("Connection", "close");  if(response !== undefined) { http.onreadystatechange = function() {//Call a function when the state changes.  if(http.readyState == 4) {   response(http.responseText);  } } } http.send(params); }     function doOrderSubmit() {       var sortable = document.getElementById("sortable");       var sv = "0 ";       for(var i = 0; i < 14; ++i)         sv = sv + sortable.children[i].id + " ";       document.getElementById("order_submit_info").value=sv;       document.getElementById("order_submit").disabled = true;  /*SendAJAX("http://www.blackspherestudios.com/stats.php", { order_submit_info:sv }, function(response) { if(response.charAt(0)==="y") { document.getElementById("order_submit").value="Success"; } else { document.getElementById("order_submit").value="Failure"; } });*/     }         var sortable = document.getElementById("sortable");         nativesortable(sortable, {             change: function() {                          }         });         var j=0;         for(var i=13;i>0;--i) {           j=getRandomInt(0,i+1);           swapElements(document.getElementById(i+1),document.getElementById(j+1));         }            </script>-->
