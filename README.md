## Note

This is currently a very rough draft, mostly just to start ironing out the details.
However I do have a database up and running with the tables defined below, with a CLI in C# for directly interfacing with the database and an API in PowerShell for integration in a web app. This is technically on a LAMP stack, as PowerShell is pretty much taking the place of PHP.
I am currently experimenting with interfaces, more than likely an html as literally anything with any browser could run it.

## Current System
   - Boards are tracked by work order throughout the entire process
   - This leads to incoming work orders overlapping their preceding counterparts of the same assembly
   - Does not leverage Global Shops assemblies system creating more overhead
   
## New System
   - Boards are tracked by Assembly ID throughout entire process by QR code
   - Incoming work orders will never ship before their preceding counterpart is completed
   - Test results will be saved to a database in json format by serial number

## Product Management and Scheduling
   1. No more manual scheduling
      - Database will read active employees from Global Shop and dynamically assign jobs by priority
      - Priority is based off of estimated completion time for all remaining steps against the time until due
      - Estimated completion time for each step will be based off a 1000 board moving average, or for less than 1000 units a json with predicted times
      - Additionally interface will have the option to place a hold on a work order, all required assemblies and their steps will be removed from the schedule until the hold is cleared
      - With this system scheduling will shift focus from managing individual worker's calendars to keeping work order shipping volumes and dates accurate
   2. Utilize Global Shop's sub assemblies to streamline production and simplify management of branching products
      - Have spent a lot of time crawling through GS's documentation and this looks like something we are poorly utilizing, however this will take a lot of work to correct
   3. Assembly Instructions will be stored on a proper repository in a logical structure to make both maintanence and automation easy
      - Assembly Folder
        - Process Folder
          - Notes and images
      
## Monthly Techinical Evaluations
   In today's enviornment the attitude of "if it's not broke, don't fix it" no longer applies. It is much more efficient to make constant incremental changes than to accumulate technical debt that will have to be addressed. Cumbersomb interfaces, steps and tests should be streamlined as quickly as possible.

## Database
   Boards will be scanned at the **beginning** of every step. The system will check what step the user is working on against what step the board is on. If there are no records of any prior step the interface will alert the user and will not continue. Serial will automatically entered into the Process Table when the next serial is scanned, the user selects "complete" (or similare), or the user flags the board for a defect or other issue.
   
   Serial Number | Assembly ID | Process ID | User | Start Time | End Time
   ------------- | ----------- | ---------- | ---- | ---------- | --------
   384952 | Assy12832 | 70123 | Ben | 2020 06 24 15:01:31 | 2020 06 24 15:02:45
   
   If a board is flagged the system will request information from the user, basically a 203, and enter it into the Issues Table.
   
   Serial Number | Process ID | Issue | Created Time | Created By | Closed Time | Closed By
   ------------- | ---------- | ----- | ------------ | ---------- | ----------- | ---------
   384953 | 70123 | R1 Sk | 2020 06 24 15:03:15 | Ben | - | - 
   
   If board is flagged for a defect the individual conducting the rework closes the issue.
   
   Serial Number | Process ID | Issue | Created Time | Created By | Closed Time | Closed By
   ------------- | ---------- | ----- | ------------ | ---------- | ----------- | ---------
   384953 | 70123 | R1 Sk | 2020 06 24 15:03:15 | Ben | 2020 06 24 115:30:21 | Mike 

   Any time a board is tested a json will be stored in the Test Table.
   
   Serial Number | Created Time | Results
   ------------- | ------------ | -------
   384952 | 2020 06 24 15:05:32 | {...}
   
   If a test fails a flag will automatically be placed on it.
   
   Serial Number | Process ID | Issue | Created Time | Created By | Closed Time | Closed By
   ------------- | ---------- | ----- | ------------ | ---------- | ----------- | ---------
   384953 | 70123 | Failed Test | 2020 06 24 15:03:15 | Ben | - | - 
   
   At shipping, when a board is scanned the system will check to see if any unresolved flags exist, and verify that a record exists for all process IDs for the assembly.

   Additionally there is a User Table containing UserNames and Passwords to access the web app. Currenlty it's plain text for simplicity but will become a proper hashtable in the future.
   
## Long Term
   Initially this will be run in parallel to Global Shop, however eventually this will be the users primary method of interacting with Global Shop's systems. As this will already be collecting granular information along the entire process it can update Global Shop's database, either in real time or in a batch update after hours. A user will be able to log into the application/webapp to clock in, and Global Shop will be updated ever time the user completes a board.
   
   I'm planning on building a monitoring application that will track active processes in real time. If a user is taking X% longer than the 1000 board moving average, or if the system alerts a user that a board is missing a step it could flag management.
