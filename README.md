# Operation Dreamland

Modernize Everything

## Current System
   - Boards are tracked by work order throughout the entire process
   - This leads to incoming work orders overlapping their preceding counterparts of the same assembly
   - Does not leverage Global Shops assemblies system creating more overhead
   
## New System
   - Boards are tracked by Assembly ID throughout entire process by QR code
   - Incoming work orders will never ship before their preceding counterpart is completed

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
   In today's enviornment the attitude of "if it's not broke, don't fix it" no longer applies. It is much more efficient to make constant incremental changes than to accumulate technical debt that will have to be addressed.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/PoeCoh/Dreamland/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
