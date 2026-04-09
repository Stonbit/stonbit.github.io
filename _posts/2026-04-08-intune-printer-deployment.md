---
layout: post
title: "Building a config-driven printer deployment package for Intune"
date: 2026-04-08
categories: [intune, powershell, automation]
description: How I built a Win32 app deployment package for network printers using a JSON config file, idempotent scripts, and persistent logging.
---

We do direct IP printing with no print server. When I needed to deploy printers to managed devices via Intune, I couldn't lean on the usual Group Policy printer deployment path. I built a Win32 app package instead.

## The problem

Our environment manages printing via Direct to IP. Up until this point, we had been manually installing every printer as IT staff. That meant, upon wiping and imaging a device, we were having to remember which printer staff or faculty needed access to, and installing it ourselves.

I sought a solution to this problem, and considering our recent migration to Intune had just begun, I looked for methods of deploying a solution to our users. Taking to Google, I found a nifty little reddit thread detailing win32 app deployment, which further linked out to an endpoint administration site with an entire guide on win32 app packaging for printer deployment. Lucky me!

So, I got to work. I opened up VSCode and a browser session with ChatGPT and the endpoint guide, and began building out a plan.

## Architecture

The package has three main components:

**A JSON config file** that holds all the printer-specific values — IP address, driver name, port name, share name. This means the logic scripts never need to change between printer deployments. All it takes is a simple file swap. I was familiar with JSON files from digging around in game files, which is not an uncommon source of my technical knowledge it turns out.

**An install script** that reads the config, creates the TCP/IP port if it doesn't exist, installs the driver, and adds the printer. Every operation is idempotent — running it twice doesn't create duplicates or errors. After reading "The Pragmatic Programmer", I was obsessed with making sure I checked for every error I could think of. I was absolutely terrified of designing bad code, and took every precaution I could to try and make this robust and production ready.

**A detection script** that checks whether the printer exists on the system. The first implementation of this failed miserably. I was checking that the port was created, even when the guide had recommended a registry key check. After implementing the change, the detection script worked.

## What I learned

Sometimes, the best way to learn is to do. Not too long ago, this all felt very arcane to me. Like the only people who could develop working scripts to any real degree of polish were wizards with knowledge totally beyond me. But, that's simply not the case. Sure, I might create bad scripts, or come up with the occasional bad idea, but the only thing that hurts me is to not even try.

This script turned out better than I could have ever hoped, and I learned a lot about powershell along the way. The important thing, especially when working with AI, is to take your time. Don't paste any code, the best thing you can do for yourself and everyone around you is to read the documentation. Understand every command, every line that you type, and what you can do with those lines of code in the future. The goal is to learn, not to generate.

## Result

A single deployment package that can be reconfigured for any direct IP printer by editing one JSON file. Clean install, clean uninstall, reliable detection.

