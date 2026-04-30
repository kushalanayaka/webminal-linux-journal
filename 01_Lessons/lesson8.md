# Lesson 8 – System and User Details

**Platform:** Webminal  
**Status:** Completed  
**Date:** 2026-04-30

---

## `uptime` – System Uptime

Shows how long the system has been up and running.

Displays:
- Current time
- How long the system has been running
- How many users are currently logged on
- System load averages for the past 1, 5, and 15 minutes

## `date` – Current Date and Time

Displays the current time of the server running webminal.org website.

## `who` – Currently Logged Users

Shows information about users currently logged into the system.

`who -a` – Prints more detailed information about logged users.

## `w` – Detailed User Information

Gives more detailed information than `who`. Displays:
- Current time
- How long the system has been running
- How many users are currently logged on
- System load averages for the past 1, 5, and 15 minutes
- Information about users and their processes

## `mount` – Mounted File Systems

Provides list of mounted file systems.

`mount -t ext4` – Views only ext4 file systems.

## `df -h` – Free Disk Space

Displays free disk space on mounted devices. `-h` switch makes the output more readable for humans.

## `free -m` – Memory Usage

Displays the total amount of free and used physical and swap memory in the system, as well as the buffers used by the kernel. `-m` shows output in megabytes.

## Commands I Typed on Webminal

```bash
uptime
date
who
who -a
w
mount
mount -t ext4
df -h
free -m
