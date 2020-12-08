# Future Electronics Swisslog RFID

Customer | Assembly Name | Assembly Number
-------- | ------------- | ---------------
Future Electronics | Swisslog RFID | EDS-SLRF410623-R3

## PURPOSE AND SCOPE
The purpose of this document is to specify the setup and procedures required to perform a production verification test of the Future / Swisslog RFID EDS-SLRF410623-R3 board assembly.

## TEST LIMITATIONS AND OTHER NOTES
Keep this supplemental materials document open while testing for photos, videos, and other assistance.

This fixture was designed by Future Electronics. EDM wrote the PC test program to control the fixture.

Per note from Denis Ouellette 12/6/18: “Test coverage is 100% in terms of functionality, though some tests are linked to others ( for example if the RFID read fails, then some LEDs like the OKT will typically fail)”

All parameters such as firmware version and checksum are stored in the ‘config.py’ file in the project directory on the laptop. If the firmware changes, this file will need to be updated. (The source repository should also be updated by engineering if the config file is changed.)

Note from Denis at Future regarding occasional trouble getting the test to pass with the capsule at 3.5”:

> I can confirm that the 3.5” read test issue is related to background RF noise, at least nearby “noise”.
> - In my case, if the test jig was pushed at the back of my bench, near the bench power bar and several connected wall mount power supplies , I had trouble reading further than 2.5in. But If I pulled the test jig about 6 inches away from the back of the bench, then I could read the receiver capsule even a bit further than 4”. (I made a little video showing this)
> - So just by moving the test set-up several inches I could get a very good reading range or a very poor range. Note also that I built this 2nd test set-up, with the power input box near the fuse boxes, close to the DUT test, so away from the Receiver tube.  But even like this, I had the behavior described above. So In your case, if just moving the jig does not improve the reading range, you could try moving the power input box away from the receiver tube. Also you may have much more “ambient noise” in your plant.
> - Please also note that the 2 coupled antennas in front of the receiver tube are critical for reading range. They must be clipped in place properly.


Results are logged in a new file for each day. Be sure to move logs to the ‘Future Shared Files’ team drive at the end of each day of testing. The folder is linked at the end of this test procedure.

## REQUIRED MATERIALS
- Fixture C9223 (Future Electronics Swisslog Tester)
- Test laptop - dedicated to this job; software is pre-installed
- USB barcode scanner - capable of scanning QR codes
- ST Link V2 programmer - dedicated to this job
- Golden DUT Board - provided by Future. Keep with the fixture.
