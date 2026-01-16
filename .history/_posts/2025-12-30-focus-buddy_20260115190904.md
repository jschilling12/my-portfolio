---
layout: post
title: "Windows Process Tracking & Logic"
author: "Jordan Schilling"
date: 2025-12-06
category: focus-buddy
---

# Project Update: Refactoring Time Tracking Functionality

## Overview
This refactor focuses on improving the structure and portability of the time tracking system. The logic was encapsulated into a dedicated `timeTracker` class, and the persistence layer was simplified by removing the SQLite database in favor of a CSV-based approach.

The original database-backed design proved unnecessary for the scope of the application and introduced extra complexity—especially considering plans to package the project into a distributable application. A CSV handler offered a cleaner, more portable solution.

## Key Refactoring Decisions

### 1. Encapsulation with `timeTracker` Class
- Centralized all time tracking logic into a single class
- Improved readability, maintainability, and testability
- Reduced reliance on global state

### 2. Abandoning SQLite for CSV
- Removed SQLite database integration entirely
- Simplified data storage and export
- Avoided writing data directly into the local working directory, which is unsuitable for packaged applications
- Enabled easier file management and portability

### 3. Improved CSV Export Handling
Initially, CSV rows were manually constructed by iterating over dictionary values and appending lists. This approach was replaced with a more robust and scalable solution using `csv.DictWriter`.

### Before

```python
for key, value in time_tracking.items():
    t = time.gmtime(value)
    values = time.strftime("%H:%M:%S", t)
    rows.append([today, key, values])
```

### After

```python
with open(file, 'w') as csvfile:
    fields = ['Run Time', 'Application Path', 'Time']
    csvwriter = csv.DictWriter(csvfile, fieldnames=fields)
    csvwriter.writeheader()
    csvwriter.writerow({'Run Time': today})

    for key, value in time_tracking.items():
        csvwriter.writerow({
            "Run Time": today,
            "Application Path": key,
            "Time": time.strftime("%H:%M:%S", time.gmtime(value))
        })

return True
```