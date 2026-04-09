---
layout: post
title: "Building a config-driven printer deployment package for Intune"
date: 2026-04-08
categories: [intune, powershell, automation]
description: How I built a Win32 app deployment package for network printers using a JSON config file, idempotent scripts, and persistent logging.
---

We do direct IP printing with no print server. When I needed to deploy printers to managed devices via Intune, I couldn't lean on the usual Group Policy printer deployment path. I built a Win32 app package instead.

## The problem

Intune doesn't have a native "deploy a printer" workflow for direct IP scenarios. Your options are basically:

- Manual installation on each device
- A script deployed as a PowerShell script object (limited logging, harder to target)
- A Win32 app with proper install/detection/uninstall logic

Win32 app was the right call. It gives you proper deployment targeting, retry logic, and the ability to define detection rules.

## Architecture

The package has three main components:

**A JSON config file** that holds all the printer-specific values — IP address, driver name, port name, share name. This means the logic scripts never need to change between printer deployments. You just swap the config.

**An install script** that reads the config, creates the TCP/IP port if it doesn't exist, installs the driver, and adds the printer. Every operation is idempotent — running it twice doesn't create duplicates or errors.

**A detection script** that checks whether the printer exists on the system and exits with the appropriate code for Intune to interpret as installed or not.

## What I learned

The detection script is where I initially ran into trouble. My first version checked that the printer port was bound rather than checking a registry key, which caused false negatives. The more reliable approach is checking for the printer object directly via `Get-Printer` or using a registry key detection rule in Intune rather than a script at all.

Persistent logging to a fixed path made troubleshooting significantly easier — instead of guessing what happened during a silent deployment, you have a dated log file telling you exactly what ran and what the result was.

## Result

A single deployment package that can be reconfigured for any direct IP printer by editing one JSON file. Clean install, clean uninstall, reliable detection.

The scripts are on GitHub if you want to adapt them for your own environment.
