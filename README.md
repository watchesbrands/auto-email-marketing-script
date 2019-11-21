# auto-email-marketing-script
I have seen many of my friends sending promotional emails during the college fests. They typically have to send ~1500 mails. The other day somebody asked me if I was sending these promotional mails too. Well, of course I wasn’t. But it was then that I got to know that all these people were doing this stuff manually! So I asked him – “Don’t you get tired?” To that he replied – “Sure I do. But what choice do I have?” And that was it.

Just so you know, there IS another way. As a matter of fact, after reading this post, you may think that you’ve wasted too much time on something as trivial as sending emails. So, read on.

First comes the format in which you have got your email addresses. These are generally provided in a csv file (comma separated values file) or so I’ve been told. A CSV file can be opened using any spreadsheet program (such as Microsoft Excel). If you open a csv file using a text editor (such as notepad), you may find that it contains something of the following format:

Name, Email
Leonardo DiCaprio, leo@gmail.com
George Clooney, georgy@yahoo.com
Matt Damon, bourne@identity.com
The format is fairly obvious. The top row contains field names – in this case – Name and Email. The remaining rows are just data for either columns.

Now, because of certain reasons I chose that I will use python as the scripting language here. Don’t worry if you don’t know python though. It’s all been made in such a way that you can just copy-paste and it’ll work. The primary reason why I chose python is because of the inbuilt csv module that makes handling of csv files a piece of cake.

Now, to make it work:

Procedure:
Step 1:

First download and install python from the website. I am using python version 2.7.3 at the time of writing. Unless you know what you are doing, I suggest you install version 2.7 and don’t install version 3.2 etc.

Step 2:

Now, for sending the actual emails. There are a couple of ways to go about this. First, and the one that I have used in this post is, by using a gmail address. I will advise you not to use your permanent Gmail address for this. Make one or two new ones that you can use specially for sending bulk mails. This is just a safety measure. Google may think that you were spamming other people and may suspend your account. So, we are using a separate account for this purpose.

Among other methods, you can set up a mail server on your machine. But let me warn you that this process is somewhat difficult and takes time.

Another way would be to register for a free or paid SMTP server. These are generally much more flexible compared to gmail. But, finding a good one is usually a not easy. You are free to try of course.

Step 3:

Now, in a directory, make a file called message.txt that will contain the actual message to be sent. Also, in the same directory, keep your csv file. We will call this file email.csv in this post.

Step 4: In the same directory as before, make a file called sendmsg.py and copy the following code into it:

import csv import smtplib

from email.mime.text import MIMEText

import re

fp = open('message.txt', 'rb')
msg = MIMEText(fp.read())
fp.close()
msg['Subject'] = 'Subject goes here'
msg['From'] = 'sender.address@gmail.com'

server = smtplib.SMTP('smtp.gmail.com:587')
server.starttls()
server.login('sender.address@gmail.com', 'password')
email_data = csv.reader(open('email.csv', 'rb'))
email_pattern= re.compile("^.+@.+\..+$")
for row in email_data:
  if( email_pattern.search(row[1]) ):
    del msg['To']
    msg['To'] = row[1]
    try:
      server.sendmail('test@gmail.com', [row[1]], msg.as_string())
    except SMTPException:
      print "An error occured."
server.quit()
And that’s it! You’re done.

Explanation:
Those of you who understand python should have no trouble understanding this little snippet of code. For those of you who don’t know python, you should follow these steps to correctly configure this script:

1.On line 8, if your text file is named something else, then you should change the name message.txt to whatever the name is. For example, if your file is named hello.txt, the 8th line will look like:

fp = open(‘hello.txt’, 'rb’)
2.In lines 11 and 12 you should change the subject and the from fields. For example, your subject was “Important message“(quotes are not included in the subject) and your gmail address was “my.email.id@gmail.com” (quotes not included), then the 11th and 12th lines would look like:

msg['Subject’] = ‘Important message’ msg['From’] = ‘my.email.id@gmail.com’
3.Next, in line 16, replace the email address with the one you used in step 2 and replace the password with your own. For example, if your email id was “my.email.id@gmail.com” and you password was “mystrongpassword”, then the 16th line would look like:

server.login(’my.email.id@gmail.com’, 'mystrongpassword’)
4.Next, in line 17, if your csv file is named something else, say “data.csv“, then change the code so that it would look like:

email_data = csv.reader(open('data.csv’, 'rb’))
5.Now, in your csv file, you will have may have many column. The one I used has two column. Open your csv file using a text editor and count from the left to find out in which column there is the email address. Now subtract 1 from that. For example, if you csv file has the email addresses in the fourth column then you will subtract 1 from that and get 3. Now, replace the index of the row list (the number in between [ ] ) in line 20, 22 and 24 like so:

if( email_pattern.search(row[3]) ):
and

msg['To'] = row[3]
and

server.sendmail('test@gmail.com', [row[3]], msg.as_string())
And that completes the configuration. Now, you don’t have to sit long hours sending all that email!

Points To Note:
1.Keep in mind that using a gmail account is somewhat restrictive. You can only send 500 email/24 hours. For this purpose, I suppose you can make 2 – 3 accounts. To know about any other restrictions, you can always search online.

2.If you want to add attachments, I would suggest that you upload the attachment to some file sharing website like mediafire and provide the link in the email. It is possible to add attachments by adding some extra code but it would generally take more time as then the code will have to send the attachment to every email.

3.Also note that if there is an error in sending the mail, it will bounce back to your own email address.

Okay then. That will be it for now. If you have any question, feel free to post them in the comments section. Also, any opinions are welcome. Please like and share ~_~
