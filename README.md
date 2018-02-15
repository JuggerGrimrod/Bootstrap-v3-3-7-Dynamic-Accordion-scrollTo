# Dynamic Bootstrap Accordion scrollTo() Plus 'Lazy-Load' Unveil.js (using Bootstrap 3.3.7, jQuery 3.3.1 and Unveil.js)  (@ /JuggerGrimrod/Bootstrap-v3-3-7-Dynamic-Accordion-scrollTo)

## This Git repository contains markup, script and style assets for a demonstration of Bootstrap 3.3.7 dynamic accordions with a scroll-to feature, dynamic ID/HREF attribute value generation, dynamic accordion drawer generation, and product-image lazy-loading courtesy of the Unveil.js plugin.  

This demonstration satisfies the requirements of a an accordion system wherein each accordion drawer represents a product category, with a category content div containing a grid of product images and titles, each housed within a div tag.  Additionally, when an accordion drawer is opened by the user, the viewport will scroll back up to the top of the newly opened accordion drawer (this behavior is not inherent to Bootstrap js).

In this demonstration:
  * A user's viewport will scroll to the top of a newly opened accordion panel as the panel is opened and displayed.  
  * Lazy-loaded placeholder images within an opened panel (i.e. *shown.bs.collapse*) are displayed via the Unveil.js plugin; the first three (3) images are rendered by default via the plugin's initilization script, with subsequent images loaded as they appear in and/or are scrolled-to in the viewport.
  * Each accordion panel contains 24 image placeholders with the total panel height exceeding 1080px.    
  * jQuery function for cloning (using the *.clone()* method) new drawers and appending them (using the *.appendTo()* method) to the *#accordion* div object after the page has completed loading, triggered by *APPEND AN ACCORDION DRAWER* button clicks.
  * Dynamic serialization of accordion drawers, drawer trigger links, and category numbers both on *document.ready* and on *APPEND AN ACCORDION DRAWER* button clicks.

This demonstration uses a single .html page running **Bootstrap v3.3.7** and the **jQuery 3.3.1** library.  All Bootstrap JS and CSS files implemented in this demonstration are minified, and are linked in the HEAD (CSS) and BODY (JS) of the **/index.html** file.

