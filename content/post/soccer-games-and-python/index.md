+++
title = "Soccer games and Python"
description = "A diary: How I played more soccer with Python"
date = "2020-03-08"
keywords = ["blog", "project", "python"]
categories = ["blog", "project", "python"]
tags = [
 "blog",
 "project",
 "python",
 "soccer",
 "docker",
 "aws"
]
series = []
aliases = []
image = "soccer.jpg"

+++

I’ve been playing soccer for the past 15 years of my life. I love everything about the game and I enjoy playing it a ton. After high school ended, there wasn’t much opportunity for me to play and I got caught up in a lot of other things like school and work. However, I started dropping in at this indoor soccer event every Thursday. Because I missed the registration date, I am not a registered player and can only play if there aren’t enough registered players willing to play that day. But, there’s also other people like me, who are frequent drop-ins who also want to play. So how does the organizer decide which drop-ins can play and which ones must sit out that week? He does it on a first-come-first-serve basis on whoever replies quickest to his weekly message on WhatsApp.

I’m usually pretty fast but the organizer doesn’t do it at a set time every week. It can be on Wednesday night, Thursday morning or even Thursday afternoon and I can’t always be checking my phone waiting for that notification. Usually, I get in but this past week, I didn’t by one spot. One drop-in replied quicker by 1 minute and I was only 4 minutes off of the official message… so I decided to build an app that will instantly reply to it so that I am always or close to always first on the list.

So I knew what I had to do: find a WhatsApp API that can receive and send messages. I also realized I could use natural language processing (NLP) to see if the message is the official announcement message.

I only ever use WhatsApp for this soccer event, so I didn’t know about its features and why it is useful. The most important thing it does is end2end encryption. This makes it tough to access my messages through a service other than my phone. Nevertheless, the first thing I tried looking for was a WhatsApp API. I found out that Twilio has a nice API for WhatsApp but after playing around with it, I realized it was only for businesses and you had to pay to use it so I had to scrap that. Next, I looked on Github for WhatsApp APIs and found three promising repos. 
https://github.com/mukulhase/WebWhatsapp-Wrapper/
Could host in a docker container and do it online in AWS, Azure or GCP
https://github.com/Rhymen/go-whatsapp
I was actually going to use this because I want to work with GO, but because I also wanted to use NLP, and it’s easier to use NLP libraries in Python, I decided to use the Python wrapper
https://github.com/tgalal/yowsup
Decided not to go with this because I wanted to host this app online in a docker container and this wouldn’t do it

