# Check Wiki First #
Before sending an email to our mailing list, check to see if your question has already been answered by one of the following:

[Help](Help.md)

[FAQ](FAQ.md)

<a href='http://code.google.com/p/security-onion/w/list'>Wiki</a>

# Moderation #
Please keep in mind that our Google Groups are moderated, so your email will have to be approved before it is published to the list.  If at first you don't see your email appear in the mailing list, there is no need to re-send your email.  It has been queued and will be approved if appropriate.

# Etiquette #
Please be courteous and respectful.  Disrespectful emails can result in being banned from the Google Group.

# Questions/Problems #

## Start a new thread instead of replying to an old one ##
Please search the mailing list to see if you can find similar issues that may help you.  However, please do not reply to old threads with your new issue.  Instead, please start a new thread and provide a hyperlink to the related discussion at https://groups.google.com/forum/#!forum/security-onion.

## Provide sufficient technical info ##
In order to be as effective and efficient as possible, please consider the following when posing your question/problem to the group:
http://www.chiark.greenend.org.uk/~sgtatham/bugs.html

## Include sostat-redacted output ##
Please run the following command:
```
sudo sostat-redacted
```

There will be a lot of output, so you may need to increase your terminal's scroll buffer OR redirect the output of the command to a file:
```
sudo sostat-redacted > sostat-redacted.txt 2>&1
```

sostat-redacted will automatically redact any IPv4/IPv6/MAC addresses, but there may be additional sensitive info that you still need to redact manually.

Attach the output to your email in plain text format (.txt) OR use a service like http://pastebin.com.

# Mailing Lists #
## Security-Onion ##
The security-onion mailing list is for announcements and general user support questions:

http://groups.google.com/group/security-onion

## Security-Onion-Testing ##
The security-onion-testing mailing list is for those who'd like to help test upcoming versions of Security Onion packages before they are released:

http://groups.google.com/group/security-onion-testing