## browser compatibility

Source: http://www.alphadigital.cl/blog/lang/en-us/resetear-estilos-css.html

It is the second time that the script `css_browser_selector.js` saves my day. This time I'm taking a better look at the script's advantages and pitfalls. The script's author is Rafael Lima. The script is available at his [site](http://rafael.adm.br/css_browser_selector/). This post is a review about the different ways to distinguish between browsers.

Here is how Rafael's script works. You include the script in your project and.... that's it. Just include his script and it's working. What the script does is add a few classes to the `<html>` tag. If the browser is Firefox3 running on Linux, then the html tag will be like <html class="gecko ff3 linux js">.



It implies that you'll be able to code different behaviors on your site depending on the user's browser/operating system just like this:



<p class="code">

.ff3 body { /* set background color on Firefox3 to yellow. */

    background-color: yellow;

}

</p>



or



<p class="code">

.chrome.linux body { /* Set font to arial only if the browser is Chrome and is running on Linux. */

# Browser Selector Scripts

    font-family: arial;

}

</p>



The drawback is that the script goes into action only after the entire page is loaded. If you have too much layout depending on css_browser_selector and the user browses your site, at first he sees a totally different site, because the operating system and browser classes are not on the html tag yet. From this moment until the browser loads all your page, the page will start changing elements' positions and colors, also depending on your implementation. By the time the browser finishes loading the page, everything will be the way you planned.



There is a different approach. Bastian Allgeier ported the <a href="http://bastian-allgeier.de/css_browser_selector/">css_browser_selector to PHP</a>. This approach has a huge advantage. The browser detection runs on the server side. Your css won't wait until the end of page loading for the browser to apply the right rules. At the moment the browser starts using your style sheet code, the classes about operating system and browser version will already be on the html tag.



The weak point of Allgeier's code is that it does not detect whether JavaScript is enabled in the browser. Another problem is that his code is not behaving well on Chrome. In a test on Chrome 5.0.375.86 beta for Linux, the script returned the classes <span class="code">"webkit safari chrome linux"</span>.



Does anybody know other tricks to determine browser/operating system or solve browser compatibility problems?
