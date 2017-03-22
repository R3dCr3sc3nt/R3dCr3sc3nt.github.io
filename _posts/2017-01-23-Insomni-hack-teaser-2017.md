---
layout: post
title: Insomni'hack teaser 2017 
tags: [ctfwriteups, insomnihack, "2017"]
---

We solved 3 challenges in this CTF.

- [crytpoquizz](#cryptoquizz)
- [smarttomcat](#smarttomcat)
- [The Great Escape - part 1](#the-great-escape---part-1)

## cryptoquizz

> Hello, young hacker. Are you ready to fight rogue machines ? Now, you'll have to prove us that you are a genuine cryptographer.
>
> Running on quizz.teaser.insomnihack.ch:1031

#### Solution

    $ nc quizz.teaser.insomnihack.ch 1031

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    ~~ Hello, young hacker. Are you ready to fight rogue machines ?    ~~
    ~~ Now, you'll have to prove us that you are a genuine             ~~
    ~~ cryptographer.                                                  ~~
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    ~~ What is the birth year of Lars Knudsen ?

After `nc`-ing a few more times, it becomes clear that the prompt always asks for a birthyear of a notable cryptographer and/or mathematician, and then promptly disconnects. Since the connection only stays open for <= 2 seconds, this is challenge requires scripting.

I noticed that there was name repetition every ~15 times that I netcat-ed, so the list of cryptographers must be finite. I ran netcat 200 times and collected all of the unique names in a text file.

```
for i in {1..200}
do
  nc quizz.teaser.insomnihack.ch 1031 | grep year | cut -d " " -f 8- > names.txt
done
cat names.txt | sort | uniq > unique_names.txt
```

`unique_names.txt` ended up having 64 names. While a dynamic solution with API calls might have been more elegant, the fatest solution is to just use Google/Wikipedia. With a few minutes of googling, I had the birth year for all the cryptographers in the [list](unique_names.txt).

The final step requires setting up `stdin` and `stdout` pipes for netcat. Since this is cubersome in bash, I wrote a python script leveraging the [socket](https://docs.python.org/3/library/socket.html) module.

```
import socket
import time

def netcat(hostname, port, ppl):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((hostname, port))

    while 1:
        data = s.recv(8000)
        print(data)
        if data == "":
            break

        if not (data.startswith("\n\n~~ What") or data.startswith("\n~~ What")):
            continue

        time.sleep(0.1)
        person = data.strip().split(" ")
        person = " ".join(person[7:-1])        
        year = ppl[person]
        s.send(year)

    s.close()

hostname = "quizz.teaser.insomnihack.ch"
port = 1031
ppl = {}
with open("unique_names.txt", "r") as f:
    for line in f:
        name, year = line.rsplit(" ", 1)
        ppl[name] = year

netcat(hostname, port, ppl)
```

After 8 rounds in the loop, the flag is revealed.

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    ~~ OK, young hacker. You are now considered to be a                ~~
    ~~ INS{GENUINE_CRYPTOGRAPHER_BUT_NOT_YET_A_PROVEN_SKILLED_ONE}     ~~
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## smarttomcat

> Normal, regular cats are so 2000 and late, I decided to buy this allegedly smart tomcat robot
Now the damn thing has attacked me and flew away. I can't even seem to track it down on the broken search interface... Can you help me ?
>
> [Search interface](http://smarttomcat.teaser.insomnihack.ch/)

#### Solution

The linked interface contains 4 things: a brief text description, a form that takes lat/long coordinates as input, a picture of an animatronic tomcat, and a dynamic map that plots the points submitted in the form. This is the text description:

```
LOST CAT

Yellow with black stripes.
Has fled to an unknown location.
Wants to kill all humans.
```

Well, that's not very helpful. A quick look in the developer console reveals that the form is actually submitted asynchronously with an AJAX request. At this point, I started up BurpSuite to further analyze the form submission. Here is what the request looks like.

```
POST /index.php HTTP/1.1
Host: smarttomcat.teaser.insomnihack.ch
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Referer: http://smarttomcat.teaser.insomnihack.ch/
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 59
DNT: 1
Connection: close

u=http%3A%2F%2Flocalhost%3A8080%2Findex.jsp%3Fx%3D0%26y%3D0
```

The post body contains a request to a java service running on `localhost:8080`. Now the tomcat reference starts to make sense. This is an [Apache Tomcat](https://tomcat.apache.org/) server. After googling around for Apache Tomcat vulnerabilities, I find [this post](http://blog.opensecurityresearch.com/2012/09/manually-exploiting-tomcat-manager.html) about weak credentials in the manager interface. More googling reveals that the manager lives on `localhost:8080/manager/html`. I then modified the form submission post request to hit the manager (in the BurpSuite Repeater).

![Modified Post Request](https://github.com/R3dCr3sc3nt/Insomni-hack-teaser-2017/blob/master/smarttomcat/burpsuite_modified_post.png?raw=true)

Empty credentials don't work, so I try some typical defaults before eventually selecting a winner (tomcat:tomcat).

![Successful modified post](https://github.com/R3dCr3sc3nt/Insomni-hack-teaser-2017/blob/master/smarttomcat/burpsuite_successful_post.png?raw=true)

The flag is `INS{th1s_is_re4l_w0rld_pent3st}`.

## The Great Escape - part 1

> Hello,
>
> We've been suspecting Swiss Secure Cloud of secretely doing some pretty advanced research in artifical intelligence and this has recently been confirmed by the fact that one of their AIs seems to have escaped from their premises and has gone rogue. We have no idea whether this poses a threat or not and we need you to investigate what is going on.
>
> Luckily, we have a spy inside SSC and they were able to intercept [some communications](TheGreatEscape-3859f9ed7682e1857aaa4f2bcb5867ea6fe88c74.pcapng) over the past week when the breach occured. Maybe you can find some information related to the breach and recover the rogue AI.
>
> X
>
> Note: All the information you need to solve the 3 parts of this challenge is in the pcap. Once you find the exploit for a given part, you should be able to find the corresponding flag and move on to the next part.

#### Solution

Loading up the pcap file in wireshark shows a bunch of TCP communication along with a few interesting sections of data. The first thing that stuck out was an email sent by rogue@ssc.teaser.insomnihack.ch.
![Mail](https://github.com/grrr83/Insomni-hack-teaser-2017/blob/master/TheGreatEscape-part1/Mail.png?raw=true)

Ultimately though the key to solving this part of the challenge was found in an FTP-exchange wherein the user `bob` logged in and uploaded a file `ssc.key`:
![Ftp Data](https://github.com/grrr83/Insomni-hack-teaser-2017/blob/master/TheGreatEscape-part1/FTP.png?raw=true)

`ssc.key` turns out to be an RSA-key that can be used to decrypt the TLS-traffic.
![RSA Key](https://github.com/grrr83/Insomni-hack-teaser-2017/blob/master/TheGreatEscape-part1/RSA.png?raw=true)

Wireshark allows for decryption of TLS-traffic. Insert `ssc.key` in **Edit -> Preferences -> SSL**
![Decryption](https://github.com/grrr83/Insomni-hack-teaser-2017/blob/master/TheGreatEscape-part1/Wireshark.png?raw=true)

After digging through some of the now decrypted TLS-traffic I found the flag hiding in plain sight in an HTTP header.
![Flag](https://github.com/grrr83/Insomni-hack-teaser-2017/blob/master/TheGreatEscape-part1/Flag.png?raw=true)
