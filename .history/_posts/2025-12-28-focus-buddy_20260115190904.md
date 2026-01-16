---
layout: post
title: "Windows Process Tracking & Logic"
author: "Jordan Schilling"
date: 2025-12-06
category: focus-buddy
---

# Project Update: Implement Time Tracking for Active Applications with SQLite Database Integration


# Implement Time Tracking for Active Applications with SQLite Database Integration

## Overview
This project adds functionality to track the amount of time spent on active applications in a Windows environment. It monitors the currently focused window, records usage duration, and persists the data in a SQLite database for later review.

## Features
- Tracks time spent on active applications using `win32api` and `win32gui`
- Stores application usage data in a SQLite database
- Continuously monitors active windows via a main tracking loop
- Handles user input to:
  - Exit the tracking process gracefully
  - Print a summarized report of time spent per application
- Includes error handling for access-denied scenarios when querying active windows

## Implementation Details
- Uses Windows APIs to detect the currently active application window
- Aggregates time spent per application during execution
- Persists records in a structured SQLite database schema
- Provides a simple CLI interaction model for runtime control

## References
- https://docs.python.org/3/library/time.html#time.strftime
- https://docs.python.org/3/library/time.html#time.asctime
- https://stackoverflow.com/questions/23675821/how-to-track-how-long-an-application-is-running-for-using-a-timer
- https://stackoverflow.com/questions/44689546/how-to-print-out-a-dictionary-nicely-in-python
- https://stackoverflow.com/questions/5409025/whats-the-difference-between-localtime-and-gmtime-in-c
- https://stackoverflow.com/questions/8331469/python-dictionary-to-csv
