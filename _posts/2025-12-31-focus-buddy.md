---
layout: post
title: "Windows Process Tracking & Logic"
author: "Jordan Schilling"
date: 2025-12-06
category: focus-buddy
---

# Project Update: Time Tracking with CSV Export and GUI Save Location

## Overview

Implemented a time tracking application capable of detecting the active window and recording time spent per application, with CSV export functionality for persistence.

After further consideration, a more robust system was required to give users control over **where their data is saved**. This led to a pivot from a simple local OS-based script toward a more realistic and distributable application architecture.

As part of this evolution, the project introduced **Tkinter** and basic GUI concepts to support user interaction.

## Design Pivot

The original implementation wrote CSV output directly to the working directory. While functional, this approach was not suitable for a packaged application.

To address this, a configurable **save point** was introduced:

* Users can choose a save directory via a GUI
* The selected path is persisted for future runs
* Subsequent launches reuse the saved path automatically

This significantly improves usability and aligns the project with real-world distribution requirements.

## GUI Save Folder Implementation

A Tkinter directory selection dialog was implemented to allow the user to choose where CSV files are saved. The selected directory path is written to a configuration file and reused on future launches.

This feature was implemented successfully on the first attempt.

```python
def save_folder(self, config_path: Path):
    # Prompt a file dialog for directory selection.
    PathFileName = str(
        Path(
            tkinter.filedialog.askdirectory(
                mustexist=True,
                title="Select Directory to Save Time Tracking CSV"
            )
        )
    )
    if PathFileName:
        config_path.write_text(PathFileName)
    # Save the selected directory path for future use.
    # Check on subsequent launches if a path is already saved.
```

## References

* [https://stackoverflow.com/questions/12059509/create-a-single-executable-from-a-python-project](https://stackoverflow.com/questions/12059509/create-a-single-executable-from-a-python-project)
* [https://www.reddit.com/r/learnpython/comments/103jd0z/how_do_i_package_my_python_code_into_an/](https://www.reddit.com/r/learnpython/comments/103jd0z/how_do_i_package_my_python_code_into_an/)
* [https://www.reddit.com/search/?q=Pyinstaller](https://www.reddit.com/search/?q=Pyinstaller)
* [https://www.reddit.com/r/learnpython/comments/n7dtkx/how_to_write_a_csv_file_with_pyinstaller/](https://www.reddit.com/r/learnpython/comments/n7dtkx/how_to_write_a_csv_file_with_pyinstaller/)
* [https://docs.python.org/3/library/pathlib.html#pathlib.Path.home](https://docs.python.org/3/library/pathlib.html#pathlib.Path.home)
* [https://stackoverflow.com/questions/41308280/execution-of-a-python-script-and-save-the-output-as-a-csv-file](https://stackoverflow.com/questions/41308280/execution-of-a-python-script-and-save-the-output-as-a-csv-file)
* [https://stackoverflow.com/questions/64305421/trying-to-ask-for-a-file-input-and-save-it-in-specific-location](https://stackoverflow.com/questions/64305421/trying-to-ask-for-a-file-input-and-save-it-in-specific-location)
* [https://docs.python.org/3/library/dialog.html#module-tkinter.simpledialog](https://docs.python.org/3/library/dialog.html#module-tkinter.simpledialog)
* [https://docs.python.org/3/library/dialog.html#tkinter.filedialog.SaveAs](https://docs.python.org/3/library/dialog.html#tkinter.filedialog.SaveAs)
* [https://stackoverflow.com/questions/32864610/understanding-parent-and-controller-in-tkinter-init](https://stackoverflow.com/questions/32864610/understanding-parent-and-controller-in-tkinter-init)
* [https://stackoverflow.com/questions/74664156/how-can-i-store-current-directory-as-variable-in-python](https://stackoverflow.com/questions/74664156/how-can-i-store-current-directory-as-variable-in-python)
* [https://stackoverflow.com/questions/12277864/python-clear-csv-file](https://stackoverflow.com/questions/12277864/python-clear-csv-file)
* [https://www.learnbyexample.org/python-file-handling/](https://www.learnbyexample.org/python-file-handling/)
* [https://www.w3schools.com/python/python_file_write.asp](https://www.w3schools.com/python/python_file_write.asp)
* [https://stackoverflow.com/questions/8369219/how-can-i-read-a-text-file-into-a-string-variable-and-strip-newlines](https://stackoverflow.com/questions/8369219/how-can-i-read-a-text-file-into-a-string-variable-and-strip-newlines)
* [https://labex.io/pythoncheatsheet/cheatsheet/file-directory-path](https://labex.io/pythoncheatsheet/cheatsheet/file-directory-path)
* [https://stackoverflow.com/questions/55400992/create-a-folder-in-the-directory-and-save-files-in-the-new-folder](https://stackoverflow.com/questions/55400992/create-a-folder-in-the-directory-and-save-files-in-the-new-folder)
* [https://stackoverflow.com/questions/18884782/typeerror-worker-takes-0-positional-arguments-but-1-was-given](https://stackoverflow.com/questions/18884782/typeerror-worker-takes-0-positional-arguments-but-1-was-given)

---
