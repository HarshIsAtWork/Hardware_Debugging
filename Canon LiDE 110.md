**Canon CanoScan LiDE 110, Dead Scanner Repair**

[IMG 1 showing scanner from 2-3 sides]

Date: 27 July 2026
Symptom: Scanner didn't respond at all. No enumeration, no movement, nothing.
Tools used: Themisto TH-M100 multimeter, soldering iron, precision screwdriver set.

**#Background**
I came back home after sem exams and had some notes to scan. 
My father pulled out this really old scanner "Canon LiDE 110". It had previously been to neighbors house
and even they tried to get it fixed in many repair shops but...NO LUCK
So i kindly asked them if i could take a look at it (because it was very intriguing)
and they Agreed. so i took it home.

`#Problem`
1. The Scanner would not get recognized as a USB device in any computer. 
2. It wouldn't power ON when plugged with Mini USB CABLE
3. There was NO movement inside the scanner

`Diagnostics`
1. First and foremost the first diagnostics to do is **USB Enumeration**.
 This tests several things at once. For example
   Whether the main SoC is alive;
   Power is constant;
   Crystal is Oscillating;
   On Board Firmware and EEPROM are working;
`RESULT`: No USB Enumeration. This means it was either power issue, or some other issue.

2. Carefully unscrewed the back panel, Took the side rails apart and carefully lifted up the glass
   This gave me the access to the insides of the scanner.
   It had 3 FFC cables going into a compartment that was screwed and shielded.
   i checked the motor mechanism by manually shifting to make sure it wasnt getting stuck.
   it was fine. so i checked the motor by giving it controlled 5v. it was moving fine.
   
   picking the head and placing it on any random place on the rack and connecting the power would initialize the position
   of the head.
   `RESULT` the head would return to its original place and blink green three times

   **This proved that the program was running fine and was initializing**
   **And also the rack and pinion were smooth and functional**

4. I then unscrewed the 



