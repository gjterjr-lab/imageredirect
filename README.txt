IMAGE REDIRECT WEBSITE
======================

What it does
------------
- Shows one full-screen image (no text, no buttons, nothing else).
- Click/tap anywhere -> goes to https://technestuk.blogspot.com/
- No click -> goes there automatically after exactly 10 seconds.
- Image changes every 30 seconds, in order (image1, image2, ... then back to
  image1). It is based on the real clock, so every visitor sees the same image
  at the same moment, and refreshing does not restart it.

Files
-----
index.html     The website (all code is inside this one file)
build.js       Helper that finds your images (needs Node.js)
build.py       Same helper, for Python (use either one)
images/        Put your pictures here (your 3 pictures are already in it)
README.txt     This file


HOW TO ADD / CHANGE IMAGES  (no code editing)
---------------------------------------------
1. Delete the current pictures in the "images" folder (image1.jpg, image2.jpg, image3.jpg).
2. Put your pictures in "images" and name them in order:

       image1.png
       image2.jpg
       image3.webp
       image4.jpeg
       image5.gif
       ...

   Extensions can be mixed. Supported: jpg, jpeg, png, webp, gif, avif, bmp, svg.
   Numbers must be continuous (1, 2, 3 ...). The site stops at the first missing
   number, so if image4 is missing, image5 and later are NOT used.

3. Run the helper ONE time from inside this folder:

       node build.js          (if you have Node.js)
   or
       python build.py        (if you have Python; on Mac/Linux: python3 build.py)

   It prints the list of images it found and writes images/manifest.js
   (and manifest.json). It warns you about gaps or duplicate numbers.

4. Upload the whole folder again to your hosting (see below). Done.

Why step 3? Normal web hosting does not let a web page list the files in a
folder, so the helper writes the list for the site. You never edit the list.
(Important: the page uses ONLY images/manifest.js as its list. If you forget
step 3, new images will not appear.)


HOW TO PUT IT ONLINE (easiest options, all free)
------------------------------------------------
Option A - Netlify Drop
  1. Go to https://app.netlify.com/drop
  2. Drag this whole folder onto the page. You get a link instantly.

Option B - Cloudflare Pages or GitHub Pages
  Create a new project, upload these files, and publish. No build command
  is needed (leave it empty).

Option C - Any normal web host
  Upload index.html and the images folder (including manifest.js) to the
  public folder (public_html).

Note: Blogger/Blogspot cannot host this page. Use one of the options above.

You can also double-click index.html to test it on your own computer.


HOW TO CHANGE THE DESTINATION LINK
----------------------------------
Open index.html in Notepad. Near the top of the <script> there is a block
marked "SETTINGS". Change only this line:

    const DESTINATION_URL = "https://technestuk.blogspot.com/";

The same block also holds the redirect delay (10 seconds) and the image
change time (30 seconds).


KEEP IMAGES SMALL (important on slow internet)
----------------------------------------------
Big files load slowly and visitors may leave before they appear. Aim for
under 500 KB per image: save as JPG (quality about 80-85) and use a width of
about 1080-1600 pixels. Phone-sized pictures (e.g. 1080 x 1920) are plenty.
Free tool: https://squoosh.app  (or any "compress image" website).

The page waits for the picture before it starts the 10-second countdown, so
people on slow connections still get the full 10 seconds with the picture
showing. If nothing can be shown within 30 seconds it redirects anyway.
Both behaviours can be changed in the SETTINGS block of index.html
(START_TIMER_WHEN_IMAGE_APPEARS and GIVE_UP_WAITING_SECONDS).


NOTES
-----
- Every visitor sees the same image at the same time because the image is
  chosen from the clock:  image number = (seconds since 1970 / 30) rounded
  down, remainder after dividing by the number of images.
- If a visitor's phone/computer clock is wrong, the page compares it with the
  web server's clock and corrects itself (when the difference is over ~2.5s).
- Because the page redirects after 10 seconds of viewing, most visitors will only see
  one image per visit. The rotation matters across visits: someone arriving
  at 12:00:00 and someone at 12:00:35 will see consecutive images.
- If one image is broken/corrupt, the page skips to the next working image
  instead of showing a broken icon or a black screen.
- If the visitor's browser has JavaScript turned off, nothing is shown
  (the page needs JavaScript to pick the image and redirect).
