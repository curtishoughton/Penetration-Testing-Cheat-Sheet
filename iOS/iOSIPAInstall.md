# Installing an IPA on a Jailbroken Device

## Appinst

The tool "appinst" software can be installed using Cydia. First the repository needs to be added to Cydia:

`https://cydia.akemi.ai`

Once the repository has been added to the list of sources, click refresh. This will load all the software available from the new repository.

Search Cydia for the `appinst` software and install.

Copy the IPA file to the jailbroken device using SCP (or similar software) - once an SSH connection to the device has been made

Copy the file to 

`/private/var/mobile/Documents`

SSH into the device, using the default password 'root' and 'alpine'.

Navigate to the directory where the IPA has been copied into 

`cd /private/var/mobile/Documents`

Install the IPA

`appinst <name_of_IPA.ipa>`

It is possible you may have to trust the application on the device by going into `Settings > General > Profiles & Device Management (or Device Management)` Find the applications profile , select, and click `Trust`

After all this has been completed, the application will be installed.



