# Assignment 3: Extract QoS Metrics for Video Streaming Applications

## Objective: Due May 23rd, 2025
In this assignment, you will extract QoS metrics for YouTube video streaming application from a given packet capture. This is **not** a group assignment. Submit your code as a jupyter notebook on [Canvas under Assignment 3][1].

## Tasks
* You are given a pcap file in which the host (with IP address `192.168.1.103`) is watching a YouTube video. You have to convert the pcap to a CSV or a Parquet file for later analysis
* Find the IP address for the YouTube streaming traffic using the DNS responses.
    * Note that, the shared packet capture is collected from a production satellite network. To save disk space, it's truncated and therefore, we can't see the TLS handshake headers. Hence, we have to rely on DNS responses alone to isolate YouTube streaming traffic.
* **[Extra Credit]** Write a query to print the top five source IP, destination IP pairs and sort them in descending order of 'bytes transferred'.
* As mentioned in the discussion sections, a YouTube video streaming session uses multiple flows for data transfer. Isolate the flow (identified by the 5-tuple) which sends the most amount of data to the host
* Run the chunk detection algorithm from the discussion section with the following constants:
    * `GET_thresh = 300` (bytes)
    * `DOWN_thresh = 300` (bytes)
    * `VIDEO_thresh = 800` (bytes)
* For each chunk, compute the following metrics:
    * TTFB (in seconds) 
    * Download Time (in seconds)
    * Download Speed (in Kbps)
    * Slack Time (in seconds)
    * TTFB Ratio = TTFB / (TTFB + 'Download Time')
* Plot the Cumulative Distribution Function (CDF) and the Line Plots for each of the computed metric.


Note that you can use any API (native Python, PySpark, pandas, etc.) to complete the tasks mentioned above. Also, remember to save your jupyter notebook by clicking the save icon button on the toolbar. You are allowed to use the code from the discussion sections for completing these tasks ([LINK][2]).

[1]: https://ucsb.instructure.com/courses/26742/assignments/356502
[2]: https://github.com/SNL-UCSB/cs176c-discussion-section/tree/master/spring25/week4/yt_chunk_analysis
