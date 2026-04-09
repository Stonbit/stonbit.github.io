---
layout: project
title: "Intune Printer Deployment Package"
description: Config-driven Win32 app for deploying direct IP printers via Microsoft Intune. JSON config, idempotent scripts, persistent logging.
category: ops
tag_label: PowerShell · Intune
stack: [PowerShell, Microsoft Intune, JSON]
github: https://github.com/yourusername/intune-printer-deploy
---

A deployment package for pushing network printers to Intune-managed Windows devices without a print server.

## Problem

Direct IP printing environments have no Group Policy printer deployment path. Intune's native printer support assumes a print server. The solution is a Win32 app package with custom install/detection/uninstall logic.

## How it works

A single JSON config file holds all printer-specific values. The install, detection, and uninstall scripts read from the config — meaning the logic never changes between deployments, only the config does.

All operations are idempotent. Running the install script twice doesn't create duplicate ports or printers. Persistent logging writes to a fixed path for easy troubleshooting.

## Stack

- PowerShell for all scripts
- JSON for configuration
- Microsoft Intune Win32 app deployment
- IntuneWinAppUtil for packaging
