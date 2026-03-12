# New Hire Old Artifacts – Investigation

Platform: TryHackMe  
Category: Digital Forensics / SIEM Investigation

Tools Used: Splunk

## Scenario

In this investigation we analyze historical endpoint logs from a finance department workstation.
Security alerts indicated that antivirus protection was disabled and suspicious executables were executed on the host.

The goal of the investigation is to review available logs and determine:

	•	What activity occurred on the system
	•	What malware or tools were executed
	•	Whether persistence or defense evasion techniques were used

## Investigation Steps

### Step 1 – Reviewing the Logs

The investigation begins by reviewing the endpoint logs stored in Splunk.
These logs include process creation events, file activity, and system behavior.

The initial search focuses on identifying suspicious executables and abnormal process activity.

Example search: index=main

This allows us to begin exploring the activity recorded on the host.

(Add screenshot of Splunk dashboard here)
![Splunk Logs](screenshots/splunk_logs.png)

### Step 2 – Identifying Suspicious Executables

During log analysis, a suspicious executable related to browser password extraction is discovered.

This tool is typically used to dump stored browser credentials, which may indicate credential harvesting activity.

The executable was located within a temporary directory, which is often used by attackers to stage tools.

Example suspicious file path: C:\Users\Finance01\AppData\Local\Temp\

The presence of credential harvesting tools suggests the attacker attempted to collect stored user credentials.

### Step 3 – Suspicious Findings

During the investigation several suspicious indicators were discovered including unusual files and evidence of previous user activity.Step 3 – Defense Evasion

Further log analysis shows that Windows Defender protections were disabled on the system.

Disabling endpoint security is a common attacker technique used to prevent detection while malicious activity occurs.

This behavior indicates defense evasion tactics.

Example activity observed: Windows Defender settings modified

(Insert screenshot of the relevant log entry)


### Step 4 – Additional Malware Execution

Another suspicious executable was identified during the investigation: EasyCalc.exe

The executable was located within the roaming AppData directory: C:\Users\Finance01\AppData\Roaming\

This directory is commonly abused by attackers because users typically have write permissions and files stored there often avoid immediate scrutiny.

Logs show that this executable loaded multiple dynamic link libraries (DLLs), including:
ffmpeg.dll
nw.dll
nw_elf.dll

These DLLs suggest the executable may be a packaged application or malicious payload.

### Step 5 – Evidence of Attacker Activity

Based on the log data, the attacker appears to have performed several actions:
	1.	Executed a credential harvesting tool
	2.	Disabled Windows Defender protections
	3.	Deployed an additional executable payload
	4.	Loaded supporting DLL files

These activities indicate a compromise of the host system.

## Indicators of Compromise (IOCs)

- Suspicious files:
    EasyCalc.exe
    IonicLarge.exe

- Suspicious DLL files:
    ffmpeg.dll
    nw.dll
    nw_elf.dll
  
- Suspicious directories
    C:\Users\Finance01\AppData\Roaming\
    C:\Users\Finance01\AppData\Local\Temp\
  
- Security control modification
    Windows Defender disabled


### MITRE ATT&CK Techniques Observed

The activity in this investigation aligns with several MITRE ATT&CK techniques:

Credential Access
Credential Dumping

Defense Evasion
Disable Security Tools

Execution
User Executed Malware

Persistence
Malicious Executables in User Directory

### Conclusion

The investigation identified multiple indicators suggesting the workstation was compromised.

Evidence shows that an attacker executed credential harvesting tools, disabled Windows Defender protections, and deployed additional malicious executables within user directories.

These actions indicate a likely attempt to collect credentials and maintain access to the system.

Further response actions would include:
	•	isolating the affected host
	•	removing malicious files
	•	resetting compromised credentials
	•	performing additional network analysis

## Lessons Learned

This investigation demonstrates the importance of monitoring endpoint logs and detecting suspicious process activity.

Key takeaways include:
	•	Attackers frequently store tools in Temp and AppData directories
	•	Disabling security tools is a strong indicator of compromise
	•	SIEM platforms such as Splunk are critical for investigating endpoint activity
