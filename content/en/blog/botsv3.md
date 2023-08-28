---
title: "Boss of the SOC walkthrough"
description: Walkthrough of the Boss of the SOC v3 box from splunk!
date: 2022-10-18T13:03:09-05:00
image: images/bots.png
license: GPLv3
comments: true
draft: false
toc: true
categories: ["Hacking"]
tags: ["Splunk", "SIEM"]
author: "Said Neder"
---

# Starting point

1. Download Boss of the SOC VM from the [THM room](https://tryhackme.com/room/kimctf2022)
2. Import the VM with VirtualBox
3. In the host machine, navigate to [localhost:8000](localhost:8000)

## Mission of the CTF

The mission is to look for certain data using the SPL (Splunk Processing Language) being the flags, data.

Boss of the SOC is a blue-team CTF that helps you enhance your hunting and analysis skills.  
You will use Splunk and other tools to answer a variety of questions about security incidents that  
have occurred in a realistic but fictitious enterprise environment. It's designed to emulate how  
real security incidents look in Splunk and the type of questions analysts have to answer. The  
objective is to recreate the life of a security analyst facing down an adversary at all stages of an  
attack.

**Note: All the information you need to answer each question is present within the question  
itself. You just need to figure out how to create the proper Splunk search query that will  
get you the information you want.**

Using splunk's manual on searching & reporting we can achieve this task.

The Boss of the SOC guide is really helpful as well.

## Resources

This are the resources I used to learn more about splunk itself:

- https://lantern.splunk.com/Security/Use_Cases/Automated_Incident_Response/Identifying_inactive_user_accounts_in_AWS
- https://lantern.splunk.com/Security/Use_Cases/Behavior_Analysis_and_Machine_Learning/Detecting_AWS_cross-account_activity
- https://www.splunk.com/en_us/blog/security/hunting-with-splunk-the-basics.html?301=/blog/2017/07/06/hunting-with-splunk-the-basics.html&elqTrackId=9f3f8b4fc75f4506ac877748720ccc0f&elqaid=5067&elqat=2
- https://sroberts.medium.com/incident-response-is-dead-long-live-incident-response-5ba1de664b95
- https://www.youtube.com/watch?v=GWl-TuAAF-k&t=6s

## Keep in mind...

- Time is the most efficient filter
- Specify 1+ index values at start of search string
- Includes as many search terms as possible and make specific
- Filter as early as possible
- Inclusion is better than exclusion
- Use OR instead of wildcards, and if used don't use \* in the middle or beginning of string
- MATCHING TERM IS YOUR BEST FRIEND!!
- DISABLE SAMPLING!!!!

## Indexes

![Pasted image 20221016173021.png](images/image1.png)

This is where data is stored, being this data **events** that are stored in indexes, and these events are stored based on different factors like metrics, history, etc...

# Questions (Answered)

## Question 1:

**This is a simple question to get you familiar with submitting answers. What is the name of the company that makes the software that you are using for this competition? Answer guidance: A six-letter word with no punctuation.**

This is a simple question to know how answering questions work, being the answer "splunk"

## Question 2:

**List out the IAM users that accessed an AWS service (successfully or unsuccessfully) in Frothly's AWS environment? Answer guidance: Comma separated without spaces, in alphabetical order. (Example: ajackson,mjones,tmiller)**

` earliest=0 iam src_user="*"`

being earliest

- splunk_access
- bstoll
- web_admin
- btun

## Question 3:

**What field would you use to alert that AWS API activity have occurred without MFA (multi-factor authentication)? Answer guidance: Provide the full JSON path. (Example: iceCream.flavors.traditional)**

`awsapicall`

After searching the suggestion "awsapicall" search the "interesting fields" for the "mfa" keyword, being the result:

`userIdentity.sessionContext.attributes.mfaAuthenticated`

## Question 4:

**What is the processor number used on the web servers? Answer guidance: Include any special characters/punctuation. (Example: The processor number for Intel Core i7-8650U is i7-8650U.)**

`index="botsv3" xeon`

Here I used brute force by searching keywords of differents types of proccessors (this took a lot of time)

Result:
`E5-2676`

## Question 5:

**Bud accidentally makes an S3 bucket publicly accessible. What is the event ID of the API call that enabled public access? Answer guidance: Include any special characters/punctuation.**

`index=* sourcetype="aws:cloudtrail" eventSource="s3.amazonaws.com" eventName="PutBucketAcl"`

After learning a bit more how SPL works, we can now look in every index, the sourcetype "aws:cloudtrail" and look for events that happened in "s3.amazonaws.com", looking for the event "PutBucketPolicy"

Answer:
`ab45689d-69cd-41e7-8705-5350402cf7ac`

## Question 6:

**What is the name of the S3 bucket that was made publicly accessible?**

This is found in the same log as question 5, specifically in the JSON section "requestParameters.bucketName" being the answer `frothlywebcode`

## Question 7:

What is the name of the text file that was successfully uploaded into the S3 bucket while it was publicly accessible? Answer guidance: Provide just the file name and extension, not the full path. (Example: filename.docx instead of /mylogs/web/filename.docx)

We search in the bucket name files that end with .txt:
`index=* frothlywebcode .txt`

Answer: `OPEN_BUCKET_PLEASE_FIX.txt`

## Question 8:

What is the size (in megabytes) of the .tar.gz file that was successfully uploaded into the S3 bucket while it was publicly accessible? Answer guidance: Round to two decimal places without the unit of measure. Use 1024 for the byte conversion. Use a period (not a comma) as the radix character.

We use the same search as question 7:
`index=* frothlywebcode tar.gz`

And by reading the logs we can find a GET request, with a sucess code of 200, and the bytes downloaded: `3057116`

We transform those bytes to MiB to get the answer: `2.93`

## Question 9:

A Frothly endpoint exhibits signs of coin mining activity. What is the name of the first process to reach 100 percent CPU processor utilization time from this activity on this endpoint? Answer guidance: Include any special characters/punctuation.

First we start broadly: `index=* cpu`

Then we start playing with the selected fields, specially sourcetype, since that will bring tons of options, remember the assigment, to look for CPU processor utilization time, so you will be looking for the source type to be "PerfmonMk:Process" and look out for interesting fields, and always keep searching instances and keywords.

Answer: `chrome#5`

## Question 10:

When a Frothly web server EC2 instance is launched via auto scaling, it performs automated configuration tasks after the instance starts. How many packages and dependent packages are installed by the cloud initialization script? Answer guidance: Provide the number of installed packages then number of dependent packages, comma separated without spaces.

Google is your best friend, you can google "EC2 instance start configuration task" and you can get helpful documentation like this one: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html

Where it says:

> The cloud-init output log file captures console output so it is easy to debug your scripts following a launch if the instance does not behave the way you intended.

Reading documentation is the best way to understand something, remember that keywords are your best friend, in this case we will put our search query this way: `index=botsv3 sourcetype="cloud-init-output" dependencies`

Being the answer: `Install 7 Packages (+13 Dependent packages)` (7,13)

## Question 11

**What is the short hostname of the only Frothly endpoint to actually mine Monero cryptocurrency? (Example: ahamilton instead of ahamilton.mycompany.com)**

When finding hostnames it should pop into your mind DNS, the domain name system, so we put as our sourcetype "stream:dns", after that we can start playing with keywords, for example:
`index=botsv3 sourcetype="stream:dns" *coin*`

Being `*coin*` in between asterisk, for everything after or before the word, so we can take a deeper look.

Having a host that repeats itself very often making it the answer: `bstoll-l`

## Question 12

**How many cryptocurrency mining destinations are visited by Frothly endpoints?**

With the same search query as question 11, we can consult in interesting fields the "query" field, having this queries:

- coinhive.com
- ws001.coinhive.com
- ws005.coinhive.com
- ws011.coinhive.com
- ws014.coinhive.com
- ws019.coinhive.com

Meaning this 6 domains are visited by frothly endpoints, being the answer 6.

## Question 13

**Using Splunk's event order functions, what is the first seen signature ID of the coin miner threat according to Frothly's Symantec Endpoint Protection (SEP) data?**

We don't know what Symantec is, so time to google it and learn how to use it in our searches, so we search it, and remember to read all the selected fields.

By reading all the selected fields we know that there's tons of sourcetype that starts with symantec, so we are going to read all of them and as we are treating security we are going to use the "symantec:ep:security:file" sourcetype, and do a quick search looking for keywords.
`index=botsv3 sourcetype="symantec:ep:security:file"`

After that, we find chrome, the same application that ramped up our CPU by 100%, with crypto miners, so we need the ID, and there are only two, being one ID the answer: `30358`

## Question 14

**According to Symantec's website, what is the severity of this specific coin miner threat?**
With the same chrome#5 log as before we can now know by looking the CIDS_Signature_String that the attack is a "Web Attack: JSCoinMiner Download 8" and by googling it, you can now the answer that is: `medium`

## Question 15

**What is the short hostname of the only Frothly endpoint to show evidence of defeating the cryptocurrency threat? (Example: ahamilton instead of ahamilton.mycompany.com)**
We can now find through the vulnerability that is "Web Attack: JSCoinMiner Download 8" and we can find this log from the event log of windows.

```
LogName=Application SourceName=Symantec Network Protection EventCode=400 EventType=3 Type=Warning ComputerName=BTUN-L.froth.ly TaskCategory=The operation completed successfully. OpCode=Info RecordNumber=6049 Keywords=Classic Message=[SID: 30356] Web Attack: JSCoinminer Download 6 attack blocked. Traffic has been blocked for this application: C:\PROGRAM FILES (X86)\GOOGLE\CHROME\APPLICATION\CHROME.EXE
```

Being the answer: `BTUN-L`

## Question 16

**What is the FQDN of the endpoint that is running a different Windows operating system edition than the others?**

We search keywords as always, FQDN means Fully Qualified Domain Name, so we search
`index=botsv3 "windows 10" OR "windows 7"` since we already know we are dealing with windows devices, then we notice that BSTOLL-L has the windows 10 enterprise edition instead of the pro version as the others, but remember we need the FQDN, so it could be `bstoll.com` or similar, we search again: `index=botsv3 bstoll-l.*` and remember that MATCHING TERMS ARE YOUR FRIENDS!!

Answer: `bstoll-l.froth.ly`

## Question 17

**According to the Cisco NVM flow logs, for how many seconds does the endpoint generate Monero cryptocurrency? Answer guidance: Round to the nearest second without the unit of measure.**

We know we are treating with cisco related stuff, so we search `cisco*` and remember always to use source and sourcetype, and to google as well, so we search `index=botsv3 source=cisco*` and encounter three different sources: `cisconvmflowdata`, `cisconvmsysdata` and `cisconvmifdata`, pretty explanatory which source to use.

We search `index=botsv3 source=cisconvmflowdata *coin*` and we get some results, but we need to answer for how many seconds does the endpoint generate xmr, remember interesting fields section to know more about the search we need to use updating our search like this `index=botsv3 source=cisconvmflowdata *coin* sourcetype=syslog`

We encounter fst and fet meaning flow starting time and flow ending time, and by tons of googling this answer is a bit off since we need to substract the maximum time and the minimum time for encountering the average time, and with this search: `index=botsv3 source=cisconvmflowdata *coinhive* | stats max(_time) as maxtime min(_time) as mintime | eval difference=maxtime-mintime`

But is not the answer, which led me really confused and started seeing blogposts about it, and might be that in every instance the answer could be a bit off, so brute force it and we get the result of: `1666`

## Question 18

**What kind of Splunk visualization was in the first file attachment that Bud emails to Frothly employees to illustrate the coin miner issue? Answer guidance: Two words. (Example: choropleth map)**

When thinking of mail POP3, IMAP/SMTP comes to mind so we start searching by those keywords: `index=botsv3 sourcetype=*mail*`, but we see that nothing interesting comes as result so we change `*mail*` to `*smtp*`, and we get fun results, like this one:
`Bud, Uh... WTF ?!?!? Billy, Is this real? Jeremiah, Are these our customers? GH ________________________________ From: HyunKi Kim <hyunki1984@naver.com> Sent: Thursday, July 26, 2018 12:08 PM To: Grace Hoppy Subject: All your datas belong to us Gracie, We brought your data and imported it: https://pastebin.com/sdBUkwsE = Also, you should not be too hard Bruce. He good man [https://pastebin.com/i/facebook.png]<https://pastebin.com/sdBUkwsE> ( ) ) ) - Pastebin.com<https://pastebin.com/sdBUkwsE> pastebin.com`

Just from reading it gave me goosebumps but we know from reading is not the first email, so it's time to search with the username since we know the sourcetype, source and host, remembering that the "malware" that is the crypto miner was in the "brew" domain, but we can't forget mircrosoft exchange for emails so we search:

```
index=botsv3 earliest=0 (sourcetype=stream:smtp OR sourcetype=ms:o365:reporting:messagetrace)
bstoll@froth.ly
| stats count by Subject
```

Making statistics counting by subject, we find this: `Postmortem on our issue with brewertalk`, reading that does not sound fun so there's need to take a look upon the issue.

Now we look for this specific topic, so we need searching by subject: `index=botsv3 earliest=0 (sourcetype=stream:smtp OR sourcetype=ms:o365:reporting:messagetrace) bstoll@froth.ly subject="Postmortem on our issue with brewertalk"```

And we find a image002.jpg, and the attach_transfer_config is base64, so we need to copy the base64 that starts `/9j/4` and ends `//2Q==`, and paste it on the base64 image decoder![Screenshot from 2022-10-17 12-40-37.png](images/image3.png)

Being this chart a `Column Chart` the answer.

## Question 19

**What IAM user access key generates the most distinct errors when attempting to access IAM resources?**

We know IAM is from AWS Cloud, being Identity and Acess Managment, but we need to focus on the _distinct_ errors, so let's search broadly: `index=botsv3 sourcetype="aws:cloudtrail" *error*` and then narrow it down with interesting fields getting our search similar to this:
`index=botsv3 sourcetype="aws:cloudtrail" *error* *key* *resources* *access*`

Being this search for keywords like error, key, resources in AWS, we need to look out for errors when accessing IAM resources.

After that search I got information that is too broad, so let's look only `*errors*` in the destination "iam.amazonaws.com" to find out there are error codes, specially one with the name of "Access Denied", so we search by it:
`index=botsv3 sourcetype="aws:cloudtrail" *error* dest="iam.amazonaws.com" errorCode=AccessDenied`

In the JSON where the error message is `User: arn:aws:iam::622676721278:user/web_admin is not authorized to perform: iam:GetUser on resource: user web_admin` is our goal, so we search the access key ID and it is the: `AKIAJOGCDXJ5NW5PXUPA` the answer.

## Question 20

**Bud accidentally commits AWS access keys to an external code repository. Shortly after, he receives a notification from AWS that the account had been compromised. What is the support case ID that Amazon opens on his behalf?**

Receiving notifications, surely that is mail, so with the searches we made in other exercises, we do the same thing but with the "key" keyword and as the sender the "no-reply-aws*" since it's an automated email:
`index=botsv3 earliest=0 (sourcetype=stream:smtp OR sourcetype=ms:o365:reporting:messagetrace) *key\* sender_email="no-reply-aws@amazon.com"`

## Question 21

**AWS access keys consist of two parts: an access key ID (e.g., AKIAIOSFODNN7EXAMPLE) and a secret access key (e.g., wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY). What is the secret access key of the key that was leaked to the external code repository?**

This is very easy since it's in the same e-mail as the previous question, reading the email you will find this section:

```
     Case ID: 5244329601 Subject: Your AWS account 622676721278
     is compromised Severity: Urgent Correspondence: Dear AWS Customer, Your AWS Account is compromised! Please review the following notice and take immediate action to secure your account. Your security is important to us. We have become aware that the AWS Access Key AKIAJOGCDXJ5NW5PXUPA (belonging to IAM user "web_admin") along with the corresponding Secret Key is publicly available online at https://github.com/FrothlyBeers/BrewingIOT/blob/e4a98cc997de12bb7a59f18aea207a28bcec566c/MyDocuments/aws_credentials.bak
```

The answer being in this github repo: https://github.com/FrothlyBeers/BrewingIOT/blob/e4a98cc997de12bb7a59f18aea207a28bcec566c/MyDocuments/aws_credentials.bak

Answer: Bx8/gTsYC98T0oWiFhpmdROqhELPtXJSR9vFPNGk

## Question 22

**Using the leaked key, the adversary makes an unauthorized attempt to create a key for a specific resource. What is the name of that resource? Answer guidance: One word.**

This is pretty easy since we know all the fields:

```
index=botsv3 earliest=0 sourcetype="aws:cloudtrail" eventName=CreateAccessKey | spath "userIdentity.accessKeyId" | search "userIdentity.accessKeyId"=AKIAJOGCDXJ5NW5PXUPA| spath "userIdentity.userName" | search "userIdentity.userName"=web_admin
```

Giving us the answer:

```
User: arn:aws:iam::622676721278:user/web_admin is not authorized to perform: iam:CreateAccessKey on resource: user nullweb_admin
```

## Question 23:

**Using the leaked key, the adversary makes an unauthorized attempt to describe an account. What is the full user agent string of the application that originated the request?**

The same as the previous question, we know all the fields for searching:

```
index=botsv3 earliest=0 sourcetype="aws:cloudtrail" eventName=Describe* userName=web_admin| spath "userIdentity.accessKeyId" | search "userIdentity.accessKeyId"=AKIAJOGCDXJ5NW5PXUPA
```

We know the accessKeyId, and the event name is something with describe so that's why we use the wildcard `describe*`, getting the result

```
awsRegion: us-east-1
   errorCode: Client.UnauthorizedOperation
   errorMessage: You are not authorized to perform this operation.
   eventID: c077df0d-2435-4152-9127-09e579dd1fb2
   eventName: DescribeAccountAttributes
   eventSource: ec2.amazonaws.com
   eventTime: 2018-08-20T09:27:06Z
   eventType: AwsApiCall
   eventVersion: 1.05
   recipientAccountId: 622676721278
   requestID: f94dfb04-2d7b-40a8-b3cc-3664b9463db8
   requestParameters: { [[+]](http://localhost:8000/en-US/app/search/search?q=search%20index%3Dbotsv3%20earliest%3D0%20sourcetype%3D%22aws%3Acloudtrail%22%20eventName%3DDescribe*%20userName%3Dweb_admin%7C%20spath%20%22userIdentity.accessKeyId%22%20%7C%20search%20%22userIdentity.accessKeyId%22%3DAKIAJOGCDXJ5NW5PXUPA&display.page.search.mode=verbose&dispatch.sample_ratio=1&workload_pool=&earliest=0&latest=now&sid=1666063233.2202#)
   }
   responseElements: null
   sourceIPAddress: 82.102.18.111
   userAgent: ElasticWolf/5.1.6
```

Answer: `ElasticWolf/5.1.6`

## Question 24

**The adversary attempts to launch an Ubuntu cloud image as the compromised IAM user. What is the codename for that operating system version in the first attempt? Answer guidance: Two words.**

We know the fields but let's start searching broadly to know where is the keyword "ubuntu" involved:
`index=botsv3 earliest=0 *ubuntu*`

And we see tons of logs of apache but nothing important, now let's get the real search:

```
`index=botsv3 earliest=0 sourcetype="aws:cloudtrail" eventSource=*amazonaws.com (AKIAJOGCDXJ5NW5PXUPA OR web_admin`
```

Since we know that web_admin is the compromised user, we search for logs about it, but the keyword ubuntu just doesn't appear at all, but AMI does, that is a Amazon Linux instance for cloud, so let's search for that:

```
index=botsv3 earliest=0 sourcetype="aws:cloudtrail" eventSource=ec2.amazonaws.com (AKIAJOGCDXJ5NW5PXUPA OR web_admin) *ami*
```

And we get under `requestParameters.instancesSet.items[].imageId` the id of `ami-af79ebc0`, and we can search it on the Ubuntu Cloud Image Finder: https://cloud-images.ubuntu.com/locator/ to know the name of the image that is Xenial, but is not the full name so a quick google fu will get you the full name of Ubuntu Xenial Cloud that is Xenial Xerus being the answer.

## Question 25

**Frothly uses Amazon Route 53 for their DNS web service. What is the average length of the distinct third-level subdomains in the queries to brewertalk.com? Answer guidance: Round to two decimal places. (Example: The third-level subdomain for my.example.company.com is example.)**

Just reading the question I know it will make me create a good search query like the flow one, so let's start broadly:
`index=botsv3 earliest=9 *aws* AND *dns*`

To know different sourcetypes, like this one that looks promising: `aws:cloudwatchlogs`and with that sourcetype we know the source that is `lambda:DNS` so now we are talking, now we need to search the third level domain as the question ask, and we search like so:
`index=botsv3 earliest=0 sourcetype="aws:cloudwatchlogs" source="lambda:DNS" *.brewertalk.com`

After this searches we know this string enters into repetion: `Z149R7NEBZTKPN`

```
1.0 2018-08-20T15:08:14Z Z149R7NEBZTKPN users1.brewertalk.com AAAA NXDOMAIN UDP ICN51 52.78.247.225 -
```

Now I don't know regex at all, but we are going to need it since our simple searches aren't enough, so with tons of google fu, we are gonna search eveything that has Z149R7NEBZTKPN before the domain, meaning it should be in all of the logs.

Now my regex skills are really off the table so with tons of google-fu we can find already made regex for taking the average length of distinct third levels subdomains and take it for our own use, getting the final search like this:

```
index=botsv3 earliest=0 source=lambda:dns *.brewertalk.com
| rex field=_raw "Z149R7NEBZTKPN\s(?<query>[^\s]+)"
| rex field=query "\.?(?<third_level_subdomain>[^\.]+).brewertalk.com"
| dedup third_level_subdomain
| eval subdomain_length=len(third_level_subdomain)
| stats avg(subdomain_length)`
```

Being our result: 8.09, but we need to round it being it 8.10, the answer.

![Pasted image 20221017230721.png](images/image2.png)

Finally the BOSS OF THE SOC IS ME!

Thank you for reading.
