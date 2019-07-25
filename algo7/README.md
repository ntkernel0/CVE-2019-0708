[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/0)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/0)[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/1)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/1)[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/2)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/2)[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/3)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/3)[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/4)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/4)[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/5)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/5)[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/6)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/6)[![](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/images/7)](https://sourcerer.io/fame/algo7/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/links/7)

bluekeep_CVE-2019-0708_poc_to_exploit

## Porting BlueKeep PoC from @Ekultek & @umarfarook882 to actual exploits.

### Script kiddies are not welcomed here as at anywhere else.

Please read the through theissues (both [closed](https://github.com/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/issues?q=is%3Aissue+is%3Aclosed) and [open](https://github.com/algo7/bluekeep_CVE-2019-0708_poc_to_exploit/issues) beofre posting stuff like "It doesn't work", "Nothing happened after I ran the script", or "Error (without being specific), please help me".

#### Welcome to [Join](https://discord.gg/4kc72bZ) our Discord Server


### ! Please note that this is not yet an exploit but rather an attempt to port existing PoCs to actual exploits.
============================================================================
#### The project is on going. 
============================================================================
#### I am just like anyone of you who enjoy solving complex problems and learn from the process. I am here just to share my progress while at the same time get some opinions from the public. Sharing is caring :)
============================================================================

I have got working shell codes when standalone. However you have to generate your own and customize it to suit your need. This is not some off-the-shelf exploits which you can just grab and check out.

Furthermore, the methods of delivery is also important to make sure your codes will execute on the remote machine.

So far we still aren't able to succesfully to pop a shell and achieved RCE; however, intensive research is being done in order understand the inner working of the vulnerability.

### FYI:
Most of the scanners and PoCs out there work by only analyzing the responses from the targeted hosts and determine if the hosts are vulnerable or not. (As all of you should know, patched and unpatched versions return different reponses, as well as O.S that are not affected). They don't actually "exploit" the targeted hosts. In order to achieve RCE, first we should try to trigger the vulnerability by sending specially crafted packets (refer to RDP MSDN for protocol specifications). After the vulnerabiliy is triggered, the second step is to analyze the crashed or memory dumps to figure out how our codes can fit in. It's not as simple as most of us think it is.

Some useful resources:

1. [CVE-2019-0708: A COMPREHENSIVE ANALYSIS OF A REMOTE DESKTOP SERVICES VULNERABILITY | Client-side (mostly)](https://www.zerodayinitiative.com/blog/2019/5/27/cve-2019-0708-a-comprehensive-analysis-of-a-remote-desktop-services-vulnerability)

2. [RDP Connection Sequence](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-rdpbcgr/023f1e69-cfe8-4ee6-9ee0-7e759fb4e4ee)

3. [Analysis of CVE-2019-0708 (BlueKeep) | Server-side](https://www.malwaretech.com/2019/05/analysis-of-cve-2019-0708-bluekeep.html)

You can use the Magic Unicorn from @trustedsec for generating shell codes.
https://github.com/trustedsec/unicorn


**Note: Please use Python 3

### RDP Connection Sequence:

The RDP client initiates the connection when the user provides the name of the remote desktop to connect to. The RDP client initiates a connection to the RD Session Host by sending an X.224 Connection Request protocol data unit (PDU)

The RD Session Host responds with an X.224 Connection Confirm PDU.

The RDP client sends a Multipoint Communication Service (MCS) Connect Initial PDU with GCC Conference Create Request. --> [Vulnerability is related to this request](https://www.zerodayinitiative.com/blog/2019/5/27/cve-2019-0708-a-comprehensive-analysis-of-a-remote-desktop-services-vulnerability).

The RD Session Host responds with an MCS Connect Response PDU with GCC Conference Create Response.

The RDP client sends an MCS Erect Domain Request PDU.

The RDP client sends an MCS Attach User Request PDU.

The RD Session Host responds with an MCS Attach User Confirm PDU.

The RDP client sends multiple (in this case six) MCS Channel Join Request PDUs.

The RD Session Host sends multiple (in this case six) MCS Channel Join Confirm PDUs.

The RDP client sends a Security Exchange PDU.

The RDP client sends a Client Info PDU.

The RD Session Host sends a License Error PDU-Valid Client.

The RD Session Host sends a Demand Active PDU.

The RDP client responds with a Confirm Active PDU.

The RDP client sends a Synchronize PDU.

The RDP client sends a Control PDU-Cooperate.

The RDP client sends a Control PDU-Request Control.

The RDP client sends zero or more Persistent Key List PDUs. In this case, zero PDUs are sent.

The RDP client sends a Font List PDU.

The RD Session Host sends a Synchronize PDU.

The RD Session Host sends a Control PDU-Cooperate.

The RD Session Host sends a Control PDU-Granted Control.

The RD Session Host sends a Font Map PDU..


