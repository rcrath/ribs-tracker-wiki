# ribs, a ganular synth/effect

Ribs is a 32 voice midi-triggered granular effect.  It can act as both a really interesting synth and as a midi controlled audio mangler.  

This document is a rough guide put together from viewing the extensive [tutorial video](https://www.youtube.com/watch?v=1iFMZsLiZOo&feature=youtu.be), looting the useful bits from the [KVR forum](https://www.kvraudio.com/forum/viewtopic.php?f=1&t=486995), and trial and error while trying out what it says to do there!  it isn't complete yet, and questions should be directed to the forum.  Ribs can be downloaded at the kvr forum as well.  

__questions for dev are in \_\_\<strong\>\_\_: Just look for the emphasis on the web page or search "\_\_"__ in a text editor.

Ribs is made by Eugene Yakshin.  You can listen to his music and hear some of the sounds Ribs is capable of at https://soundcloud.com/crimsonbrain, or contribute to his financial well-being at https://www.patreon.com/eugeneyakshin and https://www.paypal.me/EugeneYakshin. 

This manual is written by Rich Rath.  Check out his music at http://rreplay.com and http://way.net/waymusic. Also on soundcloud, bandcamp, itunes, amazon, and the usual suspects, under either his name or the band rreplay.  
<br />
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.
## Ultrahypershort Tutorial:

1. Put Ribs on an audio track as an effect
2. Add or route some audio to the track
3. Send MIDI notes to the track
4. As the notes are being sent to Ribs, the buffers start filling with the audio and a sound will appear


## Basics

* Question mark button next to Logo will tell what everything does
* On Youtube there is an excellent tutorial that includes time codes in the description to navigate it. Click on image for video.
[![Tutorial video](http://img.youtube.com/vi/1iFMZsLiZOo/0.jpg)](http://www.youtube.com/watch?v=1iFMZsLiZOo "Tutorial video")

### key terms

* buffer:  Each voice has its own buffer that is the length of the global buffer setting (the big reel).
* voice: in the MIDI sense.  each voice triggers playback of its corresponding part of the corresponding buffer.  the rules for correspondence are below.  but voices are the midi element, as it usually relates to polyphony.  See polyphony section below.
* part: An individual buffer under its corresponding tab, including its selection and all other settings.  triggered by voices, so sometimes it seems like the two are the same but the part is the audio selected in the buffer and all its settings that determine playback and recording of the slection. 

### Visual logic

* Actionable stuff is between square brackets and brighter (white instead of gray).  If it is not a knob, an actionable item whill usually have a set of square brackets around it.  The exception to the brackets is the base length for beat mode and the set of fractions in the beat mode note picker.  The latter has the whole section by a big bracket.  
* Labels are in gray.

### mouse behavior

* actionable items appear the same but do different things.  The might light up when you click them in an on/off toggle.  They might change to something else (an on A/on B toggle) or they might produce a dropdown menu. __(is there a way to make this consistent visually?)__  
* Any number can generally be scrolled up or down.  

Selecting a buffer: the buffer is selected via the tabs at the top of the buffer window. 

	* hover over tab to view its buffer without switching to it, 
	* click tab once to select buffer, 
	* click tab twice to make next voice trigger the buffer no matter what, this second click lights buffer red.
		* clicking another buffer tab when current buffer is red transfers the "next voice" function to the newly selected buffer.
		* when capturing next voice, the length parameters at the bottom left are temporarily ignored. 
		* clicking a red buffer tab again cancels the function. 

Selection a part *in* a buffer: a selection of a subset of the buffer called a part, that marks start and endpoints within which playback and recording take place.  can be set differently for each buffer.  

* Set current buffer selection by context-click (i.e. right click) and drag, or shift-left-click and drag. If "select all" button is pressed lit of "select this", then the selection will apply to all buffers.
* left clicking in the buffer moves playhead toward clicked position. how fast it gets there is determined by glide parameter ("ph g"). The behavior will vary depending on mode (simple, beat, or note) and the grain settings.  See video for details at [08:24 Playhead](https://youtu.be/1iFMZsLiZOo?t=506).  
* clicking in a buffer then vertically scrolling zooms in and out.
* Ctrl scroll: vertical (amplitude) zoom in and out
* shift-scroll: horizontal scroll 

Step Length and Playrate controls:

*Shift+wheel while these are selected will multiply or divide by factors of 2. 


## Performance modes

* Simple just plays buffer
* Notes: cuts up buffer to make notes.  follows midi notes
* Beat: syncs grains to time and has other functions

### buffer

* reel icon on left below performance nodes sets global length of buffers.  Holds about 10 seconds of audio at full length. Changing this length has a big effect on the sound.

#### refilling buffers: 

* refill all, refill this: refills all buffers or only currently selected one. If "refill this" is called for a voice that does not currently have focus, it will show "refill pending" until the voice gains focus.
* own/common: 
	* common: all buffers read from the selected buffer instead of their own content, without losing their own contents. You can switch back to "own" without losing whatever was in "own" before.  
		* __still can't quite figure this out. Help message says "doesn't erase own buffers, only switches other voices to the picked buffer."__
	* own: buffers read themselves.  different content can be recorded into different buffers. refills begin only once voice is activated.  If you switch to common and come back, the buffers will be remembered.  	
* dropdown menu (these seem to be global, affecting all buffers).  __Would be nice if buffers could set this per buffer instead of globally.__
	* notes mode: new midi note triggers refill 
	* !glide mode (not glide).  Same as notes mode, but glides and legato notes do not retrigger. 
	* hold: holds contents of buffer without refilling once filled
	* tempo: adds numerator/denom. control to the buffer window, which can be set to control the resetting of the host buffer in time with the DAW tempo. Units are are quarter notes. 1/1 will refill the current buffer every quarter note.  
	
#### buffer windows

* tabs at top of buffer are for viewing and selecting buffers. the buffers light up a little when a voice activates them
* in the buffer window, the thick gray line is the grain start, while the play head is the thin white line.
* "picked buffer" is a midi parameter allowing setting a buffer by midi cc. 
* __As far as I can tell so far,__ when you set poly/mono voices to one, only the first buffer tab gets used
* in poly mode, There is one buffer per voice.  , set it to four and the first buffer tab gets used if you play a single note, the first two if you do a doublestop and the first four if you play a four note chord. and so on.  Not sure how you would use 32 buffers.  Reducing the number of buffers brings down cpu usage a lot it seems, at least in poly mode. If you click a buffer tab twice, the next midi note will go to the clicked buffer no matter what.  
* mono mode stays on the currently selected buffer __I think__  
* If you click a new tab twice, it will make the next incoming midi note trigger that buffer no matter what.
* When set to "own", you can enter different audio in each buffer, and set the selection lengths within each buffer differently, but the overall buffer length (set with the "reel" knob at the top left) is global.  

For how to select buffers and parts, see "Mouse behavior", above.

### bottom of buffer window:

* leftmost number is selection start
* middle number is playhead position. 
* rightmost number is selection endpoint
*  "select all" applies current buffer selection to all buffers (*not, as I first thought, selecting all of the current buffer*). "Select this" limits selection to current buffer.
* "clear selection" is the "select all of current buffer" switch.  When. When "select all" button is pressed, "clear selection" clears all the selections. 

### Polyphony/mono.  

Number of voices active at once.  

* mono is a single voice active at a time, and the buffer is selected with the mouse or via midi __I think__. clicking a buffer twice forces that buffer to be triggered next no matter what __(does this override the voices setting?)__
* Poly associates the number of voices with the number of buffers to fill and playback.   For example, poly set to "4" results in the first note triggering buffer one.  A subsesquent note will trigger one again.  playing a second note before then end of the first note will trigger buffer two, and so on.  The modes in the buffer refill dropdown menu will help determine the specific behavior of recording and an playback of the buffer, along with the controls to the right of the buffer.

### playback and retrigger buttons 

to the right of the bottom of the buffer are two buttons. 

* The button that switches between roughly an "M" and a "W" determines whether the buffer starts over when it reaches the end or reverses and goes back through in reverse.
* When the button that looks like a tape play head with a left arrow is lit, it retriggers the buffer from its beginning with each new midi note that comes into that buffer (as determined by the "voices/poly/mono" section). 

## Grains

Knobs on right control grain parameters. These seem to be global, for all buffers.

### grain size and behavior

* step length: controls length of grain.  __(most granulatoras call this grain length)__ Use in note mode.  When set at zero, the note always comes from the same spot in the buffer.  When *not* equal to zero, it advances the buffer position by the number of samples on the dial __(at what rate? i.e., how often does it advance b n samples? my guess is at sample rate)__, with a range of -128 to +128.  Low positive or negative settings give some motion and life.  Higher settings cause the note to gradually fall apart and become noisy.  Does not have any effect in modes other than note.  Shift+wheel while this is focused will multiply or divide by factors of 2. 
* repeat:  repeats the step between 0 and 512 times before advancing.  In note mode, this can calm down and reintroduce steady pitch when the step length is set high.  As the repeats increase, the become a rapid trilling, and at higher settings, they create periodic jumps in the grain that keep the pitch but change the tone suddenly.  Does not seem to have any effect in other modes. balancing step length and repeat gives interesting variations.  
* The three "oc" buttons are multipliers that change the rate __(is that correct?)__ at which the grains advance.  
	* the top one, ocμ: multiply rate by number of repeats
	* the middle one, ocλ: multiply rate by grain length. Used alone, this will cause the grain to move through the buffer at the same rate as the original recording.
	* the bottom one, ocΔ: multiply the rate by the playrate (the big dial below the buffer)
* to the bottom left of the buffer is the grain length ratio, which changes the grain length by multiplying it by the ratio you set. Both numerator and denominator can be set, resulting in very long or very short grain lengths.  This seems to have a lot of overlap with the step length control, but handles differently musically in that the shifts are ratios rather than absolute lengths.

### noise/error
Below the step/rep controls are the error controls, "e int" and "e ext" which introduce error or noise. The "nebula" button is the third in that row.

* e int: introduces error into the playhead position (the thin white vertical line)
* e ext: introduces error into the grain start position (the thicker gray vertical band)
* Nebula button: when on (the nebula is concentrated looking) the error is calculated against the correct position.  When off (the nebula looks distributed), the error is calculated against the previous (erroneos) position, leading the error to propagate so that time coherence gets lost. This seemed to me to have the most clearly audible effect in simple mode. __(the icon does not really relate clearly to what is going on and is thus hard to remember.  Maybe just "fb" for feedback and have it light up when on and dim when error is calculated against correct position as in feedback is off)__  
* Above the buffer area is a long pbar that shows gap probability, that is the likelihood of a grain being skipped.  Dragging the mouse up or down increases and decreases the gap probability.  
* To the right is a stereo (Ps) mono (Pm) button which determines whether the gaps are mono or stereo __(I am guessing this means that the gaps are linked in mono mode and independent in stereo mode)__.  I really like this at low probabilities and in stereo for bringing the sound to life a little.  
* the waveform button to the right of the stereo switch fills the gaps with the incoming audio instead of leaving them silent, which can be a different way of mixing in the origianl audio signal.

### Windowing and envelope functions

* Just above the RIBS logo on the right is a dropdown menu __(I did not realize this was a dropdown menu until watching the video a couple of times.  I thought it was a label. I see now in the visual logic that labels are gray and dropdown or actionable stuff is white)__ The default of this dropdown menu is "parab" and the other two options are "linear" and "sine." __(maybe simple images of the envelope shapes would be more useful than the terms here)__  These determine the shape of the attack and release on each grain. The envelopes are simple and symmetrical with attack and release times that are always equal to each other.  Grain length acts as a sort of sustain time too. __(correct?, meaning that there is a flat horizontal section to any grain where the grain is longer than the onset and release time?)__ No effect in simple mose because no grains.
* The knob above the dropdown menu adjusts the envelope onset and release rate.  Low settings will produce rapid onset and release, leaving the grains discrete.  As it is increased, the onset and release grow longer.  The attack and release are anchored to their center points rather than the start of attack and end of release, so longer settings extend the grain earlier and later, causing overlap with other grains and a nice smearing effect that also reduces the volume in the cases I have used it in.  __(have not gotten to it yet, but maybe the autogain setting can compensate for this)__ The envelope time has no effect in simple mode since there are no grains.  The envelope settings are most noticeable in notes mode.  


## Playrate

* Shift+wheel while this is focused will multiply or divide by factors of 2.
* In simple mode adjusting this "scratches" the sound. multiplying or dividing by factors of two willmove the buffer up and down by octaves.  
* In granular modes, playrate affects the speed inside the grain.  

There are five controls to the right of playrate

* Wobble: two top two boxes next to playrate are an LFO of the playrate, with amount/depth in the left control and speed in the right (drag the mouse up for more, down for less, flat for off).  
* the two buttons below that set the wobble to affect the internal speed (i.e. playrate) of the grain ("int") and/or the size of the grain ("ext").
* sync/async matches the LFO to the host tempo (or not).


## Troubleshooting

### screeching and buzzing may appear in any of these cases: 

* turn up the "err" knobs to the right of the waveform tabs,
* select a very short section of the sound (by right click and drag on the waveform)
* set the playback speed close to zero or to max
* turn up agc params too high (on the bottom left)
* play very high MIDI notes
* use very big denumerators in base len / beat ratios (in the middle left)
* turn up the gap probability (the grey stripe on top)
* turn filter resonance to max

### the playrate could also be changed independent of pitch? Possible?

Yes and no. Often changing the playrate without changing the pitch is also done with a variation of granular approach. You could do some sort of it in NOTES mode, if instead of using the "playrate" control you use "step" on the top right. Maybe increase the "grain overlap" too, so it would sound softer.


### trying to get this working in Ableton Live 8.

I've put Ribs on a midi channel which receives midi from a midi keyboard (m-audio oxygen 49) and I've put some pad samples on an audio track wich is routed to send audio to the track Ribs is on.

The thing is, when I click on the drop down menu for the inputs for Ribs, it only shows 3 options. 

* Input 3/input 4
* Input 5/input 6
* Input 7/input 8

And when I play the pad while playing a midi note into Ribs, Ribs' refill thing starts 'playing' but no waveform appears and no audio comes out.

works fine in Ableton Live here... No unusual setup, just 1 audio + 1 midi track and
you're good to go.

Here's another setup ![screen cap](http://i1105.photobucket.com/albums/h344/pekbro/Misc/RIBSLIVE.jpg) from Live, maybe it will help to see whats going on.  Synth on the far right is the audio source, midi track is the midi source, both directed at the audio track containing RIBS.

*zusco_ wrote:*

### anyone know how to set this up in fl? (fl 12)

*user Top responded:*

In Fl Studio, you just need to place ribs on a track as an effect, then route
an audio source and a midi source to it. 

* For instance, you could route the kick drum from the basic template to some track.
* Load up a step sequencer (or other midi source), and set its midi "output"
to port 2 on the plugin's settings page.
* Put ribs in the effect rack of some track, then set its midi "input" to port 2.
   *The midi port doesn't matter, so long as the "output" for the sequencer and the "input" for ribs match up.
* Set up a midi pattern to play the kick, then route the audio from the kick track, to the track with ribs on it. Program a pattern for the step sequencer, and press play. Thats it...

The audio from the kick and the midi from the step sequencer will drive 
the ribs plugin. 

Its actually really simple, even if my instructions here seem confusing.
*This isn't the only way to do it btw, just a quick example...*

