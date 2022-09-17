---
title: "The Wrath of Cortex but without gaps"
date: 2022-09-16T22:30:44+0300
description: "Remember The Wrath of Cortex? We made the music looping gapless. In 2022. After all, why not?"
keywords: ["PS2","Crash Bandicoot","The Wrath of Cortex","hex editing","reverse engineering","music","OST","soundtrack","cortex-tools"]
url: "cortex_gapless.html"
---
It's 2001 and Traveler's Tales takes Naughty Dog's flagship franchise into their hands with the release of Crash Bandicoot: The Wrath of Cortex. IT'S AWESOME! Except for one little thing: ~~the weird models and animations~~ ~~the hideous loading times~~ ~~the not so high production values~~ ~~the inability to slide spin~~ ~~the fact the game could've been even better if Universal gave TT more than one year of dev time~~ the music has awkward silences between loops. Pretty weird since all of Crash's PS1 games have gapless looping music, and the first mainline game by a developer other than Naughty Dog doesn't. And it's a Traveler's Tales game to boot. What the heck went wrong here?

And it's not like the tracks have an excuse for that silence, since most of them end abruptly. It's just there in all the tracks. But here's the thing: it's there in all *except one track*. If you pay attention to the Medieval Madness music, you'll realize that it doesn't ever get quiet. It's the only track in the whole game that has gapless loops, and it's also a PS2 exclusive track for an unknown reason.

To investigate this whole mess, I downloaded the music files, ripped straight from the game itself and without any conversion from the JoshW website. All the files we care about are encoded in Sony's VAGp format (.vag file extension), used for the PS1, PS2 and PS3. Worth noting each music track includes two files, one for the left channel and one for the right, with the name of the track taking 7 characters while the 8<sup>th</sup> character represents the channel – for example, Arctic Antics has two files: arctical.vag and arcticar.vag. These files are all extracted from one file on the game disc called SFX.DAT. The benefit of having the music in the format used by the game is that the vgmstream plugin for foobar2000 can play them back, and it can be tossed into a hex editor for study.

Now, by hex editing the Medieval Madness tracks (gauntl2l.vag and gauntl2r.vag) and comparing it with any other track, we can see why MM is special. By setting the editor to display 16 bytes per row, the track for Arctic Antics has several rows of the 0C byte followed by 15 bytes of 00, right off the bat at address 00000040. Medieval Madness on the other hand doesn't have any 0C byte rows. It just gets into the music data immediately.

