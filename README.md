# EDR-Home-Lab-Attack-and-Defense

Project:
This lab is dedicated to simulating a real cyber attack and endpoint detection and response. Utilizing Eric Capuano's guide online, I will be using virtual machines to simulate the threat & victim machines. The attack machine will utilize 'Sliver' as a C2 framework to attack a Windows endpoint machine, which will be running 'LimaCharlie' as an EDR solution.

Eric Capuano's Guide: https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-intro?utm_campaign=post&utm_medium=web

# Setup
The first step to the lab is setting up both machines. The attack machine will run on Ubuntu Server, and the endpoint will be running Windows 11. In order for this lab to work smoothly Microsoft Defender should be turned off (along with other settings). I am also going to be installing Sliver on the Ubuntu machine as my primary attack tool, and setting up LimaCharlie on the Windows machine as an EDR solution. LimaCharlie will have a sensor linked to the windows machine, and will be importing sysmon logs.

Windows 11 Machine -
![1](https://github.com/user-attachments/assets/754bbbf4-7f7a-401b-ba3b-897691600f79)
![image](https://github.com/user-attachments/assets/803f129d-0cd5-43b8-9d1c-eccb62e74165)
![image](https://github.com/user-attachments/assets/3a7718f7-4df9-4a87-8218-50302bcc21d6)
![image](https://github.com/user-attachments/assets/b2936041-b30f-4981-a7eb-17757867f6c7)

Ubuntu Server Machine - Capture6
![image](https://github.com/user-attachments/assets/944f6fa6-f968-4ee9-9413-c6ddff587225)

# The Attacks, and the Defense
The first step is to generate our payload on Sliver, and implant the malware into the Windows host machine. Then we can create a command and control session after the malware is executed on the endpoint.
![image](https://github.com/user-attachments/assets/ee80a16a-61ac-4430-89ce-72d78b9ffd0c)
![image](https://github.com/user-attachments/assets/2aad77c0-288f-4e16-b98a-6cc97b5f4caa)
![image](https://github.com/user-attachments/assets/cdcee30c-50f3-489b-92ca-45c3a68630e3)

Now that we have a live session between the two machines, the attack machine can begin peeking around, checking priveleges, getting host information, and checking what type of security the host has.

![image](https://github.com/user-attachments/assets/1d0f31f2-f6cb-476d-9b5b-cfd47486fb93)
![image](https://github.com/user-attachments/assets/68a63dd4-c3b8-4ca4-8f2b-b7d334aa9fc2)

On the host machine we can look inside our LimaCharlie SIEM and see telemetry from the attacker. We can identify the payload thats running and see the IP its connected to.

![ky6xiw76](https://github.com/user-attachments/assets/3b7ee68b-d7b7-4007-a334-29897391ecc7)
![image](https://github.com/user-attachments/assets/c4423766-b8c8-4328-95d8-fa6007221f0e)
![image](https://github.com/user-attachments/assets/b0711ec7-7297-4809-904d-bdc24e615396)

We can also use LimaCharlie to scan the hash of the payload through VirusTotal; however, it will be clean since we just created the payload ourselves.

![image](https://github.com/user-attachments/assets/e5087005-6906-4ecf-9309-5a27384e8087)
![image](https://github.com/user-attachments/assets/b1c6aa96-93cd-4ac8-9266-f8a1f5798d99)

Now on the attack machine we can simulate an attack to steal credentials by dumping the LSASS memory. In LimaCharlie we can check the sensors, observe the telemetry, and write rules to detect the sensitive process.
![image](https://github.com/user-attachments/assets/9eb72158-66db-4a64-90fa-7452b1cdbed1)
![image](https://github.com/user-attachments/assets/92c745ed-3652-4ff6-85aa-fa805267a8c6)
![image](https://github.com/user-attachments/assets/6436a3a3-3f30-4c08-aecd-91a046837d0b)

Now instead of simply detection, we can practice using LimaCharlie to write a rule that will detect and block the attacks coming from the Sliver server. On the Ubuntu machine we can simulate parts of a ransomware attack, by attempting to delete the volume shadow copies. In LimaCharlie we can view the telemetry and then write a rule that will block the attack entirely. After we create the rule in our SIEM, the Ubuntu machine will have no luck trying the same attack again.

![image](https://github.com/user-attachments/assets/c6c76807-36b9-465c-beaa-a657da21facd)
![image](https://github.com/user-attachments/assets/a75af825-6404-4807-a1e1-f86d38574830)
![image](https://github.com/user-attachments/assets/63cb7672-2c9e-44f3-a043-d47495f54dbd)
![image](https://github.com/user-attachments/assets/8f9410ab-a36b-49ec-8cac-c623ff25feb4)




