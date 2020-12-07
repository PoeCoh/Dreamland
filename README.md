# Process Walkthrough
This is all a bit complicated so I'm going to run through the entire process should operate between the user, application, database, and Global Shop.
It is important to note that this process tracks boards by ASSEMBLY, not by WORK ORDER. Assemblies are attached to work orders at shipping.
- User logs into application
- Application assigns process (assembly/processID) based on priority and displays task related instructions to user
  - Priority is calculated based on estimated time required against planned shipping date
  - Estimated time required is calculated by assemblies 1000 unit moving average time for each step remaining, for each board remaining
  - If management places a hold on a work order all related assemblies and their processes will not be assigned for work until the hold is lifted
- For each board, at the start of each process user will scan QR code, application will automatically log each board and push updates to Global Shop
  - Application will verify that records exist for all previous processes, for each board, if records are not found user will be notified and board will not continue
  - Information will be pushed at the end of the process for each board, process ends when
    - User scans the next board, it is assumed that the prior board was completed
    - User clicks "OK"
    - User flags board for issue, Issue record will be created instead of a process record for this unit
  - For test process, application will collect test data in json form when possible and create a record with data for each unit. In the event of failure test should automatically generate an Issue record for the unit
  - For shipping, application will check serial number for completed records for each process for assemlby, and verify no open Issues remain. If no issues are found unit is applied to a work order
  
# Serial Number Details

Serial Numbers will be formatted as YYYYMMDDHHMMSS###, then cast to base64, then printed as a QR in batches of 1000 (000-999)
  - The date is to ensure that no two serial numbers could possibly be identical
  - Casting in base64 is to dissasociate the serial number from a date as the actual date is irrelevant
  - QR format to keep footprint as small as possible
  
  
# Database Details

Records will be held for a minimum of 4 years, after that the oldest records will be purged to make room for new records as required (Will prioritize keeping Shipping records). Management will be alerted if disk space reaches 80% with no records eligible for removal.

## Process Table
Primary table for database
   - Serial Number
   - ~~Assembly ID~~
   - ~~Assembly Revision~~ 
   - Process ID
   - User
   - Start Time
   - End Time

## Issue Table
Equivalent of the E203 with additional information stored per board.
   - Serial Number
   - ~~Assembly ID~~
   - ~~Assembly Revision~~
   - Process ID
   - Created By
   - Created Time
   - Issue
   - Closed By
   - Closed Time
   - Notes
   
## Test Table
This table will store test results in json format. This is mostly for troubleshooting, RMA, sanity checks, and will not affect the process.
   - Serial Number
   - ~~Assembly ID~~
   - ~~Assembly Revision~~
   - Process ID
     - While this will always be test, there are assemblies that require more than one test
   - Created Time
   - Results
   
## ~~Affiliated Table~~
~~This table will  be used to link multiple serial numbers together, in cases where boards are grouped for convenience, or when boards are combined into a new assmbly. When groupled boards are split their affiliate records will be removed.~~
   - ~~Serial Number~~
   - ~~Assembly ID~~
   - ~~Assembly Revision~~
   - ~~Created Date (for record removal)~~
   - ~~Affiliated Serial Numbers Array~~

## Assembly Table
This contains the assembly, revision, and affiliate data for each serial number. While this is yet another table this removes redundant data in all other tables, so when a revision or assemlby change is made only one table needs to be updated.
   - Serial Number
   - Assembly ID
   - Hardware Revision
   - Software Revision
   - Sub Assembly Serial Numbers
     - This field is used when units are combined to produce a new assembly. When other serial numbers are listed here their previous records will be maintained, but any future QR code scans will forward to the owning Serial Number.
   - Affiliated Serial Numbers
     - This field will be used to link multiple serial numbers together for convenience, for instance when boards remain in panels. When groupled boards are split their affiliate fields will be cleared.

## User Table
Used to deligate tasks and hold login credentials
   - Name
   - UserID (hashed)
   - Password (hashed)
   - Primary processes (SMT, AOI, Build... )
   - Secondary processes
   
# Experimenting with junk
  ![Image](/0000/download.jpg)
  
  [0000](/0000/test.md)
