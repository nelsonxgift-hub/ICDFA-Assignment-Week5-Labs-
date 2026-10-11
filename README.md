# ICDFA GRC102 - Practical Lab 8: Security Governance Simulation Assessment

**Student:** Rosemary Gift Nelson
**Registration Number:** C11/26/CGRCE-17216
**Cohort:** Cohort 11 / Batch A
**Course:** GRC102 - Information Security Governance
**Assignment:** Week 5 Practical Laboratory, Practical Lab 8: Security Governance Simulation Assessment

## Overview

This repository contains the full working environment, scripts, data, and evidence produced for Practical Lab 8. The assignment simulates a security governance program built around a vulnerable web application environment modeled on the circumstances of the 2017 Equifax data breach, specifically CVE-2017-5638, the Apache Struts2 remote code execution vulnerability. The lab is structured in four parts covering environment setup, governance failure analysis, control implementation, and governance metrics and reporting.

The full narrative explanation of every step, including labeled screenshots and analysis, is contained in the separate Associated Evidence Report submitted alongside this repository. This README describes the repository structure and how the lab environment can be reproduced.

## Repository Structure

### Part 1: Setting Up a Simulated Environment
- `docker-compose.yml` - defines the three container lab topology: a vulnerable Apache Struts2 application (CVE-2017-5638), a MySQL 5.7 database with weak default credentials, and an Ubuntu 20.04 monitoring host
- `database_init/init.sql` - initial database schema and seed data for the MySQL container
- `initialize_governance.py` - seeds the governance tracker with a baseline set of policies, controls, and risks
- `governance_tracker.py` - reads and displays the current governance posture from governance_data.json
- `vulnerability_scanner.py` - scans the lab environment for known vulnerabilities and configuration weaknesses
- `patch_management.py` - records and tracks patch status against identified vulnerabilities in the governance data file
- `governance_data.json` - central governance record: policies, controls, risks, incidents, and metrics history
- `vulnerability_report_*.json` - timestamped vulnerability scan output

### Part 2: Analyzing Security Governance Failures
- `simulate_attack.py` - exploits the Struts2 container using the CVE-2017-5638 Jakarta Multipart parser vulnerability
- `attack_log.json` - timestamped log of attack simulation requests and responses
- `analyze_governance_failures.py` - cross references attack and scan evidence against the governance baseline to identify control gaps
- `governance_failure_report_*.json` - structured governance failure findings with risk exposure ratings
- `equifax_comparison.md` - comparison of the simulated findings against the documented root causes of the real Equifax breach
- `part2_lessons_learned.md` - summary of lessons drawn from the attack simulation and failure analysis

### Part 3: Implementing Security Governance Controls
- `implement_governance_controls.py` - applies governance level remediations and updates control status in governance_data.json
- `governance_controls_report.md` - documents each implemented control, the gap it addresses, and its mapping back to Part 2 findings
- `part3_explanation.md` - explains the post implementation verification, including the finding that governance tracking and live technical remediation are not automatically the same thing
- `patch_status.json` - tracked patch status for identified vulnerabilities
- `monitoring/security_monitor.sh` - continuous monitoring shell script
- `monitoring/security_cron` - cron schedule for running the monitoring script at regular intervals
- `governance_oversight.md` - updated control ownership and review cadence

### Part 4: Developing Security Governance Metrics and Reporting
- `governance_metrics.py` - calculates governance metrics (patch compliance, vulnerability remediation, security incidents, monitoring coverage, policy compliance) from the full governance_data.json history
- `metrics_visualizations/` - five trend charts generated with matplotlib: patch_compliance.png, vuln_remediation.png, security_incidents.png, monitoring_coverage.png, policy_compliance.png
- `executive_dashboard.html` - self contained HTML executive dashboard combining the charts and summarized metric values
- `board_report.md` - narrative governance report written for a non technical board audience
- `governance_metrics_framework.md` - documents how each metric is calculated, its data source, and its refresh frequency
- `dashboard_user_guide.md` - explains how to interpret the dashboard and what actions should follow from declining trends
- `part4_explanation.md` - ties the metrics and reporting work back to the governance failures and control verification findings from earlier parts

## Reproducing the Environment

Requirements: Docker and Docker Compose, Python 3, and the Python packages used by the governance scripts (matplotlib for the metrics visualizations).
Start the lab environment

docker compose up -d

Confirm all three containers are running

docker ps

Initialize the governance tracker

python3 initialize_governance.py

Run the vulnerability scan

python3 vulnerability_scanner.py

Run the attack simulation

python3 simulate_attack.py

Analyze governance failures

python3 analyze_governance_failures.py

Implement governance controls

python3 implement_governance_controls.py --all

Generate governance metrics and reports

chmod +x governance_metrics.py
python3 governance_metrics.py


The executive dashboard can be viewed by opening `executive_dashboard.html` directly in a browser after the metrics script has run.

## Key Finding

The most significant governance finding from this exercise is documented in `part3_explanation.md`: implementing and recording a governance control in `governance_data.json` is not the same as technically remediating the underlying system. After governance controls were implemented in Part 3, the vulnerability scan and attack simulation were re run and the Struts2 vulnerability was still present and exploitable, because `patch_management.py` tracks patch status rather than applying a binary patch to the running container. This mirrors the core lesson of the Equifax breach, where a documented patch management policy existed but the verification linking policy to actual system state broke down in practice.

## Related Submission

The complete narrative report, "Associated Evidence Report," containing labeled screenshots, command outputs, and explanatory analysis for all four parts in order, is submitted as a separate PDF document alongside this repository.

## Commit History

- Part 1: Docker environment setup and governance tracker initialization
- Part 2: Attack simulation, vulnerability scan, governance failure analysis, and Equifax comparison
- Part 3: Governance controls implementation and post implementation verification
- Part 4: Governance metrics, dashboard, board report, metrics framework, and user guide
