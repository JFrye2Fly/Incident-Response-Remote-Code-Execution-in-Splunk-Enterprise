# Remote Code Execution Suspected in Splunk Enterprise

## Introduction

In this Incident Response Scenario it appears that someone made a call to a very supicious URL: xxxhttp://18.219.80.54:8000/en-US/splunkd/__upload/indexing/preview?output_mode=json&props.NO_BINARY_CHECK=1&input.path=shell.xslxxx
<br> <br>


## Body 
### When examining the traffic fro the Source IP to the Destination IP there is one observable network interaction 
<br>

xxxhttp://18.219.80.54:8000/en-US/splunkd/__upload/indexing/preview?output_mode=json&props.NO_BINARY_CHECK=1&input.path=shell.xslxxx
 <br>  <br>



### When searching the Source IP address on Virus Total this is what we find: 

- **This IP address is a Chinese IP address associate with Phishing attacks**

**Shell.sh, Shell.xsl and shell.zip are all common files used by this IP:**


 <br>
<h2> Let’s Check out the Splunk Enterprise Server to see the scope of the Damage by this malicious URL request </h2>


 <br>
***I downloaded the malicious file to my computer and grabbed the Sha256 Hash…***

 <br>
Let’s see what VirusTotal says about this Hash…
To my surprise… this file hash is clean: 

 <br>
<h2> BACK TO THE PROCCESES… CHECK OUT THIS SUSPICIOUS PATTERN I FOUND AFTER DOING SOME RESEARCH </h2>


1. Adding a new user to the Splunk Server is not suspicious in and of itself

2. 2. pam_tally2 --user analyst --reset --quiet

This command is used to set the login failed attempts of the user “analyst” to 0… It also designates it to run in —“quiet” mode… This causes no output to appear on the computer screen and it turns off logging for this event of resetting the failed login attempts for the “analyst” user.

WAIT… WHY WOULD AN ACCOUNT THAT WAS JUST CREATED NEED TO HAVE IT’S FAILED PASSWORD COUNTER RESET TO 0?

It’s super suspicious and after doing some research it’s a common pattern for attackers to be able to bypass authentication…

3. 3. The Admin account sets a password for the Newly added user “analyst” 

## Conclusion

In this Incident Response scenario it was detected that Remote Code Execution was enacted on the Splunk Enterprise Server after a shell.xsl excel file was opened. The bad actor then created a new Splunk user named "analyst" and created a password for this user in order to establish persistence. 
