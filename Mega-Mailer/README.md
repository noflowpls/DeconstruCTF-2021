# Mega-Mailer
Category: Web

Points: 591

Description: We recently launched a mass email sender that can work with any SMTP server, but recently we have reports of information leaks and and trolling through our service. Can you find whats wrong with it ?

Url: http://very.uniquename.xyz:8080/

This `Mega Mailer` challenge is one of the hardest web challenges in this CTF. Me and my teammates need to spend a lot of times on this challenges. However This was a nice challenges.

![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-32-04.png)

When We go to website, we can see that we can put `SMTP Ip and Port` and We can see `Example CSV file` named `recipitents(1).csv`.

![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-32-10.png)

# Thinking The Vulnerability

First i am spending time with injecting `SQL injection` and `XSS`. We got `500` error everytime. And After a while one of my teammates found that we need to put real `SMTP server and port.` So i tested `smtp.google.com` in `Host` field  and put `25 in Port` field. and we submit it but get `500` error. What?🤔.

After a while, i understand that we need to put `recipitents(1).csv` to submit our input to ping to `smtp.google.com`. So tried to ping with the file and got success. we can ping but not get anything. But THis is our first success.

After while, i got a idea to run our `smtp` server locally and do `port forward` to make our local `smtp server` to be global. So we can get response from server in our machine.

For that i used free `vps container` service called [goorm](https://ide.goorm.io/) that can do port forward.

![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-38-54.jpg)
 
And i host my `smtp` server in vps locally by python3 `python3 -m smtpd -c DebuggingServer -n 0.0.0.0:45` and do port forward.
![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-39-15.png)

And i put the `vps machine port forwarded ip and port` in Host and port field and Test the `SSTI` in `Main Body` field.
![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-39-39.jpg)

And got response back with `4`. So that mean our `SSTI` injection is success. Bingo! 😋
![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-39-48.png)

After tesing many `SSTI` payloads. One payload from [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) worked.
![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-40-16.jpg)

The Payload is `{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('id').read() }}` and it worked and return with flag. Bingo! again 😋.
![CTF](https://github.com/ComdeyOverFlow/DeconstruCTF-2021/blob/main/Mega-Mailer/images/Screenshot%20from%202021-10-03%2005-40-23.png)

# Flag
dsc{819_8r41n_m41L3R}

# Thanks For Reading!
