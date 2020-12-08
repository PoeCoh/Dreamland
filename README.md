# Process Walkthrough
Having learned that MariaDB supports JSON arrays I'm changing my tactics. This will allow me to consolidate the database by quite a bit.

## From the perspective of the user
1. User logs on Application
   - Application fetches assigned work roles for user from MDB
   
     Name | Roles
     ---- | -----
     Ben | `{ "Roles":["Test","Program"] }`
     
     - Multiple roles can be assigned
     - Application will prioritize roles based on order
   - Application creates a priority list of available jobs that match users roles
     - A job will not be prioritized if there is less than 1 hours worth of work ready for any given process, unless it is the total remainder left (30 minutes of work left and all are ready for given process)
     - Priority will be calculated from estimated time to complete all steps based on 1000 unit moving average, against time remaining until assembly is due
     - Management will have the ability to manually mark a job for prioritization, in this case it will take precedence for first available user with matching roles
     - Note: prioritization is per process, not globally for the entire assembly
2. Application fetches information for assembly
   - List of all prior processes from Global Shop
   - Documentation and any other required files for this process
   - Shop issues related to this assembly
3. User scans each assembly at start of process
   - Application fetches Serial Number's data from MDB
   
     SerialNumber | Assembly | Process
     ------------- | -------- | -------
     sd399Dh3123 | moogPOS | `{ "SMT":["YYYYMMDDHHmmSS","YYYYMMDDHHmmSS"], "AOI":["YYYYMMDDHHmmSS","YYYYMMDDHHmmSS"] }`
     
   - Application verifies that all prior steps have been completed
   - Application notes current time for logging purposes
   - Process for board ends when
     - User clicks `Complete`, Process Record is updated for serial number
     
       SerialNumber | Assembly | Process
       ------------- | -------- | -------
       sd399Dh3123 | moogPOS | `{ "SMT":["YYYYMMDDHHmmSS","YYYYMMDDHHmmSS"], "AOI":["YYYYMMDDHHmmSS","YYYYMMDDHHmmSS"], "Build":["YYYYMMDDHHmmSS","YYYYMMDDHHmmSS"] }`
     
     - User clicks `Issue`, an Issue Record is generated instead of updating the serial number's Process Record
       
       SerialNumber | Created | Closed
       ------------- | ----- | ------
       sd399Dh3123 | `{ "Process":"Build", "CreatedOn":"YYYYMDDHHmmSS", "CreatedBy":"Ben", "Issue":"Shits fucked up" }` | `{ "Process":"Rework", "ClosedOn":"YYYYMMDDHHmmSS", "ClosedBy":"Ben", "Notes":"Fixed everything" }`
     
     - User scans another board, prior board is assumed complete
4. User requests new job when out of available assemblies for this process, or flags the process itself
   - User can request a new job at any point, but if nothing has changed they will be served the same assembly/process
   - If user flags a process management will be immediantly notified   

Notes:
- Processes are tracked and prioritized by ASSEMBLY ID, not WORK ORDER. Assemblies are attached to work orders in shipping
- For shipping, when assembly is scanned application will verify all processes have been completed and no Issue Records remain open
- For test, any data output by software will be captured and stored in a Test Record in JSON format
  
  Serial Number | Process | Data
  ------------- | ------- | ----
  sd399Dh3123 | Test | `{ ... }`
  
  - Data is captured purely for troubleshooting, RMAs, and sanity, it is not used for any verification or calculation
  
## From the perspective of management
- Application displays list of priority assemblies based on 1000 unit moving average against time remaining until assembly is due, in order of severity
- Management can manully prioritize a process that is excluded due to units being below threshold for prioritization, or to force application to assign process to next available user with matching roles
- Application monitors posts to database, it will flag any process that exceeds x% of its 1000 board moving average
  - This tracks on both ends, above or below as both can be indicators of issues
- Any process flagged by a user will be displayed
  - Application will require someone to affirm they have seen the flag within x time, else it will start sending emails and chats/texts if possible
  
# Serial Number Details
Serial Numbers will be formatted as YYYYMMDDHHMMSS###, then cast to base64, then printed as a QR in batches of 1000 (000-999)
  - The date is to ensure that no two serial numbers could possibly be identical
  - Casting in base64 is to dissasociate the serial number from a date as the actual date is irrelevant
  - QR format to keep footprint as small as possible, last 3 digits will also be displayed in plain text at the bottom of the label as some form of distinguishable marking
  
![QRC](Unitag_QRCode_1607393414668.png)
  
## .      123
  
# Database Details
Records will be held for a minimum of 4 years, after that the oldest records will be purged to make room for new records. Management will be alerted if disk space reaches x% with no more records eligible for removal.

# Documentation Details
Documentation will be stored in a repository as such:
- docs
  - moogPOS
    - SMT
      - Instructions.md
      - Photo1.jpg
      - Photo2.jpg
    - AOI
    - Test
    - Ship
  - gridpointPOS
    - SMT
    - AOI
    - Build
      - Instructions.md
      - Photo1.jpg
    - Test
    - Ship

This will allow for the application to easily find the correct information for any process and simplify automation where possible
