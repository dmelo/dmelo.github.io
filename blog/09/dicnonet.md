

# Dicno.net

Today's post is about [dicno.net](http://dicno.net).

As the background of this blog suggests, I'm a Brazilian guy, and a very patriotic one. My English sucks but I like to write my posts in English. This way all my friends can read it :D

The subject of this post is my very new web system. A dictionary for Brazilian people's names, [dicno.net](http://dicno.net). The web system is a small mashup. There is data from four different sources, which (I like to think) is a very interesting mix. The idea of the site is to provide the maximum information possible about somebody's name.

There are photos from Google, meaning of the name from [dicionariodenomesproprios.com.br](http://dicionariodenomesproprios.com.br), luck of the day from Orkut and people who have the same name from Wikipedia.

This is a technical post. I present here the technical background that I'm using on [dicno.net](http://dicno.net).

###URLs

Each name has a URL. The latin charset includes a few accents on letters. Those accents appear in a lot of names like "João", "José" and so on. There are also some names that are written both ways (with or without graphic signs). To avoid using more than one URL for the same name, names with accents are converted into the equivalent name without accents. "João" is transformed into "Joao".

The web system is using [Zend Framework](http://framework.zend.com/). I changed the way that Zend reads the URL so that it considers the first string, after the root path, the name that the user is trying to access. This way we keep the URL very short. João's page is at [http://dicno.net/Joao](http://dicno.net/Joao).


###The Mashup Part

Google images come through [Google AJAX Search API](http://code.google.com/apis/ajaxsearch/), which means the images that appear on every page are loaded dynamically.

Apart from the Google images, all content comes from the server side; it is not loaded dynamically. This measure is for SEO purposes. If the site was populating all the content using AJAX, then search engines wouldn't find any content. As far as I know those search engines don't run the JavaScript to see if there is more information.

The site [dicionariodenomesproprios.com.br](http://www.dicionariodenomesproprios.com.br) does not have an API for foreign sites to access its content. So, I'm using [Zend\_Dom\_Query](http://framework.zend.com/manual/en/zend.dom.query.html) to parse the site's content and get the information  I want.

The content from [Wikipedia](http://en.wikipedia.org) comes in a very elegant way, through [DBpedia](http://dbpedia.org). DBpedia is a very interesting effort to make the internet (or at least Wikipedia) accessible by its semantics. When you try to access somebody's name, the server requests the list of people who have the same name.

The luck phrases are the most shameless copy and paste I ever did. No big deal. Just copy a few phrases into a MySQL table and that's it.

###Performance

If a user has a certain delay while accessing a page that has its content stored all together on the same server, imagine what happens when that user accesses a page whose content is spread over several sources, on different servers across the world. The total time to access it is the time between the browser and dicno plus the longest delay among all the foreign sources and dicno (considering they are accessed in parallel).

There is a solution to make it incredibly faster. The miracle word is CACHE. The meaning of the name that a user is searching doesn't change, but my sources could update their contents. So, a one-month cache sounds quite good. The Google images are the only foreign source that is not cached.

Every time that a user searches for somebody's name, the server searches for the cache files (I'm using [Zend_Cache](http://framework.zend.com/manual/en/zend.cache.html) for that). In cache-miss cases, the server consults the foreign sources and stores the results in the cache.

The system also presents the top names (the most visited names) and, on each page, suggests names that might be of interest to the user. In both cases the results are also stored in the cache. The list of names is stored on a MySQL database and I want to avoid accessing it because the communication between the hosting server and the MySQL database is slow.

###Conclusion

I did [Dicno](http://dicno.net) because I was looking for some fun and I wanted to start and end a project on the same weekend. Besides the cache implementation and this post, everything was done in two lazy days.

[Dicno](http://dicno.net) is fast but there are some minor details that could make it even faster. I plan to work on it later. So far, I'm very pleased with the results.