Next, I cloned the repo and tried to set up the environment. I ran into an error that I found was an issue on Github (https://github.com/mukulhase/WebWhatsapp-Wrapper/issues/846) that was posted 4 hours before I started this project. I struggled a little about how to go about solving this. I tried different things but it was my first time working with Selenium so I wasn’t too sure about it. I felt pretty stumped but after working on Github in my Microsoft internship and doing an open-source project, I knew that I probably could find a solution in pending PRs, and low and behold I found one: https://github.com/mukulhase/WebWhatsapp-Wrapper/pull/797. 

This is the end of day 1.

After playing around with it on my local machine, I realized that it would be incredibly nice to host this on the cloud so I don’t have to leave a computer on all the time. Because I work/worked at Microsoft, I wanted to use Microsoft Azure’s VM services. After looking at the free tiers and student credits, I looked at what AWS offered just to make sure I got the best deal. And although this is a questionable move, I decided to be a traitor and use AWS because of the absurd amount of usage I can get away with using its free tier.

I’ve never used VMs before nor worked with AWS EC2 on my own so this was quite an enjoyable learning experience for me. I set up the EC2 instance and used my SSH and linux skills I got from school. I downloaded all the necessary tools like Docker, GCC and pip and set up some shell scripts to make running the docker containers easier. Now, the docker containers are constantly running.

Now what did I change from the original repo? The while loop constantly checks for my soccer group chat’s unread messages. If the message is sent from the organizer, which I can check through his ID and has certain key words that he always says, then I reply back immediately that “I am available to play as a drop-in”. This will surely secure me a spot every time. 

I’ve only been tracking his messages since the start of 2020, but here is every one of them so far:
Folks, sorry for being late.
Please let me know if you can attend to tomorrow’s Thursday night soccer session.  Thank you.

Folks, it is Thursday night soccer again.  Please let me know of your attendance for tomorrow night.  Thank you.

Folks, my apology for the late notice.  
Please let me know if you can attend to this coming Thursday night soccer session.  
Thank you.

Folks, please let me know of your attendance for Tomorrow night’s soccer session.  Thank you.

Folks, please let me know your attendance to tomorrow night’s soccer session.  Thank you.
Please note that the responses from yesterday will not be counted as formal request for the drop ins.  Your respond after this text will count as valid.  TQ

Folks, my apology for late notice.  
Please let me know of your attendance to tomorrow night’s soccer session.  Thank you.

Folks, it is Thursday night soccer again.  Please let me know your attendance to tomorrow’s session.  Thank you.

In the future, I plan on using some Natural Language Processing (NLP) APIs to easily assess the message and see if it is the “availability inquiry” message. For now, I just make sure the words “folks” and “soccer” are in the string then the message sends… not the smartest but it gets the job done.

Stuff I can still add
Add scripts so our docker containers run right when EC2 container is launched
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html
Scheduler to run EC2 Instance:
Start it on Tuesday mornings and stop it on Thursday nights
https://aws.amazon.com/premiumsupport/knowledge-center/start-stop-lambda-cloudwatch/
https://aws.amazon.com/premiumsupport/knowledge-center/stop-start-instance-scheduler/
Set up cloudevents so I don’t go over free tier
https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html#free-budget
https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/invoice.html
https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/tracking-free-tier-usage.html#free-tier-table
https://console.aws.amazon.com/billing/home#/
NLP
https://docs.microsoft.com/en-us/azure/architecture/data-guide/technology-choices/natural-language-processing

To make this project even better, I thought about how I could run all these docker containers instantly on start up. I found this [article](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) and it looked pretty easy but it was more work than I wanted. I gave up on it when things weren’t working so well. Then I found out about /etc/rc.local which apparently is a bash script that runs on every reboot. It didn’t work for me either. Then I found out about cronjobs from [this stack overflow post](https://superuser.com/questions/685471/how-can-i-run-a-command-after-boot). I already wrote a bunch of bash scripts to make it easier for me, so now I just call all of them in cronjob so that when my EC2 instance is launched all those scripts run. However, one of my scripts that uses a lot of relative path syntax doesn’t run, so I’ll look into it tomorrow.

This is the end of day 2.

Changed $(pwd) to a full path but it still didn’t work so I decided I want to log the output of the script to a file, which I did with this stack overflow [post](https://askubuntu.com/questions/420981/how-do-i-save-terminal-output-to-a-file) and I found out that the error was that “the input device is not a tty”, and so I found this stack overflow [post](https://stackoverflow.com/questions/40536778/how-to-workaround-the-input-device-is-not-a-tty-when-using-grunt-shell-to-invo) and it’s now fixed.
Now I have the docker containers running right on boot, but the get_unread_messages_from_chat_id are not getting the unread messages that were sent before the docker containers are spun up. It’s not that big of a deal right now, but will be something I look into.

Next up, I wanted to automate the start and stop of the EC2 instances. The organizer says he make the announcement on Wednesday, but it is rarely consistent recently, so I want it to start up at Tuesday 11:59PM and stop Thursday at 8:00PM, which is when the session officially starts. This will also make it so that my EC2 instances are not constantly running and possibly costing me money although the free tier is very forgiving.

I followed this [guide](https://aws.amazon.com/premiumsupport/knowledge-center/start-stop-lambda-cloudwatch/) and learned a lot more about IAM rules. Albeit, I used them a lot while working at Amazon, I didn’t really understand them because I didn’t need to but wow they are cool! The Python SDKs for running and stopping EC2 instances is so simple. Testing on is super easy on Lambda as well! Setting up events with cronjobs was super easy as well on CloudWatch, so I’m super stoked about all this. Now, my EC2 instance starts up at midnight on Wednesday and shuts down Thursday night.

Can add a “break” to loop once I send the message to the group so less resources are used. Can also have exit script to kill all docker shit once the docker container closes?
https://stackoverflow.com/questions/10541363/self-terminating-aws-ec2-instance

Figured it out. End program by calling break on While True loop once a condition is met. Then in my run.sh script, I will have a `docker wait soccer` to block until the container exits, then I call `sudo halt` to stop the EC2 Instance.

This is the end of day 3.

Now, I really wanted to add some machine learning/AI, in particular some natural language processing. I don’t have a lot of data but I felt like it would be better to do some intent classification rather than my original implementation. I wanted to use Azure products because I had a coffee chat with someone working in cognitive systems, but I was also open to using some AWS services. I found an [article by Microsoft](https://docs.microsoft.com/en-us/azure/architecture/data-guide/technology-choices/natural-language-processing) and tried to see the best service to use. After some research, I decided upon two possible services I could use: Microsoft LUIS and AWS Comprehend. I tried LUIS first and really liked it. It made it easy to add data and to identify them with an intent then train them. I also checked out AWS Comprehend to see how it is, but it wasn’t for my use case of analyzing short texts. Plus, I had to upload them to S3, which I thought was cumbersome. After playing around with LUIS, and creating some intents with common messages that the organizer sends, I trained the model and got the endpoint. I used a simple [Pypi package](https://pypi.org/project/luis/) to create a LUIS client that can easily send texts for analysis and receive the analysis. So now, whenever I get a text from the group chat and from the organizer, I send it to LUIS to analyze it. If the intent is “AttendanceForSoccer”, the bot sends a message to the group chat saying that I am available to play, and gets ready to stop the EC2 Instance.

It is finally complete. The only thing I really have to do now is make sure I don’t go above my free tier for LUIS and AWS.

So here is my project that gets me into my soccer games that I always have to drop in for.
Uses WebWhatsAPI as a client for a WhatsApp instance
Uses docker containers to run the selenium browser, and run the python script that can interact with the WhatsApp instance
Used shell scripts and crontab to automatically run the docker containers when the EC2 instance starts
Docker wait to block until the python script container ends and “sudo halt” to stop the container once the message is sent
Uses AWS EC2 to host the docker containers and AWS Lambda and AWS CloudWatch to automatically start and stop the EC2 instance on a particular schedule
Uses Microsoft LUIS (Language Understanding Intelligence System) to classify the organizer’s intent so the bot can determine whether to send the “I am available” message or to not respond at all because it is irrelevant

So, the first week, it didn’t work. Looking at the monitoring graphs, it looks like the CPU utilization went to 99% at 7AM and then it had close to 0% the entire time. I had to manually reply two minutes after the message sent while I was asleep on the subway. Two drop-ins wanted to join quicker, but due to the coronavirus, I think I’ll still have a spot.

So I added better logging, so that on every boot, there is a log that logs the entire output of all the scripts. Furthermore, I added a cloudwatch event that will reboot the EC2 instance if the CPU utilization goes above 85%.

Update:
It worked the week after and then soccer is now cancelled due to the coronavirus!
