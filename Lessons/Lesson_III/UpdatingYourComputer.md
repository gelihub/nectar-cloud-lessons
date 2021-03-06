# Updating your new computer (20 min)

-- *Slide* --

## Updating your new computer

-- *Slide End* --

> Anna has managed to connect to her computer in the cloud: and knows a how to use the command line interface.
> Now she needs to apply the required commands to update her computer.

**Activity**

We are ready to update our web server. Try to execute the first command in the set handed to Anna:

-- *Slide* --

### Execute

```bash
apt-get update
```

* <span style="color:red">&#9632;</span> = I have errors!
* <span style="color:green">&#9632;</span> = I'm done!

-- *Slide End* --

    You are very much hoping for a sea of Red's here...
    You might have to call out

-- *Slide* --

### Help!

Again, something has gone wrong! You should be met with the message:

```bash
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?
```

-- *Slide End* --

Hold up a Green card when you've reached this error message.
And a Red card if you need help.

You are seeing this error because you are trying to perform system administration, and the ubuntu user you signed in
as is not the super user normally allowed to do system administration.

But don't panic!

-- *Slide* --

### Exercise

The `sudo` command (**s**uper **u**ser **do**) comes to your help. It allows the ubuntu user to run commands with
the security privileges of the super user.

Try to execute the command again, this time with `sudo`:

```bash
sudo apt-get update
```

-- *Slide End* --

You should now see a whole lot of gets scrolling by, as the operating system updates its lists of installed and
available software.

-- *Slide* --

### xkcd 149

<a href="https://xkcd.com/149/">
<img src="http://imgs.xkcd.com/comics/sandwich.png" title="The power of sudo" alt="Sandwich">
</a>

-- *Slide End* --

Think of `sudo` as being like a safety catch. When you find yourself using it, double check what you are about to
do!

-- *Slide* --

### Or:

System administrator to pharmacist:

*"Ephedrine, please."*

Pharmacist: *"I'm afraid I can't sell you that."*

*"Sudo ephedrine, please."*

Pharmacist: "Certainly sir!"

-- *Slide End* --

Now execute the command:

-- *Slide* --

### Exercise

```bash
sudo apt-get upgrade
```

If your system has software on it that needs an upgrade you will be met by a request to perform the upgrade. Something
like:

```bash
After this operation, 4,096 B of additional disk space will be used.
Do you want to continue? [Y/n]
```
-- *Slide End* --

In this case, reply, 'Y'.

Hold up a Green card when you've done this.
And a Red card if you need help.

Now we've replicated the steps Anna had to undertake in order to run the upgrade on her machine.

`apt-get` is the front end for a program called a package manager. Its rather like the appstore on your phone, and
allows you to add, remove, and upgrade applications.

To see how easy it is to use the package manager, install the fortune application.

-- *Slide* --

### Install fortune

```bash
sudo apt-get install fortune-mod
```

* <span style="color:red">&#9632;</span> = Help me!
* <span style="color:green">&#9632;</span> = I'm ready to move on...

-- *Slide End* --

-- *Slide* --

### Run fortune

```bash
fortune
```

* <span style="color:red">&#9632;</span> = Help me!
* <span style="color:green">&#9632;</span> = I'm ready to move on...

-- *Slide End* --

So what you've just done is used `apt-get`, the front end to the package manager to update the system
and then to add an application. Feel the power!

When you are finished working on your virtual machine, do the following:

-- *Slide* --

### Exercise

Type `exit`

You should see the following:

```bash
logout
Connection to <some_ip_number> closed
```
-- *Slide End* --

You have now closed the ssh connection to the remote machine. If you are not convinced, type `pwd` to see that your
terminal is now back on your local machine. The teleportation magic is over!

Hold up a Green card if you are back on your local machine.
And a Red card if you are not.

**Exercise**

Return to the security group in the dashboard and remove the ssh rule.

Now try to ssh into your virtual machine again.

What happens?

Hold up a Green card if you have managed to teleport into your remote machine..
And a Red card if you haven't.

I'm hoping to see a sea of Red!

**Exercise**

Now return to the security group and re-add a rule that allows ssh.

Try to ssh into your virtual machine again.

