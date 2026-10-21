# PAT (NAT Overload) Configuration Lab

A Cisco Packet Tracer networking lab illustrating the design of Port Address Translation (PAT), also called NAT overload, for multiple private LAN clients accessing an external network through a single translated address.

## Project Overview

The topology shows two PCs connected to a LAN switch, a NAT router, an ISP router, and a server network labelled `8.8.8.0/24`. The router-to-ISP link is labelled `200.100.50.0/30`, and the inside LAN is labelled `192.168.20.0/24`.

> **Artifact limitation:** The original Cisco Packet Tracer `.pkt` file was not provided. This repository includes the supplied screenshot and documentation only. No working `.pkt` file has been invented, and the configuration has not been independently tested.

## Features

- Private IPv4 LAN
- NAT router connected to an ISP router
- PAT/NAT overload design
- External server network
- NAT translation verification workflow

## Technologies Used

- Cisco Packet Tracer
- Cisco IOS NAT/PAT concepts
- IPv4 addressing and subnetting
- ACL-based NAT matching
- Routing and ICMP testing

## Project Structure

```text
pat-nat-overload/
├── README.md
└── screenshots/
    └── pat-nat-overload-topology.png
```

## Network Topology Details

| Segment | Network shown |
|---|---|
| Inside LAN | `192.168.20.0/24` |
| NAT router–ISP link | `200.100.50.0/30` |
| External server network | `8.8.8.0/24` |

The screenshot shows a 2911 ISP router, a NAT router, a 2960-series switch, two PCs, and a server labelled “Google Web Server.” The server label is part of the lab diagram and does not indicate a real Google server.

## How to Recreate and Test

1. Add a NAT router, an ISP router, a 2960-series switch, two PCs, and a server.
2. Connect both PCs to the switch, the switch to the NAT router, the NAT router to the ISP router, and the ISP router to the server network.
3. Configure addresses and subnet masks based on the three networks above.
4. Configure the NAT router's LAN-facing interface as `ip nat inside`.
5. Configure the ISP-facing interface as `ip nat outside`.
6. Create an ACL matching the private LAN.
7. Configure PAT overload using the outside interface address.
8. Add appropriate routes in both directions, then test reachability from the PCs to the server.
9. Generate traffic and inspect the NAT translation table.

Illustrative IOS command pattern (replace interface names and IP details to match your recreated topology):

```text
access-list 1 permit 192.168.20.0 0.0.0.255
interface <inside-interface>
 ip nat inside
interface <outside-interface>
 ip nat outside
ip nat inside source list 1 interface <outside-interface> overload
```

## Useful Verification Commands

```text
show ip interface brief
show ip route
show access-lists
show ip nat translations
show ip nat statistics
ping <server-ip>
```

## Expected Results

- The router has valid routes to the LAN, ISP link, and server network.
- PCs can reach their configured default gateway.
- Server reachability succeeds when addressing and routing are correct.
- After client traffic crosses the NAT router, `show ip nat translations` should show translations and `show ip nat statistics` should report NAT activity.

These are expected outcomes for a correctly configured recreation, not results verified from the supplied screenshot.

## Important Notes

- The original `.pkt` file was not supplied.
- Replace interface placeholders and confirm interface names on your router model.
- The external server IP must be configured consistently with the server network.
- A real internet connection is not created by Packet Tracer; the external server is simulated within the topology.
