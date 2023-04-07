# Burp Suite Interception - Installing CA Certificate

To intercept traffic using Burp Suite, Burp Suite's Certificate Authority (CA) certificate needs to be installed and trusted on the iOS device.

This allows Burp Suite to be the primary "CA" for the device and will allow for interception of HTTPS traffic (providing certificate pinning isn't enabled). If certificate pinning is enabled for an application. See the "Certificate Pinning Bypass" section of the cheatsheet.

## Installing the CA

Once the device has been set up as documented in the previous steps, you will be able to use the "Safari" web browser on the jailbroken iOS device to navigate to 

"https://burp"

This will take you to a page that has the text "CA Certificate" in the right hand corner of the screen.

Clicking the text will download Burp Suite's CA Certificate to the iOS device.

The device will show the prompt:

`The website is trying to download a configuration profile. Do you want to allow this"`

Click:

`Yes`

Then go to 

`Settings > General > Profiles"`

Select the profile and enable.

Once this is done, Burp Suite will now be allowed to intercept traffic for applications that do not implement certificate pinning.
