+++
title = "WhatsApp Soccer Pick Up Game Bot"
date = "2020-02-15"
description = "A bot hosted on AWS to help me secure a spot in pick up soccer games"
tags = [
    "AWS", "Python", "Machine Learning", "Docker", "Selenium"
]
weight = 100
image = "soccer.jpg"
+++

The gist of this project came from the fact that I missed the registration time to be a committed member for Thursday indoor soccer games. There are 15 spots, first reserved for committed members and then if they are not all filled, some drop in players are selected to join. To become selected for drop-in, you have to respond to a specific text message that the organizer sends out the day before. The problem is the text happens at a random time, so you have to be really quick.

I missed out on it two weeks in a row by an average of 4 minutes, so I decided to use my programming skills to automate something and put it on AWS, so that I always get a spot. Using Python, Selenium, Docker and some AWS services, I got in a working text once before COVID-19 shut down everything. :(


Read more about it in my [blog post](../../p/soccer-games-and-python).