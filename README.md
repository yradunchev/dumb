**When you are debugging a problem for hours and hours, you suddenly realize,
"I bet it's something really dumb!" It often is. Therefore, we present...**

## _a list of dumb (and not so dumb) things to check_

**Update: 2009-11-25:** People keep referring to this as if it was something I
wrote to be funny. Damn it, this list is 100% true! These are all things that
have happened to me and made me think, "I better write that down to help me
remember it!" Even the last item!

**Layer 0 - PEBKAC**

  1. Make sure CapsLock is off. (Same for ScrollLock and NumLock) 
  2. Type it again (without using cut-and-paste) and see if you get the same results. (good way to find a typo) (or a unicode "whitespace" char) 
  3. Use cut-and-paste to copy that variable name (or URL, commmand line, etc.) to see if it was entered correctly. 
  4. Are the binaries really the ones you think are running? (Did you install in single user mode when /opt wasn't mounted? Can you check the md5 or sha1 checksum vs. a machine that is running properly?) 
  5. Check the file permissions. 
  6. Are you really on the host you think you are? 
  7. Are you doing the test from the right machine? Would the test be more effective from a different machine? 
  8. Does your test gear test what you think it tests? What happens when you run the exact same test on a known-good and a known-bad element? Do you get the results you expect both ways? 
  9. Is that a file, a directory, a hard link, a symbolic link, or a mountpoint? 
  10. Is the filename extention right? Should it be .php instead of .html? 
  11. Is the screen paused via Ctrl-S? (press CTRL-Q to find out) 
  12. You can get to the web site? Are you working in "off-line" mode? 

**Layer 1 - Physical**

  1. Are both ends of the (power/network/video/etc.) cable plugged in? 
  2. Is the cable plugged into the right jack? (Some jacks look the same: AUI and video, Some Sun's have a 'stealth' video jack that you aren't supposed to use, etc.) 
  3. Did you unplug and re-plug in the cable to make sure its in right? 
  4. It's too easy to answer "yes" when asked, "Is it plugged in." It's better to ask them to power it off, then power it back on OR ask them to "check both ends of the power cable" OR ask them if the power light is on, off, or blinking. (and if it's blinking... RUN!) 
  5. Does plugging a lamp into that outlet make it light up? (If you use a radio, be careful of radios with battery backups) 
  6. Is the laptop running off battery? (therefore obscuring a power problem) 
  7. Is the device driver you're trying to install for the device you actually have? 

**Layer 2 - Data Link**

  1. Is there a rogue DHCP server on the network messing with you? 
  2. Is the DHCP address pool exhausted i.e. out of addresses? 
  3. VPN dies at random times with the server getting "host unreachable" ICMPs? Your client's host firewall is blocking ARP and after the ARP cache entry expires (300 seconds) the router can't get packets to it. (Some vendors keep an ARP entry alive as long as they get packets from that address (Cisco) but most don't.) (I include this because I've seen it at 2 sites.) 

**Layer 3 - Network**

  1. Is the default route set? Is the netmask set correctly? 
  2. Six classes of bugs limit network performance.    
    * packet losses, corruption, congestion, bad hardware    
    * IP Routing, long round trip times    
    * Packet reordering    
    * Inappropriate buffer space    
    * Inappropriate packet sizes    
    * Inefficient applications    
  3. Is someone else also at that IP address? (Unplug the network cable and ping the address) 
  4. (firewall ruleset issues) If you move the machine to another IP address does it still happen? If you move the machine to a different subnet does it still happen? If you put a different machine at that IP address does it still happen? If you boot the same machine on a different OS (like a CD-ROM based Linux or FreeBSD) does it still happen? 
  5. Does the same thing happen when you specify the IP address instead of the hostname? (Hint: the lmhost or /etc/hosts may be overriding) 
  6. Linux: does /etc/hosts list an entry for localhost and for the machine's name itself? (haproxy and other systems for some reason want the do a lookup of their own hostname; https acts odd if it can't find itself) 

**Layer 4 - Transport**

  1. Traceroute from A to B. Traceroute B to A. Do they match up? 

**Layer 5 - Session**

  1. _SSH, SCP, L2TP, PPTP_

**Layer 6 - Presentation**

  1. Is the program reading the last line of the file? Is it being processed right? 
  2. Is there a (invisible) CTRL-M at the end of each line in the text file? 
  3. Does the text file end with a newline? 
  4. If a line ends with a "\" (continuation character) does it really end with one? An invisible space or TAB can result in shell saying "unexpected |"! 

**Layer 7 - Application**

  1. Is DNS configured right? Misconfigured DNS masks other problems and appears as bizarre problems that will send you looking everywhere except /etc/resolv.conf 
  2. Check the environment variables (Use "strings" on the binary to find out what they really are supposed to be). 
  3. Are you running the same copy of the script that you are editing? 
  4. Is the software looking for the same copy of the configuration file that you are? (Does the new release look in /etc/example2/example.conf instead of the old /etc/example.conf location?) 
  5. Any time I've seen `/dev/null` change permissions mysteriously it has been due to this situation: A program is written that outputs to file "foo" and then adjusts the permissions of the file using chmod. This works fine for a while. Then someone modifies the program to accept a flag that lets you change "foo" to any file name. This works fine for a while. Then someone decides they don't need the output and set the filename to `/dev/null`. If run as root, the program changes the permission of `/dev/null`. If nobody notices for a few days, the change seems "mysterious". If the program wasn't run as root the chmod will get an error... unless you are ignoring errors. To avoid this you should really use umask and the "mode" option to open() instead of chmod. 

**Layer 8 - User/Political**

  1. Is the user pressing RETURN when you think they are? (Are they pressing it at all?) 
  2. Is the user typing a "/" or a "\"? 
  3. Does the user know which is the lessthan (<) and which is the greaterthan (>) symbol? 
  4. "Did you get permission to run crack against that password file?" ... "Is it in writing?" 
  5. Is it the first of the month? Maybe there is a billing problem and something got shut off.
  6. Precisely time how often the bug happens or if it happens after a certain amount of time. Graph the outages on a timeline. What protocol typeouts could this be? ARP cache entries expire after 300 seconds. Routing protocols are often set to 5 or 15 minute updates. Once I graphed "network pauses" and saw they happened exactly every 10 minutes in addition to other times: The router's CPU was overloaded by a process that ran every 10 minutes AND any time a RIP update arrived. WIthout the graph, I wouldn't have noticed the "every 10 minutes" part.

**If All Else Fails**

  1. Did you remember to check this list? 

**Other useful lists**

  1. [ Simple Trouble Shooting Application Now Fixes Everything ](http://secretgeek.net/simple_checklist.asp)
  2. [Wikipedia's list of Non-functional requirements](http://en.wikipedia.org/wiki/List_of_system_quality_attributes) is a good way to jog your memory to brainstorm issues to consider

* * *

List compiled by Tom Limoncelli originally on the SAGE-Members mailing list
later by various sources. Thanks for the help from (in no particular order)
Shaun T. Erickson, Luke Hankins, Tom Ivar Helbekkmo, Bruce Hudson, Thomas
Leyer, Brad Knowles, Cat Okita, Tom Reingold, Carolyn Rowland, Randal L.
Schwartz, Glenn E. Sieb, Rajeev Agrawala, Steve Simmons, Scott Walters, Don
Marti, John Slee, Dan Tulovsky, Frank Wojcik, David Wolfskill. In December
2009 Joseph Kern reordered the list.

