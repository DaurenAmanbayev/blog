---
layout:     post
title:      "Practical Device Emulation: Mimicking Hardware with snmpsim"
subtitle:   "A hands-on guide to capturing real SNMP snapshots for local testing"
date:       2025-09-20 10:00:00
author:     "Dauren Amanbayev"
header-img: "img/post-bg-02.jpg"
tags:       [SNMP, Networking, DevOps, Python]
---

<p>Testing monitoring templates often requires access to specific hardware that may be in production or physically unavailable. 
By using <strong>snmpsim</strong>, we can capture a "digital twin" of a real device and run it as a local daemon for safe, repeatable testing.</p>

<h2 class="section-heading">Step 1: Capturing the Device Snapshot</h2>

<p>The first step in our workflow is to gather all OID keys from the target device. We use the standard <code>snmpwalk</code> utility to create a comprehensive dump. 
To ensure the output is compatible with the emulator, we must use specific flags to format the data correctly.</p>

<p>From your terminal (Linux or WSL), run the following command to query your device (e.g., a Cisco ASR or Nexus):</p>

<ul>
    <li><strong>Numeric OIDs:</strong> Use <code>-On</code> to ensure all keys are saved as raw numbers rather than text labels.</li>
    <li><strong>Raw Values:</strong> Use <code>-beU</code> to strip units and extra formatting that might confuse the parser.</li>
    <li><strong>Full Walk:</strong> Targeting the <code>1.3.6</code> tree captures the majority of relevant networking and system metrics.</li>
</ul>

<pre><code># Example command to dump device data
snmpwalk -ObentU -v2c -c public 192.168.1.1 1.3.6 > hardware_dump.snmpwalk</code></pre>

<h2 class="section-heading">Step 2: Preparing the Test Folder</h2>

<p><strong>snmpsim</strong> maps community strings directly to the filenames in its data directory. This makes managing multiple test cases highly intuitive.</p>



<ul>
    <li><strong>Data Isolation:</strong> Create a dedicated directory (e.g., <code>snmp_data/</code>) to house your snapshots.</li>
    <li><strong>Community Mapping:</strong> Rename your captured file to the desired community string. For example, <code>cisco_asr.snmpwalk</code> will automatically be served under the <code>cisco_asr</code> community.</li>
    <li><strong>Permission Check:</strong> Ensure the directory is accessible by the user running the daemon to prevent indexing errors.</li>
</ul>

<h2 class="section-heading">Step 3: Running the Simulation</h2>

<p>Once your <code>.snmpwalk</code> file is placed in the folder, launch the <strong>snmpsim-command-responder</strong>. Point the <code>--data-dir</code> argument to your new folder. 
The daemon will scan the directory, index the OIDs, and start listening for incoming UDP requests.</p>

<p>This approach allows us to simulate high-load scenarios or complex interface configurations on our local workstations without touching the production network.</p>

<h2 class="section-heading">Technical Benefits</h2>

<p>By leveraging this local extraction and replay pipeline, we achieve enterprise-grade testing quality:</p>

<ul>
    <li><strong>Offline Development:</strong> Work on Zabbix templates or Prometheus exporters while commuting or in environments without VPN access.</li>
    <li><strong>Zero Risk:</strong> Experiment with destructive testing or high-frequency polling without impacting the performance of real hardware.</li>
    <li><strong>Consistent Baselines:</strong> Share the same <code>.snmpwalk</code> files across a team to ensure everyone is testing against identical data sets.</li>
</ul>

<p>The extracted data is served exactly as it was captured, preserving the original structure of the MIB tree and all associated values.</p>