* ### /index.html file contains the script, style and content assets for this demonstration:
  * A total of **eight accordion drawers are hard-coded in this page** and displayed by default.  Additional accordion drawers can be appended to the *#accordion* object by clicking the **APPEND AN ACCORDION DRAWER** button.  
  * **CSS assets** are linked in three external files:
    * **/css/bootstrap.3.3.7.min.css** - Bootstrap UI features called in **/index.html on line 8**.
    * **/css/fa.css** - FontAwesome icon elements called in **/index.html on line 9**.
    * **/css/styles.css** - custom UI features called in **/index.html on line 10**.
  * **JS assets** are linked in two external files:
    * **/js/jquery.3.3.1.min.js** - jQuery 3.3.1 library file called in **/index.html on line 1113**.
    * **/js/bootstrap.3.3.7.min.js** - Bootstrap 3.3.7 library file called in **/index.html on line 1114**.  
  * **Custom JS** is defined in **/index.html on lines 1120 to 1194**:
    * The Accordion Dynamic Serialization script function *serializeDrawer()* is defined in **/index.html on lines 1121 to 1146**:
    ```javascript
    // ACCORDION DYNAMIC SERIALIZATION
    // set .accordion-drawer ID value and companion .trigger-link HREF attribute value on document.ready
    // to see the dynamic .accordion-drawer/.trigger-link script in action, load this page in a browser
    // to see dynamic serialization after page load, click the APPEND EXTRA ACCORDION DRAWERS button
    function serializeDrawer(){
        var i=0;
        $('.trigger-link').each(function(){
            i++;
            var newHREF='#'+'drawer'+i;
            $(this).attr('href',newHREF);
            $(this).val(i);
        });
        var j=0;
        $('.accordion-drawer').each(function(){
            j++;
            var newID='drawer'+j;
            $(this).attr('id',newID);
            $(this).val(j);
        });
        var k=0;
        $('.serialNumber').each(function(){
            k++;
            var newCOUNTER=+k;
            $(this).html(newCOUNTER);
        });
    }
    ```    
    * The Accordion scrollTo script function *panelScroll()* is defined in **/index.html on lines 1169 to 1188**:
    ```javascript
    // ACCORDION SCROLLTO SCRIPT 
    // keep opened accordion drawers' panels from scrolling out of view through the top of the viewport 
    // (i.e. return viewport to top of opened accordion after it's contents are displayed)
    function panelScroll(){
        $('.panel-collapse').on('shown.bs.collapse', function(){
            // define width of the window as variable (for later use in mobile viewports)
            var winW = $(window).width();
            // define nearest panel object in the DOM (the .panel that'll be clicked open/displayed)
            var $panel = $(this).closest('.panel');
            // define scroll-offset (optional): adjust height px value to account for content above panel 
            // (including masthead/title-bar if they exist)
            var offsetPX = 0; // zero animates the opened .panel div to the top of the viewport
            // change offset value in mobile display if necessary 
            if(winW <= 767){
                var offsetPX = 0; // zero animates the opened .panel div to the top of the viewport
            }
            // animate the scroll behavior to height of offset pixel value, duration 1s
            $('html,body').animate({
                scrollTop: $panel.offset().top + offsetPX
            }, 1000);             
        });      
    };
    ```
    * The *appendAccordion()* function defines the *APPEND AN ACCORDION DRAWER* button behavior, demonstrating dynamic accordion generation, and is defined in **/index.html on lines 1148 to 1153**:
    ```javascript
    // APPEND ACCORDION DRAWERS
    // clones and appends new accordion drawer to #accordion on .accordionBtn button click 
    // dynamically generates ID/HREF values and .serialNumber().html() with serializeDrawer() callback
    function appendAccordion(){           
        $('.panel:last-child').clone().appendTo('#accordion');
        serializeDrawer();
    } 
    ```
  * The **Unveil.js plugin script**  courtesy of [Luis Almeida](http://luis-almeida.github.io/unveil/):
    * **/js/jquery.unveil.js** - Unveil.js plugin script file is called in **/index.html on line 1118**:
    * The *unveilImages()* function defines the default **Unveil.js callback** and accordion drawer triggering behavior in **/index.html on lines 1155 - 1167**:
    ```javascript
    // LAZY-LOADING X PRODUCT IMAGES IMMEDIATELY ON DOCUMENT.READY USING UNVEIL.JS
    // default lazy-loading via unveil.js callback function 
    function unveilImages(){
        $('.product-image').unveil(1000, function(){
            $(this).on('load',function(){
                this.style.opacity = 1;
            });
        });
        // unveil the first 3 images by default (on the accordion .panel-heading click/'open' event)
        $('.panel-heading', this).on('click', function(){
            $(this).next('.panel-collapse').find('img.product-image:lt(3)').trigger('unveil');
        });
    }
    ```

The 24 placeholder thumbnail images and 24 full-size placeholder images are used inside each accordion drawer's .panel-body div.  This means your browser will cache all of the non-loaded full-size images after your first click into an accordion drawer, when Unveil.js lazy-loads the images into DOM and display.  The lazy-loading behavior is best observed from your browser console's networking tab (which shows assets a they are loaded into the DOM in real-time) and when scrolling down through the first accordion drawer you open.

This demonstration was tested from a production Linux server running PHP v5.6.33 in Chrome 64.0.3282.140 (Official Build) (64-bit), Firefox 58.0.2 (64-bit), IE Edge, IE10 and IE9 (emulated via IE Edge console), iOS Mobile Safari via iPad Air 2, and Android Chrome.

### COMMENTS GALORE: Comments appear throughout the **/index.html** file; if you need to figure out what a given script or layout element is doing, chances are very good that there are comments explaining in detail what is going on with each component within each file.

Future enhancements for this demonstration include:
  
  * Clone of this Github demonstration using **Bootstrap 4.0.x**.

That's it.  Enjoy!

## Dynamic Bootstrap Accordion scrollTo() Plus 'Lazy-Load' Unveil.js README.md file v1.0.0 

### Free to all via GNU General Public License v3.0.
