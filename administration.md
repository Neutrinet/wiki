<!-- TITLE: hub admin -->
<!-- SUBTITLE: Administration - Bestuur - Office -->

:warning:  OUTDATED - À RELIRE :warning: 
# Origine
* Le pad de mars 2016 : https://pad.lqdn.fr/p/neutrinet-administrative-hub
* Qui est devenu la page : https://neutrinet.be/hubs/administrative.html

# English

Objective: handle everything related to administrative task and things that support those actions
Needed knowledge: there is not absolutly needed at all knowledge to act in this hub
Good to have knowledge: ASBL/VZW related knowledge, accounting (basic), maybe a bit a law knowledge, how bank and administration works in generale, for backoffice tools development python/django

## Active people:

* Denis
* Sombrefay - treasurer
* Quentin
* Bram - coordinator/dev
* Thomas

## Activities

ASBL handling:
		statuts handling (renewal, modification, paperwork for that)
		fiscal declaration
		contacting every one for the AG
relation with IBPT (telecom regulator of Belgium)
		generally that's basically answer a paper once per year and pay some stuff once per year
Members management
		hold and update a list of our members
		have the list of our future members (people that have received a cube but who hasn't started to pay their bill)
		check who has started paying for their VPN access -> they are now a member
Banks (we are at recordbanks for now, we used to be at bpost)
		handle communication with banks
		have access to the bank account for paying stuff and refund members that have used their money for that
Accounting
		do it, keep it up to that
		in theory, accounting is now only important recordbanks csv into admin.neutrinet.be and checking that everything wents fine and that it is coherent with reality
		making bills
		storing all the bills (important!)
Financial tracking
		how much is coming in, how much is going out
		who is paying what and how and who should be paying
		being able to answer to the question "can we afford that?"

## Used tools

dropbox (yeah ...)
admin.neutrinet.be (accounting, member management)
billy script
pv.neutrinet.be

## Current things to do

answer to IBPT email received on the 18/3
update member listing with recents cube ordering
ask members that don't have a VPN connection for their cotisation
update the financial planning/status
update our accounting to match reality in our new tool (once this is done we will have public accounting)
Thomas: uses https://github.com/frankwiles/django-admin-views to put link into https://admin.neutrinet.be/admin/ to https://admin.neutrinet.be/accounts/ and https://admin.neutrinet.be/accounts/uploadrecordbank_csv/
Bram history of modification of the bank movement by user
link transfer bank to user membre
port the user diff for bank movements
explore the possibility of another bank that don't stupidly block our account
		triodos?
		newb?
		keytrade?
		other?
repay Bram for the latest orders done by his account (and the fine)
it would be great to have a calendar for Neutrinet (export caldav/ics)
AurÃ©lien: centralize informations on how to communicate between each other (include pgp keys)

## Future things that can be done

go away from dropbox to use the Owncloud already deployed by Wannes
switch to an open pricing for cotisation
improve backoffice tools
automatised all the things
open a plan B bank account (keytrade ?)
get a VISA card
have a public accounting

# Français
À faire

# Nederlands
Te doen