# Inova Slimline LED Module

Customer | Assembly Name | Assembly Number
-------- | ------------- | ---------------
Inova | Slimline | 630041

## PURPOSE AND SCOPE
   The purpose of this document is to specify the setup and procedures required to perform a production verification test of the 630041 and 630043 board assemblies.
   
## REQUIRED MATERIALS
   - Test Fixture w/ wall wart power suppply
   - RS232 Serial Cable
   - Schematic

## TEST SETUP
   By virtue of the design of the test set, EDM Engineering has determined that this test set does not require periodic calibration. Before each use, verify the test set function by blocking a few of the photo inputs with a small post-it note or other piece of paper. The DUT should fail with the inputs blocked. This test should be sufficient to verify proper operation of the test set.
   1. Plug in wall wart into the AC outlet, this will power up the test fixture
   2. Make sure the photo-detector area on the test set is clean and free of debris
   3. If desired, connect the serial port on the test set to a PC running serial terminal software to provide additional debugging information
      - 9600 baud
      - no parity
      - 8 data bits
      - 1 stop bit

## VISUAL INSPECTION
   Compare the board with the documentation in the work order package to ensure that all components have been installed correctly. Check the following: solder quality, parts orientation, missing parts, and raised parts.
   
## PROCEDURE
   1. Place the <abbr title="Device Under Test">DUT</abbr> onto the top of the test set, with the LEDs facing down, ensure that the pins of J3 are pointed away from the operator
   2. Install data cables from the test set onto J1 and J2
   3. Install power cable onto J3
   4. Press the `Test` button on the fixture
   5. If the test passes, the green `PASS` LED will illuminate, else the red `FAIL` LED will illuminate on test failure
   6. Lift the <abbr title="Device Under Test">DUT</abbr> and ensure that all LEDs are cycling through red, green, and off, in that order
   7. Verify the green `READY` LED on the test set is illuminated **only** when the green LEDs are illuminated on the <abbr title="Device Under Test">DUT</abbr>
   8. Remove the power cable and then the data cables, Repeat the procedure for the next unit to be tested
   9. Place label as shown
   Image
