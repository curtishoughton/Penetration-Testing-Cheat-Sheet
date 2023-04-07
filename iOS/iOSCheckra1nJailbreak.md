# Jailbreak an iOS device with Checkra1n

I've found this works best using a Laptop/Desktop with Linux natively installed. I tried the Checkra1n jaibreak using a Virtual machine (both virtual box and vmware) and the exploit stage would fail each time. Therefore it is strongly recommended to use a computer with Linux with the base OS.

For this example I am using a Linux system with Ubuntu 20.04 installed

## Install Checkra1n

Instructions taken from [https://checkra.in/linux]

```
wget -O - https://assets.checkra.in/debian/archive.key | gpg --dearmor | sudo tee /usr/share/keyrings/checkra1n.gpg >/dev/null
echo 'deb [signed-by=/usr/share/keyrings/checkra1n.gpg] https://assets.checkra.in/debian /' | sudo tee /etc/apt/sources.list.d/checkra1n.list
sudo apt-get update
sudo apt-get install checkra1n
```

