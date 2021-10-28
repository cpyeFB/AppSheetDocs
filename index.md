# EHS AppSheet Troubleshooting Guide


### Recurring Issues

- **Server-Side Timeouts**
  - **Issue Rundown**
    - The issue seems centered around the part of the sync process where the AppSheet server tries to fetch resources from the database (Google Sheets in this case).  
    - Most built in tools for limiting the size of the working data set (such as security filters) are not applied to the data until the entire Google Sheet is fetched into the AppSheet server. So once the data is in the server, we can begin filtering what gets pulled down into the client. However, this doesn't really help us if the timeout occurs when the server is just trying to get the whole data set in the first place.
  - **Potential Solutions**
    - Moving to a SQL database could be an effective fix due to the use of Active Database Filtering. When connected to a SQL DB, AppSheet can convert security filters into a direct SQL query that will return only the results of the filtered query to the AS server. This essentially lets us limit the scope of the data set initally fetched by the AS server, which we are unable to do with Google Sheets at this time.
    - It seems that this issue shouild be fairly prolific for most enterprise level applications. Having some AS engineers take a close look is probably a good idea no matter what, and could be a step toward resolution.
  ---

- **Expression Miss-Evaluation**
  - **Issue Rundown**
    - While quite rare, there are occasionally instances where a simple expression that has been functioning perfectly fine will suddenly begin returning the wrong value. In every case I'v eenountered so far, it has been a LOOKUP() or SELECT() expression, and if I remember correctly, always in a google doc template. What seems to be happening under the hood is that the expression stops being able to find a value that meets the Yes/No condition, but rather than return nothing it returns another random value from the list specified by the LOOKUP()/SELECT().
  - **Potential Solutions**
    - In one case, I was able to resolve the issue by simply changing the expression to different but equivalent one. I believe I switched out a LOOKUP() with an equivalent SELECT(). The other two times this happened, I determined that the value that should have been returned was actually not meeting the Yes/No condition. Updating the condition to be correct fixed the issue both times. 

---

- **PDF Generation Issues** 
  - Sometimes, for a myriad of possible reasons, the PDF inspection reports saved to Google drive will fail to be generated, or need to be updated manually. If the PDFs failed to generate, then whatever caused this needs to be addressed first. After that, the reports will need to be recreated. There are three action buttons on the Inspections table (soccer ball icons) that are typically set to not display. They are labeled by the status they will set the status column to, and "Admin Workaround." These actions will trigger the bot that creates the PDFs, but will skip the update meta-data fields so they remain accurate. 
  
![Admin Workaround Actions](/assets/Admin%20Actions%20Screenshot.png "Admin Workaround Actions")

___

- **Action Visibility** 
  - One of the most common troubleshooting requests is simply that a task is missing. 
