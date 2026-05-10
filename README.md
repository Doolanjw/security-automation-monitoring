# Security Automation & Monitoring

> **Note on AI assistance:** This is a portfolio project completed during my pivot from twenty years of legal practice into AI Governance and cybersecurity GRC. The technical implementation work in this repository was completed with AI tooling (ChatGPT, Claude, Copilot) under my direction. My role on the project was design, evaluation, documentation, and quality assurance — the specific seat the IAPP AIGP credential is designed to fill. I do not represent myself as a hands-on developer or security engineer.

**Author:** Jonathan W. Doolan  
**Date:** August 2025  
**Platform:** Windows Server

---

## Overview

Designed and implemented a Windows security monitoring and automated reporting system for a mid-sized enterprise facing increased cyber threats. The project involved configuring advanced audit policies, building a PowerShell script to generate HTML security reports, testing the system with simulated attack scenarios, and automating report generation via Windows Scheduled Tasks for continuous compliance monitoring.

## Problem Statement

The organization needed enhanced visibility into authentication activity on its Windows-based systems, specifically:
- Detection of failed login attempts indicating brute-force or credential stuffing attacks
- Tracking of successful logins for correlation and anomaly detection
- Monitoring of process creation events to identify suspicious post-logon activity
- Automated reporting for security teams without manual log review

## What I Built

### 1. Advanced Audit Policy Configuration
- Enabled login/logoff auditing (success and failure) via Windows Security Policy
- Enabled credential validation auditing (success and failure)
- Enabled process creation auditing via Group Policy
- Configured audit process creation to capture command-line arguments

### 2. PowerShell Automated Reporting Script
Built `Generate-WindowsLogonReport.ps1`, a parameterized PowerShell script that:
- Queries Windows Security Event Log for Event IDs 4625 (failed logons), 4624 (successful logons), and 4688 (process creation)
- Parses raw event XML to extract usernames, IP addresses, workstation names, logon types, and failure reason codes
- Translates failure status codes into human-readable descriptions (incorrect password, account locked, account disabled, etc.)
- Translates logon type codes (interactive, network, RDP, service, etc.)
- Filters out system accounts and noise from successful logon data
- Generates a professional styled HTML report with color-coded sections, statistics dashboard, and timestamped event tables
- Accepts parameters for output path and lookback window (default: 24 hours)

### 3. Testing with Simulated Attack Scenario
- Created a test user account and forced multiple failed login attempts
- Verified the script captured all failed attempts with correct timestamps, source IPs, and failure reasons
- Confirmed successful logon events were properly correlated
- Report correctly showed user "Test1" had 3
