# **T1595** Active Scanning

## **T1595.001** Scanning IP Blocks

Active scanning IP blocks/ranges using tools like nmap.

### **T1595.001** Procedure

#### Full Port Nmap Syn Scan

`-sS` for syn scan, `-sC` to run default nmap scripting enging (NSE) scripts, `-sV` to enumerate services running on ports (versions)

```shell
sudo nmap -sS -sC -sV -vv -p- <ip>/<range>
```

#### Top 100 UDP Scan

UDP is a stateless protocol therefore port scans can be very slow. Run a limited UDP scan with the top 200 ports list built into NMAP:

```shell
sudo nmap -sU -Pn -sV -sC -vv --top-ports=200 <IP>/<range>
```

#### Limited TCP Port Scan for Redteaming

This is a custom nmap command that limits the port list to the most common ports, along with those that are most likely to be vulnerable to RCE. This port scan can be used against both internal and external targets:

```shell

sudo nmap -sS -p 21,22,23,25,53,69,80,81,88,102,110,111,137,139,143,389,427,443,445,464,465,475,476,500,512,513,514,515,541,587,593,636,717,800,801,808,890,902,993,995,1080,1090,1091,1098,1099,1100,1128,1129,1150,1194,1433,1434,1444,1460,1461,1462,1521,1688,1801,1840,1993,1995,2000,2030,2049,2103,2105,2107,2222,2300,2382,2383,2483,2484,2500,2525,3009,3011,3200,3202,3204,3203,3268,3269,3299,3300,3302,3303,3304,3306,3343,3389,3392,3395,3396,3471,3472,3473,6553,3602,3801,3803,3823,3828,3843,3863,3867,3875,4000,4200,4222,4369,4447,4786,4800,4804,4848,5000,5001,5005,5013,5022,5023,5060,5061,5081,5150,5432,5500,5501,5504,5550,5555,5580,5600,5601,5672,5700,5900,5985,5986,6000,6001,6006,6007,6008,6029,6044,6057,6071,6076,6083,6099,6113,6129,6160,6162,6379,6400,6401,6402,6501,7000,7008,7022,7067,7070,7072,7095,7181,7274,7311,7319,7320,7431,7435,7443,7548,8000,8001,8002,8003,8007,8009,8010,8012,8016,8017,8019,8041,8043,8080,8092,8100,8101,8111,8116,8117,8201,8207,8211,8243,8443,8445,8686,8834,8991,8999,9000,9001,9002,9005,9007,9008,9009,9010,9011,9012,9020,9021,9022,9023,9024,9025,9026,9080,9084,9091,9092,9100,9101,9102,9389,9555,9600,9090,9991,10000,15672,20000,25000,27017,33060,40001,40002,45000,45001,47001,49152,49154,49155,49156,49171,50000,50001,50006,50500,56975,61616,61617 -vv -sC -n -T3 -sV -oA nmap-custom-tcp <IP>/<range>
```

## **T1595.002** Vulnerability Scanning

Vulnerability scanning can be conducted with a wide range of security tooling such as Tennable's Nessus, which is a well-known vulnerability scanner many pentesters use. However, scanning with Nessus on a red team would be very noisy. A better alternative would be Google's `Nuclei` project, which can test for an individual vulnerability or a suite of vulnerabilities if a vulnerability testing template has been created.


