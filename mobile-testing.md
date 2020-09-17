### Useful resources for iOS testing: 

Useful for finding the correct Jailbreak method:
https://docs.google.com/spreadsheets/d/11DABHIIqwYQKj1L83AK9ywk_hYMjEkcaxpIg6phbTf0/edit#gid=384574280

Useful reading if you'd never done mobile testing before, like me: 
https://mobile-security.gitbook.io/mobile-security-testing-guide/ios-testing-guide/0x06b-basic-security-testing

### How to jailbreak your device

I just followed this video: https://www.youtube.com/watch?v=aPbLM8ClbKk&feature=youtu.be 

I found their channel to have a lot of useful jailbreaking videos for specific versions of iOS.

#### 'Unable to Verify App' fix: 

Airplane mode: On

Settings>Safari>Clear data

Airplane mode: Off

Settings>General>Device Management>Jailbreak Profile>Verify

JB app such as Unc0ver should work now


### Proxying the traffic for analysis

BurpSuite users: Don't waste time with the Burp Mobile Assistant. I went through the trouble of installing it and it only works on iOS version 10 or less. 

I used this page to help me set up a proxy using Burp and to setup OpenSSH on the iOS device:
https://www.whiteoaksecurity.com/2020-3-19-apple-ios-13-device-setup-for-penetration-testing/

This will work for proxying web traffic and some applications. If the application is using SSL Certificate Pinning then this method will not work. After doing some research portswigger said to do SSL-Pinning it would take 'some work'.

So I used Charles proxy instead. 

Charles is easy to set up, the proxy set up in the article above will work for Charles. Just change the port to the appropriate one. If you can do what you need with the trial version of Charles I would recommend this as I paid for Charles (only 8.99) and its still a frustrating piece of software compared with Burp, but maybe thats just me.

### Dumping and analysing the IPA/App Binary

If the IPA has not been given to you, or you are working with an application from the App Store on a jailbroken device then you will need to extract it from the jailbroken iPhone. The IPA file is encrypted when it is stored on the iOS device which means we cant use simple tools like SSH to extract the IPA. 

Here is the guide I used to extract the IPA using Frida: https://ub3rsick.github.io/2019/10/22/ios-ipa-dump-decrypted/

frida-ios-dump repo: https://github.com/AloneMonkey/frida-ios-dump

The main steps for pulling the IPA are: 
1) Install Frida on host machine and testing device (using Cydia)
2) Connect test device via usb to host machine
3) Run 'frida-ps -U' if you see a list of processes running the connection via usb was a success
4) Find the app you wish to test in the output
5) Run 'python dump.py <application_name> -H <ip-of-testDevice> -p <ssh-port>

### Analysing the dumped file

Install MobSF: https://github.com/MobSF/Mobile-Security-Framework-MobSF

MobSF Docs: https://mobsf.github.io/docs/#/

Also install the following if you want to export PDF reports: https://github.com/JazzCore/python-pdfkit/wiki/Installing-wkhtmltopdf


Check MobSF runs, if successful navigate to localhost:9999 to view the MobSF front-end. Click upload and select the IPA you dumped earlier, let it do its thing and you should then be presented with a report about the IPA. 
