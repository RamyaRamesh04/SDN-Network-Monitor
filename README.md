#SDN Project Report: Network Monitoring & Policy Control#
Ramya H R
PES1UG25CS835
1. Problem Statement
The objective of this project is to implement an SDN-based solution using Mininet and Ryu to monitor network utilization that is Bandwidth , Throughput etc. 
2. Setup and Execution
Step 1: Start the SDN Controller (Terminal 1)
cd ~/sdn_project
source venv/bin/activate
PYTHONPATH=./ryu python3 -m ryu.cmd.manager monitor_controller.py

Step 2: Launch the Network Topology (Terminal 2)
sudo mn --topo single,3 --controller remote,ip=127.0.0.1 --switch ovs,protocols=OpenFlow13
Test Scenarios (2 Success & 2 Failure)
Scenario 1: Normal Connectivity (SUCCESS)
Command: mininet> h1 ping -c 4 h2
Proof: 0% packet loss and Ryu log: [SUCCESS] ALLOWED.

Scenario 2: High Bandwidth Monitoring (SUCCESS)
Command:
mininet> h2 iperf -s &
mininet> h1 iperf -c h2 -t 15
Proof: iperf shows throughput; Ryu shows Mbps updates.

Scenario 3: Security Policy Block (FAILURE)
Command: mininet> h3 ping -c 4 h1
Proof: 100% packet loss and Ryu log: [FAILURE] SECURITY BLOCK.

Scenario 4: Service/Application Failure (FAILURE)
Command:
mininet> h2 pkill iperf
mininet> h1 iperf -c h2
Proof: Message 'Connection refused'. Proves service is down even if path is allowed.
Step 3: Inspect Flow Table (Terminal 3)
sudo ovs-ofctl -O OpenFlow13 dump-flows s1sudo ovs-ofctl -O OpenFlow13 dump-flows s13. 

Testing and Validation
Scenario 1: Normal Traffic (Success)
Action: h1 ping h2. Expected: Connectivity allowed.
Throughput is 0 because , there is no traffic.
 




Throughtput is varying because there is traffic 
 

Scenario 2: Bandwidth Monitor (Success)
Action: h1 iperf -c h2. Expected: Controller prints throughput in Mbps.
 
Scenario 3: Security Block (Failure)
Action: h3 ping h1. Expected: 100% packet loss (Denied Port 3).
 
Scenario 4: Service Downtime (Failure)
Action: h1 iperf -c h2 (with server killed). Expected: Connection refused.
 
4. Proof of Execution (Flow Tables)
Command: sudo ovs-ofctl -O OpenFlow13 dump-flows s1
 

