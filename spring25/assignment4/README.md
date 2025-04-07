# Assignment 4: Extract QoS Metrics for Video Conferencing Application

## Objective: Due June 9th, 2024
In this assignment, you will infer QoE metrics for Google Meet video conferencing application from this [pcap][4]. Submit your code as a jupyter notebook on [Canvas][3].

## Description
In the [discussion section][1], we analyzed traffic from a video conferencing session from a P2P Messenger call where both participants were sending video and audio.

In this assignment, you are given another pcap file for a video conferencing call captured using Google Meet.

## Tasks
*  You have to convert the pcap to a CSV or a Parquet file for later analysis
* Isolate each of the RTP streams which corresponds to the audio or video stream in both the inbound or outbound direction from the local client. Filter out any RTP streams with less than 1000 total packets.
* Compute the RTP payload length for each RTP packet as:  
    $$ udp.len - 8 - 12 - (4 \times rtp.cc) - (rtp.ext \times (4 \times rtp.ext.len)) $$
* Group the packets for each RTP stream into frames. As mentioned in the section, the RTP timestamp value is the same for all packets in the same frame and changes for a new frame.
* For each frame, compute the size of the frame as the sum of the RTP payload lengths for each packet belonging to that frame.  
1. Plot the frame size (y-axis) for each stream against the frame number (x-axis)
2. Using this graph, comment on which RTP streams are for video and which are for audio.
3. For the streams that you identified as video, estimate the sampling rate using the method described in section. What value do you estimate for each RTP video stream, is it close to `90 kHz`? 
* Now, group the RTP packets for the video streams into one-second windows based on the RTP timestamp values. Use the sampling rate value of `90 kHz` for converting the timestamp values into seconds.
4. For each window (x-axis), graph the number of frames per second. Note that we are using a different inference method for fps than the one we used in discussion section because we have already estimated the sampling rate, and can more accurately infer the intended fps using just the RTP timestamps. Plot the FPS (y-axis) for each video stream over each window (x-axis). Please display each stream on a separate graph.
5. Finally, plot the frame-level jitter for each video stream using a sampling rate of `90 kHz`. The jitter calculation is shown below, taken from [Appendix A.8 in RFC 3550](https://datatracker.ietf.org/doc/html/rfc3550#appendix-A.8).
```
The inputs are r->ts, the timestamp from
the incoming packet, and arrival, the current time in the same units.
Here s points to state for the source; s->transit holds the relative
transit time for the previous packet, and s->jitter holds the
estimated jitter.  The jitter field of the reception report is
measured in timestamp units and expressed as an unsigned integer, but
the jitter estimate is kept in a floating point.  As each data packet
arrives, the jitter estimate is updated:

  int transit = arrival - r->ts;
  int d = transit - s->transit;
  s->transit = transit;
  if (d < 0) d = -d;
  s->jitter += (1./16.) * ((double)d - s->jitter);
```
Both `s->transit` and `s->jitter` can be initialized to 0. You should omit the first jitter estimate when both `s->transit` and `s->jitter` are still 0.  

Note that you can use any API (native Python, PySpark, pandas, etc.) to complete the tasks mentioned above. Also, remember to save your jupyter notebook by clicking the save icon button on the toolbar. You are allowed to use the code from the discussion sections for completing these tasks.

[1]: https://github.com/SNL-UCSB/cs176c-discussion-section/blob/master/spring25/week7/Inferring%20QoE%20Metrics%20for%20Real-time%20Applications.ipynb
[2]: https://en.wikipedia.org/wiki/Real-time_Transport_Protocol#Packet_header
[3]: https://ucsb.instructure.com/courses/26742/assignments/356504
[4]: https://drive.google.com/file/d/1h88pR-4owzWD9iNJjetRTZbFMkdd4MNE/view?usp=sharing
