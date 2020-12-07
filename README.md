# Process Walkthrough
This is all a bit complicated so I'm going to run through the entire process should operate between the user, application, database, and Global Shop.
It is important to note that this process tracks boards by ASSEMBLY, not by WORK ORDER. Assemblies are attached to work orders at shipping.
- User logs into application
- Application assigns process (assembly/processID) based on priority and displays task related instructions to user
  - Priority is calculated based on estimated time required against planned shipping date
  - Estimated time required is calculated by assemblies 1000 unit moving average time for each step remaining, for each board remaining
  - If management places a hold on a work order all related assemblies and their processes will not be assigned for work until the hold is lifted
- For each board, at the start of each process user will scan QR code, application will automatically log each board and push updates to Global Shop
  - Information will be pushed at the end of the process for each board, process ends when
    - User scans the next board, it is assumed that the prior board was completed
    - User clicks "OK"
    - User flags board for issue, Issue record will be created instead of a process record for this unit
  - For test process, application will collect test data in json form when possible and create a record with data for each unit. In the event of failure test should automatically generate an Issue record for the unit
  - For shipping, application will check serial number for completed records for each process for assemlby, and verify no open Issues remain. If no issues are found unit is applied to a work order
  
# Database Details

Records will be held for a minimum of 4 years, after that the oldest records will be purged to make room for new records as required. Management will be alerted if disk space reaches 80% with no records eligible for removal.

## Process Table
Primary table for database
   - Serial Number
   - Assembly ID
   - Process ID
   - User
   - Start Time
   - End Time

## Issue Table
Equivalent of the E203 with additional information stored per board.
   - Serial Number
   - Assembly ID
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
   - Assembly ID
   - Process ID
   - Created Time
   - Results
   
## Affiliated Table
This table will  be used to link multiple serial numbers together, in cases where boards are grouped for convenience, or when boards are combined into a new assmbly
   - Serial Number
   - Assembly ID
   - Affiliated Serial Numbers Array

## User Table
   - Name (hashed)
   - Password (hashed)
   - Primary processes (SMT, AOI, Build... )
   - Secondary processes
