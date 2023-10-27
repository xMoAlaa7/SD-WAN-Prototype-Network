# SD-WAN Prototype Network

To view the deployment documentation, contact the author <a href="mailto:moalaaeldeen7@gmail.com">here</a> and to view a demo of <span style="color:red"><b>SOME</b></span> of project's results, watch [this](https://youtu.be/zl80-LDKbZk) video.

## Contributors:

- <a href="mailto:moalaaeldeen7@gmail.com">Mohamed Ahmed Abdelsalam</a>

- <a href="mailto:moalaaeldeen7@gmail.com">Laila Ahmed Maged</a>

- <a href="mailto:moalaaeldeen7@gmail.com">Abdelrahman Hesham Mohamed</a>

- <a href="mailto:moalaaeldeen7@gmail.com">Rahma Waleed Saeed</a>

## Project Outline:

- The project is a a prototype infrastructure consisting of two sites (Data Center and Service) that are connected over the actual Internet via Cisco’s SD-WAN Architecture with the aid of concepts as Bridging, Port Forwarding, NAT, and DDNS.
- The WAN Edge Routers and vSmart Controllers are managed using the GUI provided by the vManage NMS -after the initial manual on-boarding via the CLI of each device- by using Templates and Policies.
- The infrastructure incorporates Cisco’s Three Layer Hierarchical Model over its private network on both sites and applies protocols and concepts such as OSPF, HSRP, VLANs, and Inter-VLAN Routing.
- The Datacenter site includes a Windows Server that provides Services (DHCP, DNS, WDS, etc.) and Administration (GPOs, Users, Groups, and OUs management) over both sites.
- Each site includes a FortiGate Firewall (configured with policies and synced with the LDAP server) and has its WAN Edge Router equipped with an IPS for security purposes.

## Network Topology:

<p align="center">
  <img src="images/SD-WAN Network Topology.png" width="600" height="auto">
</p>

### Site 100:

Site 100 contains the controllers and includes two host computers running behind two different gateways with different Internet subscriptions (therefore two public IP addresses). Behind the first host computer lies the vBond orchestrator and behind the second lies both the vManage NMS and the vSmart controller all running as VM instances.

This method of deployment for the controllers ensures that the vBond orchestrator is connected in a similar manner to all other controllers and WAN edge routers and has its own public IP address.

Each of the two different gateways were configured with DDNS instead of using a static IP address on each. This translates to The vBond orchestrator and the vManage NMS (for easy access of the GUI) having a domain name that is mapped to the dynamic IP address of their respective gateway (after applying port forwarding).

### Site 200:

Site 200 is the first service site and is the Data Center as it contains the Domain Controller. It includes one host computer running behind a gateway with the CSR1000v_01 (which is the WAN edge router) running as a VM instance inside a GNS3 instance and connected to the private network. The CSR1000v_01 has an IPS service installed that uses Cisco’s IPS Signature list. It’s also running the OSPF routing protocol on its LAN (Local Area Network) interface.

The private network includes:
- A FortiGate FW.
    - Running the OSPF routing protocol.
    - Running several policies.
- R1 and R2.
    - Running HSRP (Hot Standby Router Protocol) with R1 being the active router and R2 being on standby.
    - Running the OSPF routing protocol.
    - Serving as DHCP (Dynamic Host Configuration Protocol) relay agents between the Domain Controller and users.
- A Core Switch, two Distribution Switches, and three Access Switches.
    - Users are distributed into three different VLANs having IDs 40, 50, and 60 according to which Access Switch they’re connected to.
    - The Domain Controller is under VLAN 70 connected to S7.
- The Domain Controller which provides multiple services to both networks in Site 200 and Site 300.

### Site 300:

Site 300 is the second service site. It includes one host computer running behind a gateway with the CSR1000v_02 running as a VM inside a GNS3 instance and connected to the private network. Similar to CSR1000v_01, the CSR1000v_02 has an IPS service installed that uses Cisco’s IPS Signature list. It’s also running the OSPF routing protocol on its LAN interface.

The private network connected to CSR1000v_02 is similar to the one connected to CSR1000v_01, however, it has different VLAN IDs (10, 20, and 30), different IP prefixes, and no Domain Controller (relies on the one in Site 200).

