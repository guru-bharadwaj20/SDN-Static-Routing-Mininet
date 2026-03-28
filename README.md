# Static Routing using SDN Controller
**Course:** UE24CS252B - Computer Networks  
**Name:** Guru R Bharadwaj  

## Problem Statement
Implement static routing paths using controller-installed flow rules in an 
SDN environment using Mininet and Ryu controller.

## Objectives
- Define static routing paths
- Install flow rules manually via SDN controller
- Validate packet delivery
- Document routing behavior
- Regression test: Ensure path remains unchanged after rule reinstall

## Topology
```
H1 (10.0.0.1) --|         |-- H3 (10.0.0.3)
                S1 -- S2 -- S3
H2 (10.0.0.2) --|         |-- H4 (10.0.0.4)
```

## Setup and Execution

### Requirements
- Ubuntu 24.04
- Mininet
- Ryu Controller (Python 3.11 venv)

### Step 1: Activate Ryu environment
```bash
source ~/ryu-env/bin/activate
cd ~/Desktop/sdn_project
```

### Step 2: Start Ryu controller (Terminal 1)
```bash
ryu-manager static_controller.py
```

### Step 3: Start Mininet topology (Terminal 2)
```bash
sudo mn --custom static_topo.py --topo statictopo --controller remote --switch ovsk,protocols=OpenFlow13
```

### Step 4: Test connectivity
```
mininet> pingall
mininet> h1 ping -c 3 h3
mininet> h1 ping -c 3 h4
```

### Step 5: Verify flow tables
```
mininet> sh ovs-ofctl dump-flows s1
mininet> sh ovs-ofctl dump-flows s2
mininet> sh ovs-ofctl dump-flows s3
```

### Step 6: Run regression test
```
mininet> sh python3 ~/Desktop/sdn_project/regression_test.py
```

## Routing Behavior

| Source | Destination | Path |
|--------|-------------|------|
| H1 | H3 | H1 -> S1 -> S2 -> S3 -> H3 |
| H1 | H4 | H1 -> S1 -> S2 -> S3 -> H4 |
| H2 | H3 | H2 -> S1 -> S2 -> S3 -> H3 |
| H2 | H4 | H2 -> S1 -> S2 -> S3 -> H4 |

## Expected Output
- `pingall` shows 0% packet loss
- Flow tables show static rules installed on all 3 switches
- Regression test shows PASS for all switches

## Tools Used
- Mininet - Network emulation
- Ryu - SDN Controller
- ovs-ofctl - Flow table inspection
- ping - Connectivity validation
- Wireshark - Packet capture and analysis