What happens?

Hold up a Green card if you have managed to teleport into your remote machine..
And a Red card if you haven't.

I'm hoping to see a sea of Green!

The takeaway: Security groups can stop you from accessing your server if they aren't configured properly.

It is a good idea to remove the ssh rule from the security group when you don't kneed it. This stops hackers from
trying to access your machine.

-- *Slide* --

### Question

You remove ssh from a security group shared by many servers.
Will you be able to ssh into any of the servers?

1. Yes
1. No
1. Only if there is another security group applied to the server
   that has ssh enabled.

-- *Slide End* --

**Answer C** A server can have multiple security groups, and if anyone of them allows a port, then that port is allowed.

## Man in the middle attacks

**Demo**

Ask for three volunteers. Ask the first to write a message to the second on a sticky note, then place it into an envelope.
And give it to you. Place the third person, (preferably male) in between the first and the second person.

You then carry it to the third person.

Make the point that as before, the envelope is the encryption wrapper. And only someone with the private key can
unwrap it and see it. But that the third person has actually convinced the first person that he is the second person.

Get the third person to read the message, perhaps change it a bit, then put it into a different coloured envelop,
and give it back to you.

You then take it to the second person, who writes a reply and puts in back into the envelop. You return it to the third
person, who opens it, reads it, and perhaps changes it again. Then you put it back into the original envelop, and take
it back to the orginator.

Make the point that the man in the middle has tricked the two on either side, and is listening to everything they say
and do.

Thank the volunteers.

How ssh stops this from happening is that in every communication with a server, the first thing the server sends
is a unique fingerprint to you. That was when you got asked the question:

-- *Slide* --

### Remember this?

```bash
The authenticity of host '144.6.225.224 (144.6.225.224)' can't be established.
RSA key fingerprint is d8:14:f5:85:5f:52:cb:f2:53:56:9d:b3:0c:1e:a3:1f.
Are you sure you want to continue connecting (yes/no)?
```

Um: Windows users: that's the: "Do you trust...." dialogue

-- *Slide End* --

When you replied 'yes' that fingerprint for the server was stored as a line in a file.

From this point on, whenever you connect to the server, that fingerprint checked against the entry in
the file. If they remain in sync, then everything is sweet.

But if they change, then ssh will refuse to give you a connection, showing an error:

-- *Slide* --

### There is possibly a man in the middle!

```bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
20:db:ea:9c:66:9a:ee:07:e6:1b:d8:a4:9d:d0:99:bc.
Please contact your system administrator.
Add correct host key in /root/.ssh/known_hosts to get rid of this message.
Offending RSA key in /root/.ssh/known_hosts:87
  remove with: ssh-keygen -f "/root/.ssh/known_hosts" -R mdcs-256-w14
ECDSA host key for mdcs-256-w14 has changed and you have requested strict checking.
Host key verification failed.
```

Um: Windows users: that's the: "Do you trust...." dialogue

-- *Slide End* --

The error message is very helpful: it even gives you the location of your file, and the
exact command required to fix it!

Note that earlier versions of the ssh software don't show the command required to fix this warning!

**Q**

How likely are you to see this error on the Research Cloud?

Hold up a Red sticky note if you think the answer is 'lots',
Otherwise hold up a Green sticky note.

**A**

You won't see it that much. The Greens have it correctly. But remember I mentioned that NeCTAR recycle IP numbers?
If you kill an instance and restart it, then the chances are that at some stage an instance will have the same IP
number as an old one. It's then that you'll see this error message.

-- *Slide* --

### The fix:

OSX:

```bash
ssh-keygen -R <hostname>
```

Where `<hostname>` can also be the IP number of your affected machine.

Um: Windows users: Just click the "Yes I Trust This Computer" button.

-- *Slide End* --

To recap: in this lesson we learnt how to update the software on the Ubuntu based system using the package manager.

Remember, updating the software keeps your server more secure and thus safe.

Each operating system type has its own package management program: so if you aren't on an Ubuntu system, you need to
find out what the update command is. Ask a friendly Linux person to show you the right command. And remember to
update often!

You also learnt to switch rules on and off on the security group, thus controlling access to the VM.