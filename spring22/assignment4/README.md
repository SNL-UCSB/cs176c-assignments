# Assignment 4: Extract QoS Metrics for Video Conferencing Application

## Objective: Due June 7th, 2022
In this assignment, you will extract QoS metrics for Google Meet video conferencing application from a given packet capture. This is **not** a group assignment. Submit your code as a jupyter notebook on [the Gauchospace submission][3].

## Description
In the [discussion section 7][1], we analyzed traffic from a video conferencing session where the primary host (site of packet capture) was in a two minute video call over Google Meet with some other end-host. The video call was muted, and the data is unidirectional such that, the primary host received video packets from the Google Meet's central server.

In this assignment, you are given another pcap file for a two minute video call captured using the same setup as in discussion section 7.

## Tasks
*  You have to convert the pcap to a CSV or a Parquet file for later analysis
* Isolate the flow which corresponds to the video stream. Same as in the discussion section, the RTP packets with video payload have a payload type value of `96`.
* Compute the RTP payload length for each RTP packet containing a video payload.
* Group the video payload lengths into frames. Recall that packets may arrive out of order at the end-host. Therefore, make sure to reorder packets before grouping them into frames. As mentioned in the section, you can identify the frame boundaries using either of the following information:
    * The Marker bit (rtp.marker) in the RTP header denotes the ending of a frame.
    * The RTP timestamp value is the same for all packets in the same frame and changes for a new frame.
* For each frame, report the number of packets, number of bytes and average bytes per packet. Plot each metric as a CDF.
* Now, group the video payloads into one-second windows based on the RTP timestamp values. Use the sampling rate value of `90 kHz` for converting the timestamp values into seconds.
* For each window, report the total number of packets, number of bytes and the number of frames. Plot each metric as a CDF.
* Draw a line plot where x-axis is the Window ID and the y-axis is number of frames.
* Report the maximum and minimum frame rate for this video conferencing session.


Note that you can use any API (native Python, PySpark, pandas, etc.) to complete the tasks mentioned above. Also, remember to save your jupyter notebook by clicking the save icon button on the toolbar. You are allowed to use the code from the discussion sections for completing these tasks.

[1]: https://github.com/SNL-UCSB/cs176c-discussion-section/tree/master/vca_analysis
[2]: https://en.wikipedia.org/wiki/Real-time_Transport_Protocol#Packet_header
[3]: https://gauchospace.ucsb.edu/courses/mod/assign/view.php?id=2094022

