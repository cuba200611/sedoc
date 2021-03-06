Build Techniques
================

Signal and Power Routing, Bypass Caps
--------------------------------------

On an IC, all pins that source current must draw the current from the Vcc
pin; all pins that sink current must sink it to the GND pin. The route
between these two should be as short as possible.

For AC signals, return current through a ground plane will tend to follow
the route of the signal trace parallel to it on the other side of the
board. This effect is not huge at low frequences, such as 400 Hz, but by 1
MHz it's very tight, as shown in [this video][feranec] and the following
image. Thus, the ideal is ground and power planes underneath the signal
traces. Breaks in the ground plan will force the current around the break,
as seen in this [return current image](../sch/return-current.jpg).

Short of a proper ground plane, a quadrille mesh will also work well.
Contrawise, Vcc down one side and ground on the other makes for very long
return paths!

The "short return path" principle applied:
- On cables with multiple power and ground, do not cluster either at the
  ends. Distribute them evenly down the length of the connector.
- When using multiple solderless breadboards, jumper between GND (and maybe
  Vcc) buses at multiple points along the length.

Bypass caps:
- On every IC: 0.1 μF between power and ground, as close to the power and
  ground pins as possible.
- Every eight chips: 4.7 μF on array's power rails (PCB trace inductances).
- Also see [Capacitor Notes](../capacitor.md).

References:
- forum.6502.org, [Techniques for reliable high-speed digital circuits][f65
  2029]. In the thread, [this post][f65 80566] has simulation images of
  ground return paths at 1 Mhz.


Soldering
---------

- On protoboard especially, but also with edge connectors, use tape to mask
  off traces/holes that should remain solder-free. (Clean with IPA after.)
- [JRE] suggests 0.5 mm solder. I've been using 0.8 ok, but I'd
  definitely not want to go larger.
- Thoroughly tin tip, leaving lots of solder on it, before first use
  and before putting iron back in stand. Wipe when removing from
  stand. [jre-sol05]
- For conical tip, use edge in preferent to point (where room) for
  better heat transfer.
- Primary heat transfer is _through solder_. Use blob of fresh solder
  on tip to help heat transfer, but not to solder the work. Solder
  bonding the work should be melted on the work itself, not on the iron.
- Generally no need to remove solder from holes when re-soldering;
  just cut off device and remove pins. [jre-sol09]

Desoldering ([jre-sol10]):
- For removing solder alone (no part), clean tip and touch to solder;
  after it melts some will come away with the iron when you lift it off.
- Add solder first to help with heat transfer and remove oxidization.
- Raise rather than slide iron when done to help avoid lifiting pads.
  (Also never slide wick across a pad.)
- Clean solder sucker tip after every suck.
- Notch the tip (just one side) of a solder sucker to let it get
  closer to a PCB hole while still leaving space for the soldering
  iron to get in.
- For SMD discrete parts add solder to get big blobs on both ends so
  you can then touch/melt both at once with the iron. Put solder on
  iron tip for heat xfer before removing part.

Heat shrink can also be shrunk by rubbing the shank of the soldering
iron against it (not too slowly).

[jre]: https://josepheoff.github.io/posts/howtosolder-toc
[jre-sol05]: https://josepheoff.github.io/posts/howtosolder-5getstarted
[jre-sol09]: https://josepheoff.github.io/posts/howtosolder-9throughhole-remove
[jre-sol10]: https://josepheoff.github.io/posts/howtosolder-10soldersucker


### Soldering Iron

[Goot PX-201], 70 W temperature controlled. Tip base is ∅7mm × 37.

                     R      Style
    ●PX-2RT-SB      0.3     conical
    ●PX-2RT-B       0.5     conical             standard tip/w PX-201
    ●PX-2RT-BC      1.0     45° conic section
     PX-2RT-2C
     PX-2RT-3C
     PX-2RT-4C
     PX-2RT-5C
     PX-2RT-1.6D
    ●PX-2RT-2.4D    2.4     chisel
     PX-2RT-3.2D
     PX-2RT-4D
     PX-2RT-5D
     PX-2RT-5K

The above came off a shop inventory listing; there are more on the
[Goot site][goot px-201].

Other suggestions I've seen:
- Hakko FX-888D is very standard.
- [KSGER T12]: Hakko-compatible tips.
- Anesty ZD-915 vacuum desoldering unit; cheap and works well.

[goot px-201]: http://www.goot.jp/en/handakote/px-201/
[KSGER T12]: https://www.amazon.com/dp/B07PMZGPQQ


SMD
---

Smaller parts (<`0805`) are not marked. Keep only one value at a time
open on the workbench, and put all parts away before opening up
another value part.


Sockets
-------

DIP sockets come in two types: machine-tooled with gold flashed
contacts (more reliable long term) and cheaper tin wiper contacts
which are more suited to frequent removal and reinstallation. For
wiper, always use double-wipe, where there are contacts on both sides
of the IC pin.


DIP I-leads (Butts) on Multi-layer PCBs
---------------------------------------

As [described on forums.6502.org][gw-ilead] by Garth Wilson, you can
make PCB routing easier by not using through-hole for certain parts
but instead trimming the DIP leads just below the shank and soldering
them directly to longish pads (of normal width) that are on only one
side of the board. (This takes more solder than a normal surface mount
because you need a sizable fillet.) You can also try bending the DIP
leads inward to make them J-leads. This leaves more space on the other
side of the board (and actually all other layers) for routing traces.

