# Operation Dreamland

Modernize Everything

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
      - Have spent a lot of time crawling through GS's documentation and this looks like something we are poorly utilizing, however this will take a lot of work to correct.
      
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
   384952 | 70123 | R1 Sk | 2020 06 24 15:03:15 | Ben | - | - 
   
   If board is flagged for a defect the individual conducting the rework closes the issue.
   
   Any time a board is tested a json will be stored in the Test Table.
   Serial Number | 
