# Challenge 23: Local Admin No More, Security by Design

## Details:

    Author: Brendan Higgins
    Framework Category: Operate and Maintain
    Specialty Area: Systems Analysis
    Work Role: Systems Security Analyst
    Task Description: Coordinate with systems architects and developers, as needed, to provide oversight 
                      in the development of design solutions. 
## Before The Challenge

<img src="Images/Before.PNG">

## Scenario
     
     Recently we have become aware of a security risk that our current windows workstations are vulnerable to. 
     If an unauthorized user were to gain access to the system, there is not check or defense in place to guard 
     against that attacker creating themselves a local administrator account to use later for backdoor access. 
     This needs to be resolved. We need you to put in a manage safeguard to stop this from ever happening on our 
     windows workstations.

## Additional Information
  
    More details and objectives about this challenge will be introduced during the challenge meeting, which will 
    start once you begin deploying the challenge.

    You will be able to check your progress during this challenge using the check panel within the workspace once 
    the challenge is deployed. The checks within the check panel report on the state of some or all of the required 
    tasks within the challenge.

    Once you have completed the requested tasks, you will need to document the methodology you used with as much detail 
    and professionalism as necessary. This should be done on the documentation tab within the workspace once the challenge 
    is deployed. Below the main documentation section be sure to include a tagged list of applications you used to 
    complete the challenge.

    Your username/password to access all virtual machines and services within the workspace will be the following...

    Username: playerone
    Password: password123


## Meeting Notes:

`Gilly Bates`
I've had to manually remove a couple of local admins from machines I have not recognized in the last few days. I don't actually know if these are local admins from a long time ago or if they were recently created without authorization, but I think we may have a slight security vulnerability in our system by not preventing their creation in the first place...

`Thanh Akasaka`
Gilly is right, this is an issue that we should address sooner than later. I had never noticed this before.

`Richard LeGrand`
I CAN'T BELIEVE THIS. FIX THIS GUYS. EXCEPT MAKE SURE I GET TO KEEP MY ADMIN STATUS ON ALL THE BOXES BECAUSE I HAD MYSELF ADDED LOCALLY A LONG TIME AGO AND I APPRECIATE NOT HAVING TO GET PERMISSION TO DOWNLOAD EVERY LITTLE THING. SO ANNOYING.

`Gilly Bates`
Okay, @playerone fix this for us please. It's not a difficult task to restrict local administrator groups on the domain controlled Workstations with Group Policy on the AD server. Remember playerone, administrator, and myself are already domain admins. However, Gary, Richard, and Thanh need admin access to the domain as well. Make them a new security group in the DasSecGroup OU and call it TechAdmins. After that, make a new Domain-Level GPO, call it DasAdmins, and enforce it. Modify DasAdmins to restrict local admin privileges to those authorized (domain and tech admins) to be administrators and delete anyone not defined by those groups. Local administrator account should already be disabled by default so don't worry about that and just remember that domain and tech admins will have to be a member of the local, built-in administrator group.

`Richard LeGrand`
REMEMBER I WANT ADMIN STILL

`Gilly Bates`
To recap...Fix the identified local admin security vulnerability with AD changes (Groups, GPO), create a new Security Group called TechAdmins containing Gary Thatcher, Richard LeGrand, and Thanh Akasaka. Then under DasSecGroup create a new GPO called DasAdmins that allow current Domain Admins and the new TechAdmins group Local Administrator Rights with DasAdmins GPO and deny Admin Rights to everything else with the DasAdmins GPO

## Network Map

<img src="Images/Network-map.jpg" >

## Solution Write Up:

1. Created a new Active Directory Security Group:

    Go to Server Manager
    
		○ Under the Roles >
		○ Active Directory Domain Services>
		○ Active Directory User and computers>
		○ ad.daswebs.com>
		○ DasSecGroup
		○ Right Click >  New group
			§ Created OU TechAdmins Under DasSecGroup
			Added the following users to the TechAdmins group
				□ Gary Thatcher
				□ Richard LeGrand
				□ Thanh Akasaka.
   
     Create OU
  
	<img src="Images/OU.png" >
  
     Add users to OU
  
  	<img src="Images/OU-add-Users.png" >
  
2. Created New GPO Named DasAmdin root Domain (DesSecGroup) level of AD

	 	○ Under DecSecGroup
		○ Right Click and Add no GPO
		○ Set the created DasAdmin GPO to Enforced
		○ Right click on the DasAdmin GPO
		○ Click Enforced
	
     Create new GPO DasAdmins
  
  	<img src="Images/GPO.PNG" >
  	<img src="Images/GPO2.PNG" >
  
     Enforce new GPO DasAdmins
  
  	<img src="Images/GPO2-enforce.PNG" >
   
