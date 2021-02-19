+++
title = "UBC Course Auto Registrar"
date = "2020-01-05"
description = "A Python project to automatically get me into UBC courses once a spot is available."
tags = [
    "UBC", "Python", "School", "Docker", "Selenium"
]
weight = 100
image = "ubc.jpg"
+++

{{< figure src="ubc.jpg" width=800rem title="UBC classroom">}}

Using Python, Selenium and BeautifulSoup4, I was able to create a bot that automatically registered me in UBC courses once a spot was available. There are other services similar to this one out there, but I wanted to be a little different. Almost all the other services available (paid and free) only notify you once a spot is available. I wanted to actually automatically register you in it. I also added notifying through AWS SNS just in case it failed for some reason.

Using BeautifulSoup4, I parsed the course page to check if a seat was available (on a loop), and if there was a seat available, I used Python and Selenium to log in, go to the page and register you into the course.


[Github](https://github.com/vandyliu/get-me-in-a-course)