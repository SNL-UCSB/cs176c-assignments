# Assignment 1: Due April 21<sup>st</sup>, 2021

## Turn-in 
Submit a pdf report on the [Gauchospace submission](https://gauchospace.ucsb.edu/courses/mod/assign/index.php?id=116982) that has answers to the questions in the sections below. Please keep your answers as precise and concise as possible. Note that there are a total of `16` questions given in bullet points. Write your answers in sections just as the questions appear in sections.

## Overview
In this assignment, we'll investigate the `802.11` wireless network protocol. Before beginning, you might want to re-read Section 7.3 in the text. Since we'll be delving a bit deeper into `802.11` than is covered in the text, you might also want to check out [A Technical Tutorial on the 802.11 Protocol](http://www.sss-mag.com/pdf/802_11tut.pdf), by Pablo Brenner (Breezecom Communications). And of course, there is the 802.11 standard itself, [ANSI/IEEE Std 802.11, 1999 Edition (R2003)](http://gaia.cs.umass.edu/wireshark-labs/802.11-1999.pdf). In particular, you may find Table 1 on page 36 of the standard particularly useful when looking through the wireless trace.  We will provide a trace of captured `802.11` frames for you to analyze which you are to use to answer the following questions. 

## Getting Started
Download [this trace file](https://drive.google.com/file/d/1iiRV27on7lzv73OOTjw2lTewc3SNaZtI/view?usp=sharing) `Wireshark_802_11.pcap`. It was collected using AirPcap and Wireshark running on a computer in the home network of one of the authors of the textbook. The network consisted of a `Linksys 802.11g` combined access point/router, with two wired PCs and one wireless host PC attached to the access point/router. There were access points in neighboring houses available as well, and can be overheard in the trace. In this trace file, we'll see frames captured on channel 6. Since the host and AP that we are interested in are not the only devices using channel 6, we'll see a lot of frames that we're not interested in for this lab, such as beacon frames advertised by a neighbor's AP also operating on channel 6. The wireless host activities taken in the trace file are:
  * The host is already associated with the `30 Munroe St` AP when the trace begins.
  * At `t = 24.82`, the host makes an HTTP request to `http://gaia.cs.umass.edu/wiresharklabs/alice.txt`. The IP address of `gaia.cs.umass.edu` is `128.119.245.12`.
  * At `t=32.82`, the host makes an HTTP request to `http://www.cs.umass.edu`, whose IP address is `128.119.240.19`.
  * At `t = 49.58`, the host disconnects from the `30 Munroe St` AP and attempts to connect to `linksys_ses_24086`. This is not an open access point and so the host is eventually unable to connect to this AP.
  * At `t=63.0` the host gives up trying to associate with the `linksys_ses_24086` AP, and associates again with the `30 Munroe St` access point. 

Once you have downloaded the trace, you can load it into Wireshark and view the trace using the File pull down menu, choosing Open, and then selecting the `Wireshark_802_11.pcap` trace file. 

## Beacon Frames
Recall that beacon frames are used by an `802.11` AP to advertise its existence. To answer some of the questions below, you'll want to look at the details of the `IEEE 802.11` frame and subfields in the middle Wireshark window. 
  * What is the SSID of the access point that is issuing most of the beacon frames in this trace?
  * What are the intervals of time between the transmission of the beacon frames from the `30 Munroe St`. access point? (Hint: this interval of time is contained in the beacon frame itself).
  * What (in hexadecimal notation) is the source MAC address on the beacon frame from `30 Munroe St`? Recall that the source, destination, and BSS are three addresses used in an `802.11` frame. For a detailed discussion of the `802.11` frame structure, see section 7 in the `IEEE 802.11` standards document (cited above).
  * What (in hexadecimal notation) is the destination MAC address on the beacon frame from `30 Munroe St`?
  * The beacon frames from the `30 Munroe St` access point advertise that the access point can support four data rates and eight additional *extended supported rates*. What are these rates?

## Data Transfer
Since the trace starts with the host already associated with the AP, let's first look at data transfer over an `802.11` association before looking at AP association/disassociation. Recall that in this trace, at `t = 24.82`, the host makes an HTTP request to `http://gaia.cs.umass.edu/wiresharklabs/alice.txt`. The IP address of `gaia.cs.umass.edu` is `128.119.245.12`. Then, at `t=32.82`, the host makes an HTTP request to `http://www.cs.umass.edu`. Make sure that your Time Display Format is *Seconds Since Beginning of Capture* to make it easier to find these packets.
  * Find the `802.11` frame containing the SYN TCP segment for this first TCP session (that downloads alice.txt). 
  * What are three MAC address fields in the `802.11` frame? 
  * In this frame, write the MAC address (in hexadecimal representation) corresponding to the wireless host, the access point and the first-hop router.
  * What is the IP address of the wireless host sending this TCP segment? 
  * What is the destination IP address? 
  * Does this destination IP address correspond to the host, access point, first-hop router, or some other network-attached device? Explain.

## Association/Disassociation
Recall that a host must first associate with an access point before sending data. Association in `802.11` is performed using the `ASSOCIATE REQUEST` frame (sent from host to AP, with a frame type 0 and subtype 0) and the `ASSOCIATE RESPONSE` frame (sent by the AP to a host with a frame type 0 and subtype of 1, in response to a received `ASSOCIATE REQUEST`). For a detailed explanation of each field in the `802.11` frame, see page 34 (Section 7) of the `802.11` spec at `http://gaia.cs.umass.edu/wireshark-labs/802.11-1999.pdf`.
  * What two actions are taken (i.e., frames are sent) by the host in the trace just after `t=49.5`, to end the association with the `30 Munroe St` AP that was initially in place when trace collection began? (Hint: one is an IP-layer action, and one is an 802.11-layer action).
  * An `ASSOCIATE REQUEST` from host to AP, and a corresponding `ASSOCIATE RESPONSE` frame from AP to host are used for the host to associated with an AP. At what time is there an `ASSOCIATE REQUEST` from host to the `30 Munroe St` AP? When is the corresponding `ASSOCIATE REPLY` sent? (Note that you can use the filter expression `wlan.fc.subtype < 2` and `wlan.fc.type == 0` and `wlan.addr == IntelCor_d1:b6:4f` to display only the `ASSOCIATE REQUEST` and `ASSOCIATE RESPONSE` frames for this trace.)
  * What transmission rates is the host willing to use? The AP? To answer this question, you will need to look into the parameters fields of the `802.11` wireless LAN management frame.

## Other Frames
This trace contains a number of `PROBE REQUEST` and `PROBE RESPONSE` frames. 
  * What are the sender, receiver and BSS ID MAC addresses in these frames? 
  * What is the purpose of these two types of frames? (To answer this last question, you'll need to dig into the online references cited earlier in this lab).

