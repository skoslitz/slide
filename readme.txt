http://johnmacfarlane.net/pandoc/demo/example9/producing-slide-shows-with-pandoc.html

Producing slide shows with Pandoc

You can use Pandoc to produce an HTML + javascript slide presentation that can be viewed via a web browser. There are five ways to do this, using S5, DZSlides, Slidy, Slideous, or reveal.js. You can also produce a PDF slide show using LaTeX beamer.

Here’s the markdown source for a simple slide show, habits.txt:

% Habits
% John Doe
% March 22, 2005

# In the morning

## Getting up

- Turn off alarm
- Get out of bed

## Breakfast

- Eat eggs
- Drink coffee

# In the evening

## Dinner

- Eat spaghetti
- Drink wine

------------------

![picture of spaghetti](images/spaghetti.jpg)

## Going to sleep

- Get in bed
- Count sheep
To produce an HTML/javascript slide show, simply type

pandoc -t FORMAT -s habits.txt -o habits.html
where FORMAT is either s5, slidy, slideous, dzslides, or revealjs.

For Slidy, Slideous, reveal.js, and S5, the file produced by pandoc with the -s/--standalone option embeds a link to javascripts and CSS files, which are assumed to be available at the relative path s5/default (for S5), slideous (for Slideous), reveal.js (for reveal.js), or at the Slidy website at w3.org (for Slidy). (These paths can be changed by setting the slidy-url, slideous-url, revealjs-url, or s5-url variables; see --variable, above.) For DZSlides, the (relatively short) javascript and css are included in the file by default.

With all HTML slide formats, the --self-contained option can be used to produce a single file that contains all of the data necessary to display the slide show, including linked scripts, stylesheets, images, and videos.

To produce a PDF slide show using beamer, type

pandoc -t beamer habits.txt -o habits.pdf
Note that a reveal.js slide show can also be converted to a PDF by printing it to a file from the browser.

Structuring the slide show

By default, the slide level is the highest header level in the hierarchy that is followed immediately by content, and not another header, somewhere in the document. In the example above, level 1 headers are always followed by level 2 headers, which are followed by content, so 2 is the slide level. This default can be overridden using the --slide-level option.

The document is carved up into slides according to the following rules:

A horizontal rule always starts a new slide.

A header at the slide level always starts a new slide.

Headers below the slide level in the hierarchy create headers within a slide.

Headers above the slide level in the hierarchy create “title slides,” which just contain the section title and help to break the slide show into sections.

A title page is constructed automatically from the document’s title block, if present. (In the case of beamer, this can be disabled by commenting out some lines in the default template.)

These rules are designed to support many different styles of slide show. If you don’t care about structuring your slides into sections and subsections, you can just use level 1 headers for all each slide. (In that case, level 1 will be the slide level.) But you can also structure the slide show into sections, as in the example above.

Note: in reveal.js slide shows, if slide level is 2, a two-dimensional layout will be produced, with level 1 headers building horizontally and level 2 headers building vertically. It is not recommended that you use deeper nesting of section levels with reveal.js.

Incremental lists

By default, these writers produces lists that display “all at once.” If you want your lists to display incrementally (one item at a time), use the -i option. If you want a particular list to depart from the default (that is, to display incrementally without the -i option and all at once with the -i option), put it in a block quote:

> - Eat spaghetti
> - Drink wine
In this way incremental and nonincremental lists can be mixed in a single document.

Inserting pauses

You can add “pauses” within a slide by including a paragraph containing three dots, separated by spaces:

# Slide with a pause

content before the pause

. . .

content after the pause
Styling the slides

You can change the style of HTML slides by putting customized CSS files in $DATADIR/s5/default (for S5), $DATADIR/slidy (for Slidy), or $DATADIR/slideous (for Slideous), where $DATADIR is the user data directory (see --data-dir, above). The originals may be found in pandoc’s system data directory (generally $CABALDIR/pandoc-VERSION/s5/default). Pandoc will look there for any files it does not find in the user data directory.

For dzslides, the CSS is included in the HTML file itself, and may be modified there.

For reveal.js, themes can be used by setting the theme variable, for example:

-V theme=moon
Or you can specify a custom stylesheet using the --css option.

To style beamer slides, you can specify a beamer “theme” or “colortheme” using the -V option:

pandoc -t beamer habits.txt -V theme:Warsaw -o habits.pdf
Note that header attributes will turn into slide attributes (on a <div> or <section>) in HTML slide formats, allowing you to style individual slides. In Beamer, the only header attribute that affects slides is the allowframebreaks class, which sets the allowframebreaks option, causing multiple slides to be created if the content overfills the frame. This is recommended especially for bibliographies:

# References {.allowframebreaks}
Speaker notes

reveal.js has good support for speaker notes. You can add notes to your markdown document thus:

<div class="notes">
This is my note.

- It can contain markdown
- like this list

</div>
To show the notes window, press s while viewing the presentation. Notes are not yet supported for other slide formats, but the notes will not appear on the slides themselves.

Prev 	 	 Next
Pandoc’s markdown 	Home	 EPUB Metadata