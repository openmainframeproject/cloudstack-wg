**Open Mainframe Project**

Following is the vision and mission of the Open Mainframe Project (OMP).

**Vision**

Linux on the Mainframe as the standard for enterprise class systems and applications

**Mission**

Build community and adoption of Open Source on the mainframe by:

* **Eliminating barriers** to Open Source adoption on the mainframe

* **Demonstrating value** of the mainframe on technical and business levels

* **Strengthening collaboration** and resources for the community to thrive

**The "Cloudstack" working group**

There is a working group that meets every other Friday from 11:00-12:00 Eastern time.  How can this group be more effective in attaining the the OMP mission?

What is the goal of this working group/subcommittee?

* Maintain this document on how to deploy an open source based private cloud on System z

* Create a 3-hour class for SHARE/VM Workshop/etc. to show Private cloud on z?

**Private cloud solution high level goals**

Following are ten high level goals that have been considered in the design and construction of a Private cloud solution on System z

1.    Meet users' real needs - the solution should be acceptable to end users, not administrators.

2.    Use a phased approach - *Continuous delivery* should work for this solution.

3.    Ensure accessibility - The self-service portal should work for all browsers and be easy to navigate.

4.    Ensure usability - Elementary tasks should be easy and drill-down intuitive.

5.    Personalize the portal - allow user preferences to be set.

6.    Keep content clear and concise - Do not require deep IT knowledge nor platform specifics.

7.    Offer multiple means of help - Documents, articles hover-help, links to chat, defect opening, etc.

8.    Measure the portal - Constantly monitor the portal's performance.

9.    Continuous improvement - Get user feedback to improve usability and performance

10. Availability - The portal should never go down.

**Private cloud specific goals**

Following are more specific goals of a Private cloud solution on z:

Allow *self-service* - Users with credentials will be able to build, rebuild, destroy, configure and report on zLinux guests for the groups that they are members of.

Require *authentication* - Users must first authenticate with valid Linux user/password credentials before they can access the self-service portal.

Enforce *authorization* - Users will only be able to operate on systems in their Linux group. Those who are members of more than group will be able to change groups, in order to operate on guests in that group. Members of the vmlinux group will be treated as administrators and will be able to see all groups.

Enable *groups* - the main grouping guests will be based on users’ primary Linux group. Within each group, a secondary grouping mechanism is afforded. This grouping level is named *projects*.

Allocate *quotas* - groups are given quotas for the number of CPUs and the amount of memory. Once a group has exhausted their quota of CPUs or memory, members of that group will not be able to provision more guests. Administrators will be able to set groups’ quotas.

Allow guest *filtering* - to manage the list of guests displayed in the portal, associates will be easily able to filter the list of guests by host name, or by project. Administrators will also be able to filter on group.

Maintain an *audit trail* - all operations must be logged with user names and the groups they are running as.

Enable additional function for System z administrators, including the ability to modify quotas, permanently delete guests, run arbitrary commands

**Required operations**

The following operations are required for self-service.

Operations related to **security** are as follow:

* **Login                    	**Gain access to the self-service environment

* **Logout                 	**End the self-service session

Operations related to **cloning** guests are as follow:

* **Build                     	**Create new guest(s) in new virtual machine(s)

* **Rebuild                	**Replace Linux OS and data in existing virtual machine(s)

* **Disable		**Disable guest; not usable/startable (resources remain)

* **Purge                   	**Actually delete a destroyed system (admins only)

Operations related to **recycling **guests are as follow:

* **Power on            	**Boot powered-down guest

* **Power off           	**Shut down running guests (soft/hard down)

* **Reboot (hard)   	**Shut down, log off, log on then boot guests

* **Reboot (soft)     	**Recycle guests without a z/VM logoff

* **Stop                     	**Freeze all CPUs (administrators only)  maybe not

* **Start                    	**Resume frozen guests (administrators only)   maybe not

Operations to **configuring** guests are as follow. These operations will correspondingly add to or detract from the group’s quotas.

JSV NOTES:  Add/Remove CPU and MEMORY could easily be ONE API each.  For instance, a call with "Modify_CPU" and CPUs=n or CPUs=+n or CPUS=-n.  Without +/- the cloud-host would deterime if it is adding or removing CPUs

* **Add CPUs            	**Dynamically add CPUs to guests up to a set maximum

* **Remove CPUs    	**Dynamically remove CPUs from guests down to one

* **Add memory      	**Dynamically add memory to running guests

* **Remove memory	**Dynamically remove memory from running guests

* **Capping/SHARE	**Dynamically change SHARE settings (capping, etc)

* **Add/expand disk	**Minidisks/EDEV/DASD

* **Set group        	**Change the group running guests (for scope of view/control)

Operations to **Reporting** on guests are as follow:

* **System details        **Report on all aspects of selected systems

* **System expirations	**Report on when selected systems expire

* **Capacity reporting 	**Point to ES Capacity Planning guest

* **LIST			**Simple listing of all existing guests

 

 

**API Recommendations**

**Common call structure for all API calls**

Through a secure/SSL POST call to the Cloud Host, a JSON data block (body) would be constructed and sent containing the API name in the header as a dict type and any required data for the API.

*	{"api_name": {“var1”:”val1”,“var2”:”val2”, … } }*

**Common Responses to all API calls**

The header of the JSON data should contain the following for all calls:

	

<table>
  <tr>
    <td>rc</td>
    <td>The return code for the API called</td>
  </tr>
  <tr>
    <td>rs</td>
    <td>The reason code for the API called</td>
  </tr>
  <tr>
    <td>apimsg</td>
    <td>Any error or status message for the API call</td>
  </tr>
  <tr>
    <td>errfunc</td>
    <td>The function or process that caught the error</td>
  </tr>
</table>


**SECURITY**

**Generate TOKEN**

API would allow the caller to receive a token to be used for further requests. Caller would pass credentials (userid/password) to be authenticated for a token to be generated.

Token should have a "life span" and expire after a defined time of non-use

Token should be tied to IP address of caller at the least, along with some other identifier

<table>
  <tr>
    <td>API Name</td>
    <td>POST or GET</td>
    <td>Data Type</td>
    <td>Contents</td>
  </tr>
  <tr>
    <td>get-token</td>
    <td>POST</td>
    <td>string</td>
    <td>{
"userid":”xxxxxx”,
“password”:”xxxxxx”
}</td>
  </tr>
</table>


Responses:

rc 0 in header is success; anything else is failure

On success header will contain

"auth-token":”string...”

Well, I gave it a start… quite a lot more to go. James V

