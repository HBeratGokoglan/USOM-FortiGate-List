# USOM FortiGate Threat Feed Integration

This repository provides an automated system to fetch malicious IP, Domain, and URL lists from **USOM** (National Cyber Response Center of Turkiye) and integrate them directly into **FortiGate Next-Generation Firewalls**.

## Overview
The system automates the cycle of fetching, processing, and hosting threat intelligence:
1. **Fetch:** PowerShell script retrieves data from USOM API.
2. **Process:** Merges Domains and URLs; splits files at 120,000 lines for FortiGate compatibility.
3. **Categorize:** Organizes data into folders by Criticality Levels (Level 4, 5, 6, etc.).
4. **Deploy:** Automatically pushes updates to this repository using GitHub API.

## Repository Structure
Each level folder contains:
- `iplist.txt`: Malicious IP addresses.
- `combined_list_x.txt`: Merged Domain and URL lists (Split into multiple files if necessary).

## FortiGate Implementation
To integrate these feeds:
1. Navigate to **Security Fabric > External Connectors**.
2. Select **Create New > IP Address** (for iplist) or **Domain Name** (for combined lists).
3. Provide the **Raw GitHub URL**:
   `https://raw.githubusercontent.com/[YOUR_USERNAME]/[REPO_NAME]/main/Level[X]/[FILENAME].txt`
4. Configure **Refresh Rate** as per your security policy.
5. Use the created objects in **Firewall Policies**.

## Technical Notes
- **Update Frequency:** Files are overwritten during each run to ensure only active threats are blocked.
- **Traceability:** Each file includes a `# Last Updated` timestamp header.
- **Resilience:** The automation includes retry logic for API stability and connection persistence.