3. Linked the created DesAdmins to the ad.daswebs.com

		○ Under ad.daswebs.com
		○ Right click
		○ Click Link an Existing GPO
		○ Select DasAdmin GPO
		○ Right Click the linked DasAdmins
		○ Click Enforced 
	
    Link the created DesAdmins to the ad.daswebs.com
  
  	<img src="Images/GPO2-link .PNG" >
  
    Enforce the linked DesAdmins to the ad.daswebs.com
   
      <img src="Images/GPO2-link-enforced.PNG" >
  
4. Built-In Administrator Group Updated to Contain Domain Admins and TechAdmins Within Group Policy 
   Built-in Administrators Group Updated to Delete Member Users and Group users Within Group Policy
  
	   ○ Under The Created DasAdmin GPO.
	   ○ Edith The DasAdmin GPO.
	   ○ right click and Edith.
	   ○ Under users Configuration.
	   ○ Preference >Control panel settings > Local Users and Groups.
	   ○ Right Click then > New > local group.
	   ○ Under action set to Update
	   ○ Group name : Select … to  search for Administrator {built in} from the drop down
	   ○ Rename to: leave blank
	   ○ Description: Add the description of what this will do
	   ○ Select delete all member user
	   ○ Added user to the created  administrators group:
		§ Domain Admins and TechAdmins
	
  	
	Add Built-in Administrator to local group
   	<img src="Images/GPO-Restrictions.png" >
   	<img src="Images/GPO-Restrictions2.png" >
  
  	Check the delete all member user and group
  
  	<img src="Images/GPO-Restrictions3.png" >
  
  	Added user to the created administrators group.
  
  	<img src="Images/GPO-Restrictions4.png" >
  	<img src="Images/GPO-Restrictions5.png" >
  
5. Added TechAdmins and Domain Admins to Restricted Group
	
		○ Under computer configuration > windows Settings> Security Settings> Restricted Groups
		○ Right click on restricted Groups
		○ Click on Add group
		○ Click on browse
		○ Click Okay
		○ Click on the Administrator you added and the click properties
		○ Under Members of this Group: Add the TechAdmins and the Domain Admins Groups
		○ Under This group is a member of: Add Administrators
		○ Click okay.
	
    Add user to restricted group
    First add the built-in Administrators group 
   	<img src="Images/GPO-Restrictions6.png" >
   	<img src="Images/GPO-Restrictions7.png" >
   	<img src="Images/GPO-Restrictions8.png" >
   	<img src="Images/GPO-Restrictions9.png" >
   	<img src="Images/GPO-Restrictions10.png" >
  
  Add user Domain Admins & TechAdmins to the created restricted Administrators group
  
  <img src="Images/GPO-Restrictions11.png" >
  <img src="Images/GPO-Restrictions12.png" >
  
  Completed 
  
  <img src="Images/Finish1.1.png" >

## NICE Framework KSA

	K0006. Knowledge of specific operational impacts of cybersecurity lapses.
	K0040. Knowledge of vulnerability information dissemination sources 
	(e.g., alerts, advisories, errata, and bulletins).
	K0044. Knowledge of cybersecurity and privacy principles and organizational 
	requirements (relevant to confidentiality, integrity, availability, authentication, non-repudiation).
	K0049. Knowledge of information technology (IT) security principles and methods (e.g., firewalls, 
	demilitarized zones, encryption).
	K0056. Knowledge of network access, identity, and access management (e.g., public key infrastructure, 
	Oauth, OpenID, SAML, SPML).
	K0060. Knowledge of operating systems.
	K0075. Knowledge of security system design tools, methods, and techniques.
	K0174. Knowledge of networking protocols.
	K0263. Knowledge of information technology (IT) risk management policies, requirements, and procedures.
	K0276. Knowledge of security management.
	S0022. Skill in designing countermeasures to identified security risks.
	S0031. Skill in developing and applying security system access controls.
	S0147. Skill in assessing security controls based on cybersecurity principles and tenets. (e.g., CIS CSC, 
	NIST SP 800-53, Cybersecurity Framework, etc.).

## CAE Knowledge Units

	Cybersecurity Foundations
	Cybersecurity Principles
	IT Systems Components
	Operating Systems Concepts
	Operating Systems Hardening
	Windows System Administration
	
##  NICE Framework 1.0 KSA
	177. Skill in designing countermeasures to identified security risks
	179. Skill in designing security controls based on information assurance (IA) principles and tenets
	191. Skill in developing and applying security system access controls
	58. Knowledge of known vulnerabilities from alerts, advisories, errata, and bulletins
	63. Knowledge of information assurance (IA) principles and organizational requirements that are 
	relevant to confidentiality, integrity, availability, authentication, and non-repudiation
	70. Knowledge of information technology (IT) security principles and methods (e.g., firewalls, 
	demilitarized zones, encryption)
	90. Knowledge of operating systems

## NICE Framework 1.0 Competencies
	Configuration Management
	Identity Management
	Information Assurance
	Information Systems
	Network Security
	Operating Systems
	


