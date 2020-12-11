# Design Principles
  Web app will consist of a main page divided into iframes. This main page will contain minimal baked in elements like navigation and will be the primary page that users will interact with. Modules like e203 will be inserted into iframes. This will host the login functions and interact with the [User Database](User.md) to get the user's skill matrix and check assembly's skill requirements. As these modules are contained in iframes it will be relatively easy to rearrange them as needed.
  
  I've experimented with PowerApps, ReTool, and a lot of other off the shelf solutions but they all charged a premium for interacting with databases... per user. Ultimatly I got fed up with those as they neither did quite what I wanted to do nor were they cost effective. For our purposes any solution would have to be web based so it could run off of any hardware so I decided to suck it up and contend with the languages that I hate and build a solution from scratch. Bonus of this is users can pull up information on ANY device. For this reason future database and server should run behind our network so you have to be on our network to use it. Until then I'm hosting on GitHub to flesh everything out, when done it's a matter of copy/paste as all references will be relational.
  
  [Main page](Main.html), very much a work in progress and might be completely broken for a while.
  
## Process Control Frame

   <iframe src="ProcessControl.html" height="300px" width="100%" title="Process Control"></iframe>

   This will probably be the main iframe that users will interact with. Serial Numbers will be scanned into this module, check their history against the process steps in global shop, and either add to the [Serial Number's array](Serial Database.md) or generate an [Issue Report](Issue Database.md), the equivalent of an E-203, on steroids. If a scanned assembly does not have a record for a prior step as defined by Global Shop, module will not generate a new record and the assembly will not continue.
   
## Documentation Frame

   <iframe src="Instruction.html" height="300px" width="100%" title="Documentation"></iframe>

   This iframe will display required work instructions for whatever process the user is currently working on. This will be automatically fetched from a repository.

# Serial Number Details
  Serial Numbers will be formatted as YYYYMMDDHHMMSS###, then cast to base64, then printed as a QR in batches of 1000 (000-999)
  - QR codes will be printed off in batches of 1000
  - The date is to ensure that no two serial numbers could possibly be identical
  - Casting in base64 is to dissasociate the serial number from a date as the actual date is irrelevant
  - QR format to keep footprint as small as possible, last 4 characters will also be displayed in plain text at the bottom of the label as some form of distinguishable marking

<p align="center">
  <img width="100" height="100" src="Unitag_QRCode_1607393414668.png"><br>
  <b>E123</b>
</p>

  
# Database Details
Records will be held for a minimum of 4 years, after that the oldest records will be purged to make room for new records. Management will be alerted if disk space reaches x% with no more records eligible for removal.

[User Database](User.md)

[Serial Database](Serial Database.md)

[Issue Database](Issue Database.md)

# Repository Details
Documentation will be stored in a repository as such:
- docs
  - moogPOS (Assembly ID)
    - SMT (Process Folder)
      - Instructions.md (Written document in MarkDown)
      - Photo1.jpg (Image used in written document)
      - Photo2.jpg
      - Skills.json (Skills requirement matrix for process)
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

# Roadmap
  I'm splitting this into 3 phases

## Phase 1
   Databases will run in parallel to Global Shop, it's only interactions will be fetching lists of processes for assemblies whenever someone starts working on a job and looking up what process someone is logged into, for the previous reason, it will not be posting any information at this time to Global Shop. Application will compare the current Serial Number's Process Array to the list provided by Global Shop, if a process is missing from the Serial Number's Array the appication will alert the user and not permit the assembly to continue. Users will still have to log yields in Global Shop.

## Phase 2
   Incorporate automated scheduling. Application will schedule individual processes based on time until due against the total estimated time to complete all assemblies in the order. Processes will be assigned to users based on their skill matrix in the [User Database](User.md) and the skill requirements listed in the processes repository location. When sufficient data is available application will use Serial Number's database to make calculations as those will be much more granular, if there is unsufficient data it will use Global Shop's records to  calculate time. Application will indicate which database it used to calculate time. Application ***might*** log user into a job when they log on, depending on how well I've ironed out posting to Global Shop.
   
   Estimated time will be calculated by:
   - Running average of 1000 assemblies per process
   - Number of assemblies remaining
   - Processes remaining for each assembly
   - Safety fudge factor
   
   Additional rules:
   - Will not assign a process unless at least 1 hours worth of assemblies are available for the next process to prevent time waste from constantly changing over to new jobs (not accusing us of doing this, but if this rule wasn't in place the application would).
   
     Exeptions:
     - If the total amount of the remaining assemblies is less than 1 hour, and all are ready for next process
     - If management flags an assembly as a priority it will push them through as soon as a user is avaible for a new job
     
## Phase 3
   Application will sit between users and Global Shop, user interaction directly with Global's systems will no longer be required. Every time a user scans and completes an assembly the application will push to Global Shop's database. Loging into and out of application will log user into and out of Global Shop. This also conveniently means we no longer have to deal with Global's glitchy interface jumping on everyone all the time.
