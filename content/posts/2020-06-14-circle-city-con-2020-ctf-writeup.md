+++
title = 'Circle City Con 2020 CTF Writeup'
author = "Sam Craven"
date = 2020-06-14T16:50:08
slug = "circle-city-con-2020-ctf-writeup"
summary = "My writeup on my successes and, uh, learning opportunities from the 2020 Circle City Con CTF."
categories = ["tech", "ctf"]
tags = ["tech", "ctf"]
featured_image = "/images/CCC_2020_CTF_header.png"
+++


# Table of Contents
* [Successes](#successes)
* * [Getting in](#gettingin)
* * [MYSTERY:free-space](#mysteryfreespace)
* * [MYSTERY:doot-doot](#mysterydootdoot)
* * [FORENSICS:emotional-consequences](#forensicsemotionalconsequences)
* * [PROGRAMMING:program](#programmingprogram)
* [~~Failures~~ Learning Opportunities](#failureslearningopportunities)
* * [MYSTERY:totally-not-stolen](#mysterytotallynotstolen)
* * [OSINT:hunker](#osinthunker)
* * [OSINT:lockdown](#osintlockdown)
* [Afterward](#afterward)
# Successes
## Getting in:
Hint in #ctf in the con Discord:

>"Decode this clue to assist me in combating the apocalypse!" -Nostradamus
>
>`YUhSMGNITTZMeTlrYVhOamIzSmtMbWRuTHpOelowWm5jRU09`

Base64Decode that to get
`aHR0cHM6Ly9kaXNjb3JkLmdnLzNzZ0ZncEM`

Base64Decode _that_ to get
`https://discord.gg/3sgFgpC`

## MYSTERY:free-space
>These might as well be free points, good luck:
>
>`00110100 00110011 00100000 00110100 00110011 00100000 00110100 00110011 00100000 00110111 01100010 00100000 00110100 01100100 00100000 00110111 00111001 00100000 00110010 00110000 00100000 00110100 00110110 00100000 00110011 00110001 00100000 00110111 00110010 00100000 00110111 00110011 00100000 00110111 00110100 00100000 00110010 00110000 00100000 00110100 00110110 00100000 00110110 01100011 00100000 00110110 00110001 00100000 00110110 00110111 00100000 00110010 01100101 00100000 00110010 00110000 00100000 00110101 00111001 00100000 00110110 00110101 00100000 00110111 00110000 00100000 00110010 01100011 00100000 00110010 00110000 00100000 00110110 00111001 00100000 00110111 00110100 00100000 00110010 00110000 00100000 00110111 00110010 00100000 00110110 00110101 00100000 00110110 00110001 00100000 00110110 01100011 00100000 00110110 01100011 00100000 00110111 00111001 00100000 00110010 00110000 00100000 00110111 00110111 00100000 00110110 00110001 00100000 00110111 00110011 00100000 00110010 00110000 00100000 00110111 00110100 00100000 00110110 00111000 00100000 00110110 00110001 00100000 00110111 00110100 00100000 00110010 00110000 00100000 00110110 00110101 00100000 00110110 00110001 00100000 00110111 00110011 00100000 00110111 00111001 00100000 00110010 00110001 00100000 00110111 01100100`

Binary -> ASCII that to get
`43 43 43 7b 4d 79 20 46 31 72 73 74 20 46 6c 61 67 2e 20 59 65 70 2c 20 69 74 20 72 65 61 6c 6c 79 20 77 61 73 20 74 68 61 74 20 65 61 73 79 21 7d`

Hex -> ASCII that to get
`CCC{My F1rst Flag. Yep, it really was that easy!}`

It took me a bit to work out the proper format for communicating flags. I always joke that this is the _real_ first flag. Messaging the bot with `!flag free-space "CCC{My F1rst Flag. Yep, it really was that easy!}"` found joy.

10 points - I'm on the board!

## MYSTERY:doot-doot
>message received...
>
>https://nostradamus.imfast.io/dots3.wav

I've duplicated that file [[here]](__GHOST_URL__/files.seemsgood.net/ccc-2020-ctf/dots3.wav) in case that link dies some day.

Step 1, the easy guess - listen to it. Nope. That's unintelligible. Damn, I was hoping for some easy morse.

Step 2: Drop it into [Audacity](https://www.audacityteam.org/) and let's do some analysis on it. Immediately after loading in, I think "huh, that kinda looks like maybe the Morse is spread out over a bunch of frequencies." I make a note to attempt to translate it after this next thing I'm going to try. 

![dots3-audacity](__GHOST_URL__/content/images/2020/06/dots3-audacity.png)

Asking Audacity to output the [spectogram](https://en.wikipedia.org/wiki/Spectrogram) makes the flag pop-out almost plain as day!

![dots3-spectogram](__GHOST_URL__/content/images/2020/06/dots3-spectogram.png)

In case that's hard to read, it's `CCC{I wouldn't wear new slippers on a full moon.}` A quick message to the CTF bot confirms!

15 points - up to 25 now!

## FORENSICS:emotional-consequences
>Please recover the sensitive information from this redacted document.
>
>https://nostradamus.imfast.io/NotIncluded.pdf

(File duplicated [[here]](https://files.seemsgood.net/ccc-2020-ctf/NotIncluded.pdf))

A redacted PDF. There's been some funny stuff with PDFs not being properly redacted. Often, a black box is painted over text rather than properly using [Acrobat's redaction feature](https://helpx.adobe.com/acrobat/how-to/redact-pdf.html). Let's try that first. Select all, copy, paste into a text editor. And there it is!

`CCC{TroyandAbedintheFlaaags}`

10 points - 35 total!

## PROGRAMMING:program
>You can tell it's bad when people stop commenting their code, but when they don't even tell you what language it is, it's gotta. be the end of the world.
>
>https://nostradamus.imfast.io/program

(File duplicated [[here]](https://files.seemsgood.net/ccc-2020-ctf/program))

My first try was just to run this in Bash. As root on my desktop, obviously (no, not really). It, of couse, spits out a syntax error.

I open a text editor and fumble my way around trying to make this a bit more readable. I think it I _had_ to I could walk through the steps myself if I'm unable to get it to run.

I re-read the prompt.

>they don't even tell you what language it is

Hrm. I wonder if this will just run in some language. I try python 3. Syntax error. I try python 2. Syntax error. Hrm.

I ponder a bit more. What's the language that people always joke that no one can read it? Perl! I run the code in Perl. Of course, it runs.

It's a bit slow, so I guess I wouldn't have been able to manually process the information.

![program](__GHOST_URL__/content/images/2020/06/program.gif)

```
rot13
PPP{Arire gehfg na ryrpgevpvna jvgu ab rlroebjf}
```

Run that output through a [ROT13](https://en.wikipedia.org/wiki/ROT13) cipher and you get `CCC{Never trust an electrician with no eyebrows}`

20 points - 55 total!


And that's as far as I got. But I did a lot more work on other challenges! And here it is:

# ~~Failures~~ Learning Opportunities
## MYSTERY:totally-not-stolen

>Since we're no longer able to play in a conference hotel, we figured we could bring the hotel to you. Here are two hotel keys that we totally borrowed and dumped for you. We believe that you should be able to compare the check in, check out, and room number associated with each key. On the first key, we can tell you that the owner 
>
>* \[1\] checked into their room @ 12:02 on June 8, 2019
>* \[1\] checked out @ 11:00 on June 15, 2019
>
>What was the room number and check-in time for time for the second key? 
Make sure to setup the correct flag format and answer the question in this style: RoomNumber+MM-DD-YY_HH:MM
>
>* Key1: d09797dfac9704d82597979791d49797979797979695859f918e978682918e1a
>* Key2: 7334347c0f34a77b863434343063343434343434351c1631322d342521322db9

My first guess: Base64Decode again!

Nope. Damn, I'm gonna have to work on this one!

My first thought is that we've been given these dates because they're in there somewhere. Let's see what the [epoch time](https://en.wikipedia.org/wiki/Unix_time) for those dates is. Because I don't use the tool often enough, this means that I get to google "man date" which is always elicits a chuckle when it returns [exactly what I'm looking for](https://www.man7.org/linux/man-pages/man1/date.1.html).

Indy is in EDT in June and I'm in CDT so I'll need to specify that. A quick check confirms that I'm passing the time zone correctly. Then, a [freshly learned bash trick](https://www.saotn.org/convert-decimal-hex-bash/) converts it to hexadecimal. 

```
sam@Neptune:~$ date -d '2019-06-08 12:02 EDT'
Sat Jun  8 11:02:00 CDT 2019
sam@Neptune:~$ date -d '2019-06-08 12:02 EDT' +%s
1560009720
sam@Neptune:~$ printf '%x\n' $(date -d '2019-06-08 12:02 EDT' +%s)
5cfbdbf8
sam@Neptune:~$ 
```

Hrm. That's not anywhere in the strings we have. Maybe this hotel uses [The One True Time Zone](https://en.wikipedia.org/wiki/Coordinated_Universal_Time).

```
sam@Neptune:~$ date -d '2019-06-08 12:02 UTC'
Sat Jun  8 07:02:00 CDT 2019
sam@Neptune:~$ date -d '2019-06-08 12:02 UTC' +%s
1559995320
sam@Neptune:~$ printf '%x\n' $(date -d '2019-06-08 12:02 UTC' +%s)
5cfba3b8
sam@Neptune:~$ 
```

Hrm. That's not it either.

I try converting it to/from ASCII, hex, and decimal, and don't get anywhere.

Let's try for the other date.

```
sam@Neptune:~$ date -d '2019-06-15 11:00 EDT'
Sat Jun 15 10:00:00 CDT 2019
sam@Neptune:~$ date -d '2019-06-15 11:00 EDT' +%s
1560610800
sam@Neptune:~$ printf '%x\n' $(date -d '2019-06-15 11:00 EDT' +%s)
5d0507f0
sam@Neptune:~$ date -d '2019-06-15 11:00 UTC'
Sat Jun 15 06:00:00 CDT 2019
sam@Neptune:~$ date -d '2019-06-15 11:00 UTC'
Sat Jun 15 06:00:00 CDT 2019
sam@Neptune:~$ date -d '2019-06-15 11:00 UTC' +%s
1560596400
sam@Neptune:~$ printf '%x\n' $(date -d '2019-06-15 11:00 UTC' +%s)
5d04cfb0
sam@Neptune:~$ 
```

No joy to be found here, either. I know the date is stored on the card but I think I can pretty reasonably say that it's not in epoch format.

I bash my head against encoding/decoding with Base64, decimal, and some more esoteric things for a while before deciding to come back to this one later.

## OSINT:hunker
>To escape the inevitable, 4880 nautical miles east from where we stood, this aptly named site will do just fine as a bunker.
>
>(This flag does not have the standard format.)

This seems like one where the idea will be easy but the execution will be a bit difficult. My guess is that we're looking for an object or place 4,880 nm east of the Indy Westin. I'd also guess that this isn't going to be something super big and easy to spot like "Paris." so simply scrolling to the east won't help.
This is easy enough in concept but the execution isn't so easy. There used to be [free tools](https://www.freemaptools.com/radius-around-point.htm) that could do drawings over Google Maps but as the price of their API climbed, these tools mostly died.

But before I go mucking about trying to build something in the Google API, let's try another simple way to accomplish this - get the location and ask Wolfram what is 4800 nm away from it. I used [LatLong.net](https://www.latlong.net/convert-address-to-lat-long.html) and input the Indy Westin's address and it gave me coordinates (39.766281, -86.163887). I input `4800 nautical miles east of (39.766281, -86.163887)`. Wolfram doesn't like that query. I'll convert to remove the "nautical" to make it less likely to trip up. 4,800 nm is 5,524 mi - `5,524 miles east of (39.766281, -86.163887)`. Still no joy. I try just the lat/lon. It doesn't give me a map. Okay, so maybe that's why it was tripping up. I input `39.766281N, 86.163887 W` and it pulls up a map straightaway. I input a handful of different ways to indicate "4,800 nm east of there" but I'm not able to get anything. It looks like I'll be using Google Maps after all.

First, create a map:

```
function initMap() {
  // Create the map.
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 1,
    center: {lat: 0, lng: 0},
    mapTypeId: 'terrain'
  });
// Drawings on the map will go here
}
```

Unfortunately, I can't draw a line of a specific length on Google Maps. I can however draw a line around the lattitude and then add a radius and see where those points match.

Drawing the line:

```
  var line_coordinates = [
    {lat: 39.766, lng: -86.164},
    {lat: 39.766, lng: 0},
    {lat: 39.766, lng: 90},
    {lat: 39.766, lng: -180},
    {lat: 39.766, lng: -86.164}
  ];
  var map_line = new google.maps.Polyline({
    path: line_coordinates,
    geodesic: false,
    strokeColor: '#FF0000',
    strokeOpacity: 1.0,
    strokeWeight: 2
  });

  map_line.setMap(map);
```

Drawing the radius:

```
var indyCircle = new google.maps.Circle({
  strokeColor: '#FF0000',
  strokeOpacity: 0.8,
  strokeWeight: 2,
  fillColor: '#FF0000',
  fillOpacity: 0.35,
  map: map,
  center: {lat: 39.766, lng: -86.164},
  radius: 8889600 //4800nm in meters
});
```




<iframe src="https://files.seemsgood.net/ccc-2020-ctf/hunker-map.html" title="Hunker map" width="100%" height="100%"></iframe>

I tried a variety of markers and towns around this point. I was most confident in Bükerler, looking similar to the word "bunker" in the clue, but that wasn't it. At this point I'm either guessing at what thing around that spot on the map I'm supposed to input or I'm wrong about what "where we stood" means.

## OSINT:Lockdown

>Considered the Cinderella of the Garden, accompanied by シガナ, how many can you find at 78N, 15E?
>
>(This flag does not have the standard format.):

Finding out what "Cinderella of the Garden" means requires quoting the phrase in Google - otherwise you'll get a lot of Cinderellas that happen to occur in gardens. The [first result](https://www.silive.com/homegarden/garden/2008/06/the_cinderella_of_the_garden_w.html) is kind enough to not just provide the name (Orchid Cactus) but also the genus - [Epiphyllum](https://en.wikipedia.org/wiki/Epiphyllum)!

シガナ is a bit more difficult. Google translate says it means "Sigana" in English but I'm not able to find anything relevant with that.

Entering "78N, 15E" into Google Maps takes me to Spitsbergen, Svalbard and Jan Mayen. Oh hey, Svalbard sounds familiar. I wonder if this is related to the [Svladbard Global Seed Vault](https://en.wikipedia.org/wiki/Svalbard_Global_Seed_Vault). The coordinates listed on that Wiki page match up! My guess is that the flag will be the number "Cinderella of the Garden" and "シガナ" in the vault.

The Svladbard Global Seed Vault posts [its entire inventory online](https://www.nordgen.org/sgsv/index.php?app=data_unit&unit=sgsv). You can also download the data for  your own searcing. Neither "epiphyllum" nor "sigana" return any results. Maybe I was wrong about which plant is the "cinderella of the garden."

I search a bit more. I find two more flowers described as such "cinderella of the garden" ([genus zinnia](https://bloominghillva.blogspot.com/2010/08/zinnia-cindrella-of-garden.html) and [genus chaenomeles](https://davesgarden.com/guides/articles/view/2128). I hate CTFs.

Zinnia has 13 results in the seed db. Chaenomeles has none.

Back to Googling for シガナ. I add "plant" to my query. The first result is is [a Pokemon character named Zinnia](https://bulbapedia.bulbagarden.net/wiki/Zinnia). The page also contains a translation of "シガナ" into "Shigana" (and is actually "Aster" in the English version of the video game.) .

Now I'm frustrated because I'd gone down a different Pokemon rabbit hole earlier but I decided it was dumb and deleted my documentation on it.

Zinnia is already something we're looking at so now it feels like I'm on the right track. The seed vault doesn't have any entries for Shigana. [Aster](https://en.wikipedia.org/wiki/Aster_\(genus\)) is a genus though. Let's search that.

Ding! Aster has 30 results. Combine that with Zinnia's 13 for 43.

Nope, that's not it either. The prompt just asks how many, not how many varieties. The db tells us how many seeds of each sample are in the vault, so perhaps that's what we're looking for. Rather than do multiple searches and copy+paste lots of data, I'll search the tabulated data they provide directly. The spaces in my expressions are to ensure I only get results for that genus rather than results that may happen to contain that string.

```
sam@Neptune:/tmp$ cut -d$'\t' -f 6,8 sgsv_templates_20200613.tab | grep -e 'Aster ' -e 'Zinnia '
Zinnia violacea Cav.	500
Zinnia peruviana (L.) L. s.l.	500
Aster alpinus L.	500
Aster amellus L.	500
Zinnia peruviana (L.) L.s.l.	500
Zinnia peruviana (L.) L.s.l.	500
Zinnia haageana  	100
Aster alpinus L. 	500
Zinnia acerosa  	200
Aster tongolensis Franch.	500
Zinnia violacea Cav.	500
Zinnia haageana Regel	500
Zinnia violacea Cav. 	500
Zinnia acerosa  	500
Zinnia violacea	100
Aster amellus L.	500
Zinnia juniperifolia  	500
Zinnia citrea  	500
sam@Neptune:/tmp$
```

I could probably do some kind of Bash wizardry here but I copy+pasted this into Google Docs (which will take tabulated data without any action required) and ran a `SUM()` on the seed count column for a total of 7900. No joy there.

# Afterward
The CTF was fun but I was ultimately frustrated at how few points I was able to get. I'm excited to read others' writeups to see how close or far I was.



