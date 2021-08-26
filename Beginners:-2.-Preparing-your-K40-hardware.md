Once you have physically unpacked your K40, it may be tempting to plug it straight into the mains and get lasering - but for a raft of reasons this is not advisable.

As you can see from the list below, the K40 does not exactly arrive ready for immediate use - and the level of technical skill needed to get it properly ready is non-trivial.

Once you have read and digested this page, the next step in the Beginners Tutorial is to [install MeerK40t](./Beginners:-2.-Installing-MeerK40t).

If you want to return to the [Beginners Index](./Beginners:-0.-Index) click [here](./Beginners:-0.-Index).

## Contents
* [Safety matters](#safety-matters)
* [End stops](#end-stops)
* [Burn quality](#burn-quality)
* [Enhancements](#enhancements)
* [Charring](#charring)

## Safety matters
Hopefully you will have already read the [Beginners Wiki page on Safety matters](./Beginners:-1.-Safety-matters) in order to get a basic understanding of the major areas of potential danger to your health when you use your K40.

This section of this Wiki page is designed to give you information on how to check and / or modify your K40 hardware in order to mitigate these risks and so you can use your K40 safely.

The build quality of K40s can be extremely variable, and some of the common issues with a newly unpacked K40 can be dangerous or even deadly.

We cannot provide the answers to all the issues here - but we do want to give you at least a heads up on the things you need to do to stop your K40 from electrocuting you, burning out your eyes, damaging itself or just disappointing you with very poor results.

1. [Electrical Safety](#electrical-safety)
2. [Fumes](#fumes)
3. [Cooling](#cooling)
4. [Protecting your eyes](#protecting-your-eyes)
5. [Lid laser cut-off](#lid-laser-cut-off)
6. [Risk of fire](#risk-of-fire)

### Electrical Safety
#### Laser voltages
The CO2 lasers operate on **EXTREMELY** high voltages - voltages that are quite high enough to stop your heart - and the capacitors in the K40 power supply can retain a charge for quite a considerable time after you turn it off / disconnect it from the mains. ***Please exercise a great deal of caution when you are approaching the high voltage parts of the electronics.*** Leave the capacitors to discharge for several hours after you have disconnected your machine from the mains.

#### Wiring connections
People report that the wiring inside the K40 can be poorly connected - I certainly found several wires not properly tightened when I received mine. So before plugging it in, follow all the wires and ensure that all the terminals are securely tightened.

Don't forget also to check the wiring inside the fan unit for proper connections.

#### Earthing
It is **essential** that your K40 is earthed properly - both to avoid accidental electrocution if the metal chassis accidentally becomes connected to the mains or laser voltages, but also because the lifetime of your laser can be shortened considerably if it is not earthed properly.

If you are using the K40 in a country where every plug has a properly grounded earth pin, that is usually quite sufficient as a basis - but if not (and if you have any doubts, get an electrician to check it) then you need to install an earth spike close to your K40 and connect it to the earth connection on the rear.

But most importantly you should check that the earth connection on the rear has not been insulated from the chassis by paint - unscrew it, scrape off the paint around the hole and screw it back on again. Check with an ohmmeter that the resistance between the chassis and the mains earth pin is 0.0 ohms.

#### High voltage connections
Please check the high voltage wires to the laser tube carefully - ours were fine, but according to the internet many people have reported poor quality connections.

### Fumes
The toxicity of the fumes from your K40 will depend on the material you are lasering, but some materials can be really quite toxic, whilst others are just unpleasant. There are few, if any, materials you can laser completely safely and pleasantly without having a means of extracting the fumes to where they can dissipate without you breathing them in.

The supplied extractor fan is generally considered to be inadequate - and because it is fitted to the back of the laser, it creates positive pressure in the exhaust pipe to the outside and any leaks in that pipe will result in fumes entering your workroom. We added a second in-line fan at the far end of the exhaust tube to create negative pressure in the tube and prevent this, and used window/door gap insulating foam tape to fill the largeish gap between the standard fan and the case.

The supplied plastic fume vent is generally considered to be utterly useless. Considering an aluminium expanding one is only a few euros/dollars, it is not expensive to use something better.

The metal duct which goes from the back panel through to the bed area is generally considered to be less than optimal. It extends past the back edge of the gantry frame and can prevent you inserting materials which are the full depth of the gantry (even though the rear most part of this area is not reachable by the laser. Some people recommend removing this duct altogether in order to improve air-flow, but this the improved volume of air moved can be offset by air travelling around the sides of the gantry rather than across the laser bed. An alternative is to cut this back so that it does not protrude past the back edge of the gantry.

### Cooling
Your K40 laser tube is water cooled and water flow is **essential** to prevent failure due to overheating. Most K40s come with water pipes and some form of water pump, but don't come with any water tank or means of cooling. To avoid significantly shortening the lifespan of your laser tube, it is important that you keep your laser tube below 25°C during use (generally recommended between 15°C and 20°C), so whilst this section is not really a safety related item, creating a water reservoir and deciding how you will keep the water within the above temperature range is an essential part of commissioning your K40. 

For a reservoir, I used a 50 litre plastic storage box like this:

![image](https://user-images.githubusercontent.com/3001893/127775469-04944050-076a-4864-a1d8-904f013b60e9.png)

I drilled two holes in one end of the lid for pipes to go through, with rubber washers on either side to form some sort of seal, and attached the pump to the inlet pipe and fitted heavy galvanised washers to the other (held on by more rubber washers) to weight the outlet and keep it under water and prevent air returning up the pipe when the pump was off.

It is also essential that the water you use is non-conducting. The easiest way to achieve this is to use deionised water, (we get ours in 5 litre bottles from our local LIDL store at c. 0.70€ per bottle), but if your tap water is sufficiently soft or softened (and you may need to have the conductivity tested) that may also be suitable. You may need to add a non-conducting anti-algae chemical to the water.

If you are located in a climate where you may find it difficult to keep the water temperature below 20°C-25°C, then you will need some form of cooling. For cooling we use ice packs from our domestic freezer - but many people invest in technology to cool their water. Here is a non-exhaustive list of possible solutions:

* Add frozen water bottles to the bucket.
* If your tap water is sufficiently soft and pure, run tap water into the bucket with an overflow to drain.
* Re-purpose an office water-bottle style cooler.
* Re-purpose a miniature refrigerator.
* Use Peltier/electrothermal panels.
* Use a computer CPU water-cooling radiator
* Use a CW-3000 or CW-5200 “water chiller”.
* Use an aquarium water chiller.
* Scavenge a used window air conditioner.

Because water expands when it freezes, if you live in a colder climate you also need to ensure that the water in your laser tube doesn't freeze or it will crack the glass. You can add non-conducting anti-freeze to the water reservoir, or drain the water out of your tube when there is any risk of freezing.

Having a means of measuring the water temperature is useful to check that it is not going over 25°C. Our K40 came with a digital temperature display on the main panel (using a thermistor taped to the **outside** of the cooling water exit tube just outside the business end of the laser). 

I am also not quite sure whether it is better to measure the output temperature (giving an indication of how hot it was in the laser tube) or the input temperature (which shows whether your cooling water is already too hot to be able to cool the laser enough), but in the end decided that output temperature was the one to watch. I was also unsure just how accurate the temperature measurement is when the thermistor is simply taped to the **outside** of the pipe - personally I doubt that this is really much good at measuring the output water temperature without being adversely affected by the ambient air temperature. 

For some months I decided that an external thermistor taped to the outlet pipe was sufficient, however I subsequently decided to 3D print [this inline thermistor housing](https://www.thingiverse.com/thing:4565279), and found that this gave a significantly more accurate reading. 

![image](https://user-images.githubusercontent.com/3001893/127776127-7d0c0c64-3fc9-4cb6-96b3-4c5d60a6889b.png)

Another alternative I considered was a cheap aquarium thermometer with a sucker to stick it to the side of my water tank wall in order to have a better indicator of the reservoir temperature.

Whilst it is unlikely that the pump will fail, some people route the tubing through a mechanical and/or electronic flow meter (which can also have a temperature gauge integrated).

![image](https://user-images.githubusercontent.com/3001893/120121463-81667800-c19b-11eb-88e9-d4f3133e334e.png)

Finally, once you have your cooling water system working, you will need to tip your machine to get the air bubbles out of the laser tube. I also found that rotating the tube (before mirror alignment) so that the water pipes are entering / leaving the laser at the top also helped to avoid bubbles.

### Protecting your eyes
Your K40 should have come with a red-tinted acrylic window in the lid. The acrylic stops the infra-red laser light, and the red-tinting is designed to filter out the other dangerous light frequencies (like ultra-violet) that the laser tube produces in normal (non-laser) radiant light. But this panel is not as dark as e.g. an arc-welding helmet, and is not sufficient to dim the bright light created when the laser burns some materials.

Depending on the strength of your burn and the material you are lasering, your burn will create a **bright** spot on the work. Even with the lid down and the "protective" window in the way, this bright spot can **still** damage your eyes.

This is perhaps the aspect of your K40 that is potentially most hazardous to your health - and the easiest to get careless with when you use your K40 regularly. If you are not sure just how dangerous this can be, please watch [this video](https://www.youtube.com/watch?v=-wXApAAh8xA).

So please, please, ensure that you keep your K40 lid down when burning, and get some laser safety specs (suitable for CO2 lasers) and wear them when you are looking through the window.

### Lid laser cut-off
Some people add a protecting microswitch to the laser compartment lid, so that the laser is cut-off if you lift the lid, and that this is particularly pertinent if you have inquisitive children. Clearly if you cut off the laser mid-burn you may not be able to restart it at the appropriate point, and may instead have to start again from the beginning with new material. Personally I (@Sophist-UK) prefer not to have this switch for several reasons: 
* I don't have children who might lift the lid
* I often prop the lid slightly open so that the horizontal gap at the bottom assists with laminar airflow across the bed
* Taking care for my eyesight, I often wish to lift the lid to see how the burn is progressing and check the quality
* If I am trying to align material and e.g. want to make a short low-power burst burn with the test switch, it will be both inconvenient and more difficult to see where the laser hits if I have to lower the lid each time.

However, I try to be **very** careful and so far have not had any safety issues. For your own K40, the choice is yours to make.

### Risk of fire
The minimum precautions to take for fire are:

* Stay with your K40 to monitor it for fire when it is working; and
* Keep a CO2 fire extinguisher close to hand

Some people also fit some sort of fire suppression inside their K40. **To do:** Research and links needed here.

## End stops
This is not really a safety issue, but a logical place to mention it is before the section on gantry alignment.

Your K40 (and software like Meerk40t) needs to establish a known position of the head so that it can keep track as it moves and not try to move too far, hit the hard limits of the guide rails and start making a loud grinding noise and / or a bang. (It is likely that this will happen to you at some point, and it sounds very worrying when it does. However, in most cases no permanent harm is caused, your k40 just losses position and future burns area made in the wrong place.) 

It establishes a position by "homing" - by which I mean it moves to one corner of the bed (for most K40s the top-left corner) where it has x & y end-stop sensors to tell it where to stop. These are often (but not always) optical sensors consisting of a u-shaped plastic electronic sensor in a fixed position at the end of the axis and a metal strip to break the light beam.

It is not uncommon for your k40 to arrive with the metal step out of position, so that it doesn't interior the light beam. If that is the case (as it was with my own K40) then the very first time you power it on, you panic and turn off the machine in a hurry when you hear this loud grinding noise.

To save yourself a fright, before you post on for the first time, check that the metal strip is not bent or of position and if it is adjust it.

## Burn quality
The light as it exits from your laser is a very narrow and very, very straight beam of invisible light. It is reflected off 3 mirrors and goes through a lens to focus it before it hits the material in a tiny spot of concentrated light. There are quite a few things that can cause this to go awry:

1. [Mirror/lens cleanliness](#mirrorlens-cleanliness)
2. [Belt tension](#belt-tension)
3. [Gantry & Bed Alignment](#gantry--bed-alignment)
4. [Laser and Mirror alignment](#laser-and-mirror-alignment)
5. [Lens orientation](#lens-orientation)
6. [Focus](#focus)
7. [Smoke](#smoke)
8. [Burn charring](#burn-charring)
9. [Reflection charring](#reflection-charring)

### Mirror/lens cleanliness
When your K40 arrives, it is quite likely that your mirrors and lenses will be quite dirty - certainly mine were absolutely filthy when it arrived. If you fire up your laser whilst these have dirt on them, the dirt will absorb laser energy where it hits the mirror / lens and can cause burnt spots at a minimum and possibly crack your mirror completely. 

So before you align thm, you should clean the three mirrors and the lens with rubbing alcohol or acetone. If you are doing this before aligning mirrors, you can unscrew the retaining rings and take the mirrors out to clean them fully. After alignment use a cotton bud to clean them.

The smoke from your burns can also make both mirrors and lenses become dirty again quite quickly - cleaning them needs to be a fairly regular activity, though this can be reduced by installing smoke/air assist.

### Belt tension
The laser head moves in both X- and Y-directions - and there are two stepper motors and 3 belts to achieve this.

For positional accuracy and to prevent the belts slipping on the stepper-motor cogs, the belts needs to be reasonable tight. Certainly when I (@Sophist-UK) received my K40, the belts were way too loose and needed tightening. In theory this can be done with a reasonably long Philips screwdriver, but if you have any difficulties, you may need to remove the gantry entirely from the case to achieve this. For this reason, tightening these belts should be done **before** laser alignment as if you have already aligned your laser, you will need to redo it after removing and replacing the gantry because it is highly unlikely that it will be in **exactly** the same position when you have replaced it.

The three belts and their adjustment access are as follows:
* Y-belt 1: On the left side of the gantry. Adjusted through a hole in the back of the K40 case below the laser compartment.
* Y-belt 2: On the right side of the gantry. Adjusted through a hole in the back of the K40 case below the laser compartment.
* X-belt: Under the X-axis bar. Turn off the machine and leave it to discharge. Open both the main and electronics lids. Move the X-bar so that it is opposite the hole between the two compartments. You should be able to see the adjustment screw. |

You should tighten the belts so that they thrum when plucked. If in doubt a little too tight is better than a little too loose.

### Gantry & Bed Alignment
Accurate lasering requires the gantry to be square by which we mean:
* Gantry frame having 90° angles between all 4 sides
* X-gantry being square to the sides / parallel to the front/back rails - you may need to adjust the angle of the belt cams on the left/right belts to ensure this is the case
* Laser head is at right angles to the x-gantry / mirror at exactly 45° so laser beam is exactly vertical (in both x & y directions)

In addition the bed needs to be flat with relation to the gantry frame i.e. so that the distance between the laser head and the bed is the same at all points left-right and front-back.

### Laser and Mirror alignment
The idea is that regardless of where the laser head of your K40 goes to on the bed, the laser light should enter the head in exactly the same position and angle - and to do this you need to have the laser itself and the three mirrors aligned. Once you understand the procedure it is actually not that difficult, but it is an essential part of your laser setup.

Before you attempt to align laser & mirrors, you should undertake any activities which require you to remove the gantry from the case e.g. internal duct cutting, gantry alignment, belt tensioning because if you remove the gantry from the case you will need to realign the mirrors again.

There are several videos on how to do this on YouTube. One example is: [The Short K40 Laser Alignment Video](https://youtu.be/6Bcgo-CoBAs)

In essence the steps are as follows:

a. Put tape on the 1st mirror near the business end of the laser and adjust the laser position so that (very short) bursts of the beam (on very low power) hit the mirror approximately in the centre.

b. Put tape on the 2nd mirror that moves front-to-back with the gantry. Move the gantry to the back and adjust the position of the 1st mirror so that the beam hits the mirror, then move the gantry to the front of the machine and see if the spot moves. Adjust the angle of the 1st mirror both horizontally and vertically so the spot stays the same when the gantry is at the back and the front. Hopefully it will be in the centre of the 2nd mirror, but you may need to adjust the position of the laser, the mounting base of the 1st mirror or the mounting base for the 2nd mirror in order to get it to stay the same and be approximately in the centre of the 2nd mirror.

c. Put tape on the opening on the moving head where the laser beam will enter and adjust the angle of the 2nd mirror both horizontally and vertically so that the spot stays in the same position. If the spot is not in the centre of the opening you may need to adjust the position of the head on the gantry slider or the 1st/2nd mirror or laser to make it approximately central.

d. The 3rd mirror (clipped into the head itself) is typically not adjustable.

e. The lens in the bottom of the head is typically not adjustable.

### Lens orientation
The K40 lens is flat on one side and convex on the other, and the direction that it is fitted inside the laser head **is** important - **it needs to be curved side upwards**.

### Focus
Whilst the laser beam is a narrow parallel beam of light (i.e. it stays the same width as it travels away from the laser), it is not small enough to burn material really effectively. So the K40 has a lens at the bottom of the head (held in with a screw on circular retaining plate) to focus the laser beam to an even smaller dot.

On the stock K40 there is a specific distance below the laser head where the laser is finely focussed into a tiny dot. Higher or lower than that distance, the spot will be bigger and less concentrated.

The height of the bed that your material sits on will obviously need to be lower than that distance by the thickness of the material you are working with - and if you use different materials of varying thickness, then the height of the bed needs to vary to match.

To call the bed that comes with the K40 "useless" would be being kind. It is a fixed height bed (👎 for being fixed height) with a rectangular opening (👎 for this being much, much, much smaller than the laser area, 👎 for being able to drop materials through the hole), one side of which is a spring loaded clamp (👎 for the maximum clamp opening being even smaller than the opening, 👎 for the clamp not being flush with the rest of the table).

![image](https://user-images.githubusercontent.com/3001893/120121012-13b94c80-c199-11eb-9296-f68bb4948329.png) ![image](https://user-images.githubusercontent.com/3001893/120121088-7579b680-c199-11eb-8eef-0b612e1e1bb7.png)

Some people have come up with elaborate means of varying the focal distance by mounting the lens in a moveable carrier. (TODO - add a link.) A lot of people use a [variable height bed](#variable-height-bed) to achieve the same thing.

### Smoke
The extractor fan(s) will create some air flow across the bed front-to-back, but this is not typically strong enough or laminar enough to consistently and quickly remove smoke from the burn area (where it can disperse the laser beam) and keep it from dirtying your mirrors and lens. 

You should consider installing [smoke assist and / or air assist](#smoke-and-air-assist), and possibly improving the default air flow front-to-back by enlarging the air openings in the front of the lid (or propping the lid slightly open) or even adding additional fans and air ducting inside the lid.

TODO: Summary of options / advice / links on where / how to buy / make the best air/smoke assist. 

### Burn charring
When you are burning, rather than just burning particles away as tiny cinders, the laser beam can actually cause the material to catch fire where the laser beam is, giving a visible flame (like a candle flame) rather than just a bright spot.

There are various ways to mitigate this:

* Reduce power or increase laser movement speed - reducing the time that the material has to heat and catch light. However since the purpose of lasering is to burn, this is counter-productive.
* Dampen the material - the laser is still powerful enough to burn where it hits even if it is damp, but the dampness in the surrounding material prevents that from catching light
* Air-assist - a strong jet of air aimed at the point the laser hits effectively snuffs out the flame.

### Reflection charring
When you are cutting, once you have cut through the laser beam can reflect off the bed surface (grill, knife, etc.) back onto the underside of the material and char in those areas that are resting on the grill. This is particularly noticeable when cutting wood.

I have bought a spray can of Matt Black Stove Paint and intend to paint my bed surface with this to see if it prevents these reflections.
`Matt` means that it will not reflect laser light, `Black` means it will absorb the laser light instead, `Stove` means that it can survive high temperatures without burning or blistering etc. Doing this will have the added benefit of preventing reflections into your eyes from the laser hitting areas of the bed which are not covered by material.

![image](https://user-images.githubusercontent.com/3001893/124183858-bc1c4280-dab0-11eb-8820-d30a1895927f.png)


## Enhancements
If you have small solid pieces of material to cut, I suppose it is just about possible to use your K40 with just the addition of a cooling water reservoir, but most people decide to implement some of the following enhancements first.

1. [Variable height bed](#variable-height-bed)
2. [Holes in the bottom of the K40 case](#holes-in-the-bottom-of-the-k40-case)
3. [Casters](#casters)
4. [Analogue current meter](#analogue-current-meter)
5. [Smoke and air assist](#smoke-and-air-assist)
6. [Camera](#camera)

### Variable height bed
I (@Sophist-UK) never bothered even to try using the standard bed, and replaced it with an adjustable height bed of my own design built using a few bearings and a 3d printer belt off the internet, a few 3D printed parts and some steel / aluminium (aluminum for our American cousins) profile from the local DIY superstore.

You can also use a choice of beds. A solid bed is not generally considered a good choice because there is no gap below the bottom of the material and you can get charring when you cut through. Alternatives are grill (pre-shaped fine grating), knife (multiple parallel vertical bars) and pin (multiple vertical pins) to support the material - the lower the contact area, the smaller the chance of . Which one is best for you will depend on the materials you are going to use and how flexible they are - the more floppy, the more support you need.

### Holes in the bottom of the K40 case
I discovered after the event that there is a largeish hole in the bottom of the K40, and when you have burned through your material the laser can carry on down and through the hole and char the surface of the table that your laser is sitting on. I riveted an air grill across this hole.

### Casters
My K40 came with casters - which is IMO a pretty dumb thing to do with a piece of equipment like this that you really don't want rolling off the edge of the table it is sitting on. So the first thing I did was replace them with M10 furniture feet from my local DIY superstore.

### Analogue current meter
Whilst we are talking about wiring, if you have a K40 with an analogue mA meter then great, but if not you should really consider buying one and wiring it in. 

![image](https://user-images.githubusercontent.com/3001893/120121608-47e23c80-c19c-11eb-87b9-186417b41e02.png)

In order not to shorten the life of your laser you need to keep the current below c. 15mA. However, the digital power display on those K40's that have it indicates a theoretical % of total power (the K40 laser is usually a 35W laser despite the name), and this % is extremely misleading as it bears little relationship to the laser power actually generated. Those people who have installed these alongside the digital display say that a power level of 30% on the digital display can reach this maximum current, and that higher levels can **substantially** shorten the life of your laser tube. So, if you don't have a mA meter to tell you the real power, then please do not use your K40 above (say) 30% unless you want to keep shelling out for a new laser tube.

(I cannot say for certain whether this was the case for my own K40, but I was running in early days at 80% on the digital display, and I didn't get more than an hour's burn time before the tube failed - I cannot say whether these were related, but I did need to get a replacement laser tube under warranty.)

### Smoke and Air Assist
Air assist is a small air jet blowing air at the focal point of the laser to blow away smoke and help the laser beam to hit the material cleanly without smoke either reducing the power or blurring and widening the laser spot size. It can also help snuff out any flames that cause charring. Air assist is provided either with a replacement head or a cap that fits over your existing head or a carefully positioned highly directional air jet fitted to the laser head.

Smoke assist is similar but broader jet of air designed to blow smoke away from the lens and mirrors towards the extraction vent, and reduce smoke staining on the material. It is typically provided by additional fans fitted to the case (e.g. on the inside of the lid where the vent holes are provided) and / or or with a broader jet of air from a pipe fitted to the laser head.

TODO: Add further examples and links.

### Camera
MeerK40t has the ability to use a camera installed in the lid of your K40 to help you position material in the right place to cut. 

TODO: Someone who has done this and who has the experience needs to write this section.

## Charring
Not so much an enhancement, but this seems to be a suitable place to mention that when lasering materials prone to charring - in my case plywood and leather - I have used a spray bottle to spray water onto the surface of the material. Damp material or even a thin sheen of water on the surface seems to have little effect on the laser strength but does massively reduce charring and/or flames.

## Next page
We hope that this has helped you prepare your K40 hardware for your first burn. Now it is time to [install MeerK40t](./Beginners:-3.-Installing-MeerK40t).

If you want to return to the [Beginners Index](./Beginners:-0.-Index) click [here](./Beginners:-0.-Index).

---
### Authors
The MeerK40t team is grateful for the help from @Sophist-UK and @tiger12506 in creating this page. If **you** think it can be improved still further, please feel free to edit the page and add your userid to this list. 😃

### To Do
Add more example pictures and links to solutions.