[gw-ilead]: http://forum.6502.org/viewtopic.php?f=12&t=5923&start=45#p73277


Crimping "Dupont" and Similar Connectors
----------------------------------------

Terminology:
- A _wire_ consists of a _conductor_ wrapped in _insulation_.
- _Connectors_ may be male or female, and have two sets of metal tabs, the
  _insulation tabs_ at the end and the _conductor tabs_ in the middle.
  (Tabs may also be called _barrels_.)
- The connectors are inserted into a _shroud_, which for dupont-style
  accepts both male and female connectors.

Use a proper crimping tool. For dupont it should crimp the wire tabs
with "curl-in" but the insulation tabs with "wrap-around." I use a
Preciva PR-3254 with three blocks:
1. (outside) XH and C3-R 2.54 mm, JST
2. (middle) Molex IDE power, ATX power, VH3.96
3. (handle) dupont 2.54 male/female

When crimping, press the jaws together _very_ hard; it's easy to use too
little force and get a loose connection, even if all else is done
correctly.

The insulated part _must_ be fully inserted to at least the inside end of
the insulation grip tabs on the connector. It's can work if it extends very
slightly into conductor crimp tabs, but this should be avoided if possible.
Give the crimped connection a good strong tug before inserting it into the
housing.
- The amount of conductor you need to strip is less than you might
  think; only about 3 mm or so. 5 mm is too long and will interfere
  with pin insertion for female connectors.
- However, you can strip twice the length and fold the bare conductor back
  at the half-way point, if you need more thickness. This will only work
  with very thin conductor; normal 24 gauge will be too thick if you do
  this.
- Ensure the connector lines up the insulation ("wrap") tabs and conductor
  ("curl in") tabs with the proper spots on the crimper. It may be
  easier to insert the tabs into the slot, even if this means turning
  the tool around until you can use that direction, but that makes it
  harder to align the gap between the two sets of tabs to the centre
  of the die.
- Sometimes closing the crimper more than one notch will not allow the
  insulation to get into the insulation tabs. Other times it's ok. I
  sometimes use two.
- To get correct alignment: insert the connector until the conductor tabs are
  almost at the centre ledge between the two sets of tabs. There should be
  more space between the centre ledge and the insulation tabs than between
  the centre ledge and the conductor tabs. Then when inserting the wire from
  the other side, use the ledge at the top of the jaws (opposite the side on
  which the connector rests as the "feel stop" for the insulation. This
  should bring the insulation almost but not quite to the conductor tabs,
  ensuring that it's fully held by the insulation tabs.
- When doing the crimp there's no need for the conductor or insulation to be
  resting on the connector; even resting against the top edge of the jaws,
  as far away from the connector as it can get, it will still be crimped
  properly.

#### Double Crimps

It can be useful to crimp two wires into a single connector to duplicate a
ground.
1. Crimp a short wire in the normal way and cut it down to 3 cm from the
   back of the crimp pin.
2. Strip the other end of the short wire to 2-3× normal bare length; none
   of the insulation, only the conductor should enter the crimp connector.
   (This wire should need little strain relief; don't pull it!)
3. Strip the longer wire, the signal you're duplicating, as normal.
4. Place longer wire on top of shorter wire, with the ends lining up, and
   crimp so that only the long wire's insulation goes in the insulation
   tabs; the short wire will have conductor in both conductor and
   insulation tabs..
5. Get the longer wire's pin in the shroud first; it may require a bit of
   squeezing due to the double-insulation at the entry to the shroud.
   (Consider stripping bare conductor to outside of shroud?)
6. Loop the shorter wire's pin into its shroud slot.

#### Colors

Colors follow, as much as possible, the electronics color code, with
substitutions where there aren't enough wire colors.

    0   black               (arrow/triangle on connector)
    1   brown, white        (draw black stripe on white with marker)
    2   red
    3   orange, white
    4   yellow
    5   green
    6   blue
    7   violet, white       (tried orange, but found it confusing)

References:
- Matt Millman, [Common Wire-to-board, Wire-to-wire Connectors, and Crimp
  Tools][millman]. Fantastic guide with many, many details.
- Gogo:Tronics, [Crimping Electronics Connectors (Dupont, PH, XH, VH,
  KF2510)][gogo].


Mechanical Connections
----------------------

- [Superglue (cyanoacrylate) and baking soda][cabs] creates a very
  sturdy filler compound especially good for filling porous materials.
  Use baking soda to fill the gap and then drop on the superglue. The
  reaction is exothermic and produces noxious vapors.



<!-------------------------------------------------------------------->
[cabs]: https://en.wikipedia.org/wiki/Cyanoacrylate#Filler
[f65 2029]: http://forum.6502.org/viewtopic.php?f=4&t=2029
[f65 80566]: http://forum.6502.org/viewtopic.php?f=4&t=2029&p=80566#p80566
[feranec]: https://youtu.be/4nEd1jTTIUQ?t=631
[gogo]: https://sparks.gogo.co.nz/crimping/
[millman]: http://tech.mattmillman.com/info/crimpconnectors/