<ul style="display:flex;flex-wrap:wrap;justify-content:space-around;justify-content:space-evenly;padding:0">
<li style="display:block;text-align:center">
artical.vag hexdump
<pre><code>00000000  56 41 47 70 00 00 00 20  00 00 00 00 00 2f 2c 60  |VAGp... ...../,`|
00000010  00 00 ac 44 00 00 00 00  00 00 00 00 00 00 00 00  |...D............|
00000020  44 3a 5c 43 52 41 53 48  4d 7e 31 5c 41 49 46 53  |D:\CRASHM~1\AIFS|
00000030  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000040  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000050  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000060  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000070  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000080  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000090  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000000a0  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000000b0  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000000c0  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000000d0  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000000e0  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
000000f0  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000100  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000110  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000120  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000130  0c 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
...
002f2c60  14 00 2f b2 fd 50 ee 43  0f 16 1f df ec 10 42 35  |../..P.C......B5|
002f2c70  14 01 32 03 4b bf fc ff  d2 02 00 00 00 00 00 00  |..2.K...........|
002f2c80  14 07 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
002f2c90                                                                      </code></pre></li>
<li style="display:block;text-align:center">
gauntl2l.vag hexdump
<pre><code>00000000  56 41 47 70 00 00 00 20  00 00 00 00 00 2d d3 50  |VAGp... .....-.P|
00000010  00 00 ac 44 00 00 00 00  00 00 00 00 00 00 00 00  |...D............|
00000020  44 3a 5c 43 52 41 53 48  4d 7e 31 5c 41 49 46 53  |D:\CRASHM~1\AIFS|
00000030  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000040  32 00 a6 1d f4 ef 4f bf  fd 63 fe ef 10 d2 5d 90  |2.....O..c....].|
00000050  01 00 df 1e 00 33 d1 1d  44 13 f0 1f ef f0 ee 10  |.....3..D.......|
00000060  11 00 3e c6 da 41 13 ed  10 32 de 2e e5 fd ef 50  |..>..A...2.....P|
00000070  11 00 c6 0a 55 be 3f a0  4f 75 a0 fd 10 af fd f0  |....U.?.Ou......|
00000080  11 00 10 43 f0 f1 f0 00  32 bf 0e 34 02 be 40 c4  |...C....2..4..@.|
00000090  31 00 12 01 20 e4 4e 21  d2 3f e7 fb 52 03 3d ef  |1... .N!.?..R.=.|
000000a0  31 00 43 03 ef 72 02 10  42 a0 43 f1 ff 40 c4 fe  |1.C..r..B.C..@..|
000000b0  00 00 1d 13 10 d0 2f 14  df ed 11 ef 0f ff 00 dd  |....../.........|
000000c0  31 00 01 1e 0a f4 ff 02  2f 12 cf 21 e2 dd 12 bd  |1......./..!....|
000000d0  31 00 c3 4e ce 4f 1a f2  fb c2 dc 97 0a c0 2b 0c  |1..N.O........+.|
000000e0  11 00 e2 ec e1 00 21 13  ae 4b f4 3f ce 2e f3 4e  |......!..K.?...N|
000000f0  31 00 cd 2e f0 e2 1c 54  ce 32 0f 11 f0 33 bd 44  |1......T.2...3.D|
00000100  31 00 10 1f 10 24 e0 55  ef 5f e4 3d 20 1e 02 2e  |1....$.U._.= ...|
00000110  11 00 33 f2 df 40 d2 0c  34 22 af 5c 15 be 21 01  |..3..@..4".\..!.|
00000120  11 00 f1 e0 30 1f e1 21  01 ef 1f 34 11 bc 4f e5  |....0..!...4..O.|
00000130  31 00 61 d1 2e 44 cb 64  af 25 11 e3 5f c3 5e c3  |1.a..D.d.%.._.^.|
...
002dd350  31 00 df 1c c4 1b d2 0d  cf 02 dc f2 3a d4 fb 03  |1...........:...|
002dd360  31 01 1f be 32 cd d5 1f  1f 1f 1f 1f 1f 1f 1f 1f  |1...2...........|
002dd370  31 07 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |1...............|
002dd380                                                                      </code></pre></li>
</ul>

Further investigating both files reveal that the 0C bytes at the beginning of any track are the only instance where these blanks occur. So, one could assume that the presence of these bytes are why the other tracks don't loop seamlessly, but Medieval Madness does because of their absence.

The next assumption can be made by studying the VAGp format. The vgmstream library has code to decode and play it back, and [this code documents how the format handles looping proper](https://github.com/vgmstream/vgmstream/blob/0be524e84f8954d8eccc02613d49b032395220ce/src/meta/vag.c#L272). The lower half of the second byte of each 16-byte row is responsible for the looping and there's various flags controlling the playback of the tracks in-game. Heck, The Wrath of Cortex itself uses two of these flags: 1 and 7. By scrolling down the bottom of any track, you'll see a 01 byte in the penultimate row and a 07 byte in the final row. With the link to the documentation above, I had the power to use these flags to control how, when and where the song should loop.

Whether you delete the 0C rows or use the loop flags, the end result in foobar2000 is success. The silence between loops was no longer there. It was time to apply my discoveries to the game and this is when things got a little fun. However, deleting the 0C rows would be troublesome as it would shrink the size of the VAGp file, which would in turn shift the location of all the other files within SFX.DAT (and ultimately the rest of the files on the CD-ROM), which would no doubt cause glitches or prevent the game from booting, as all references to the location of those files would become incorrect.

Thankfully, SFX.DAT wasn't encrypted, so I was able to throw it in HxD, find where Arctic Antics is stored, and place the 6 flag to mark the beginning of the loop at the first row after the 0C bytes, and a 03 flag before the row with the 01 flag. Time to test the game on a real PS2 and PCSX2.

The track did play, but strangely enough, the track played some notes, skipped other notes, played some, skipped some, and so on. The music just wasn't coherent anymore, and to add salt to injury, the gap was still present. Both the emulator and the console just didn't play the track properly, and experimenting with other flags yielded the same disappointing result.

Not to be discouraged, I had another realization at that point. Since all tracks use the 1 and 7 flags at the end, I assumed that means the game knows what to do with these flags specifically. Perhaps if I moved the 0C rows to after these flags and therefore after the proper end of the music, the song files would have the exact same size in bytes, but in reality, these rows would no longer be part of the track. So, I did just that with Arctic Antics, and cut and paste the wall of 0Cs to the end. This approach also worked with vgmstream, but the gap was still there in emulation and on console. To verify that I wasn't going insane, I applied this trick to only one of the channels instead of both, and the song did begin early on that channel while the other channel started later – in other words, both channels were out of sync as expected. Either way, the silence persisted.

It was as if those flags didn't actually work at all. Probably TT had planned to do something with these flags but didn't have time to implement them in the game's music player.

There was only one last thing left to do, and that was deleting the offending rows. I actually did that in-place in the game's ISO (after converting from BIN/CUE), hoping that it would just work, except the gaps were still there. That's when I got the memo and figured out that there must be a table of contents defining where each file starts and ends.

Since I don't know how to reverse engineer game-specific files, Egor helped me figure out and research most of this stuff and actually wrote several Python scripts for the purpose of this one forgotten PS2 game. To keep things brief, he realized that SFX.DAT has a table of contents, which is split into three parts. The first part contained the file offsets relative to the start of SFX.DAT and the sizes of the files. The second part organized the files into a folder structure, and attached filenames, which are stored in the third part, to the files. A lot of theory crafting went into this process before the tools could be written to 1) extract all the files from SFX.DAT, and 2) use these same files to rebuild a perfect copy of the original SFX.DAT. With these tools at hand, we were ready to take on the game.

I manually had to remove the 0C silence lines for each track in the game, and then copy pasted the edited tracks to overwrite the extracted ones, and then run the rebuild script to get a smaller SFX.DAT archive. With the hex editor, I copied the new SFX.DAT's data in hex and overwrote the hexes of the original one in the ISO; there were leftover bytes from the old archive after the new one ended, but knowing that the size and position of the table of contents is specified within the file. we thought it should work.

The result is…

IT WORKS!

On both PCSX2 and PS2, the music loops from end to start without an awkward silence. For some tracks, the loop is so seamless, you have to really pay attention to tell the previous iteration from the new one. For others, it's a bit more obvious due to the drop in tone. But at that point, the main goal had been achieved, and no bugs or glitches occured when playing about a third of the game . It turns out the solution to the looping was simply to remove those silences, at least on the PS2. Had TT been given more time, the loop flags probably could've had an actual purpose instead of just… being there. I can't say for sure exactly what went wrong there, but it's finally fixed, in 2022, 21 years after the game was released.

Once the correct method of patching the game was established, the only thing left was to refine and improve the tools built for this process. We went from Egor's simple extraction and rebuilding tools combined with my manually edited tracks to an automated patcher that does it all straight to the game ISO itself. The NTSC-U Greatest Hits version is supported, but the patcher is written so that it even if it struggles with other builds, it's easy to patch it up.

For now, please enjoy the game's soundtrack with gapless music by using [cortex-tools](https://github.com/DankRank/cortex-tools). Crashes to Ashes, Atmospheric Pressure, Gold Rush, and more have never been this enjoyable.
