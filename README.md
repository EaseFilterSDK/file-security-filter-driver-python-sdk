# EaseFilter File System Filter Python SDK

![Platform](https://img.shields.io/badge/Platform-Windows-blue)
![Language](https://img.shields.io/badge/Language-Python%203.x-yellow)
![License](https://img.shields.io/badge/License-Commercial%20SDK-green)

The **EaseFilter Python SDK** provides a powerful, high-level Python interface to the EaseFilter Windows Kernel Minifilter Driver. This SDK empowers Python developers to easily integrate comprehensive file system monitoring, granular access control, transparent file encryption, and process sandboxing directly into their Python applications.
![EaseFilter File Security Architecture](https://www.easefilter.com/images/zero-trust-architecture.jpg)

## 🚀 Key Features

* **File System Monitoring:** Track file creation, deletion, reading, writing, renaming, and security changes in real-time.
* **Granular Access Control:** Intercept file I/O operations and dynamically allow, deny, or modify access based on user, process, or file type.
* **Transparent File Encryption:** Automatically encrypt data on-the-fly as it is written to the disk, and decrypt it when read by authorized processes.
* **Process Sandboxing:** Restrict untrusted processes from modifying critical system files or accessing confidential directories.
* **Rapid Integration:** Utilize Python's rapid development capabilities without needing to write complex C/C++ kernel-level code.

## Installation

NOTE: Administrator permissions are required to run EaseFilter.

### Install with Hatch

The recommended installation method uses [Hatch](https://hatch.pypa.io/latest/),
which takes care of the project dependencies.

First, [install Python](https://www.python.org/downloads/).

Then, install hatch:

    pip install hatch

Edit the license key field in `config.toml`:

    license_key = "..."

Change to this directory, and to run the included CLI, use:

    hatch run cli [args]

Otherwise, to start a Python REPL (command line), run:

    hatch run py

In this environment, you can import the `easefilter` module to interface with
the filter. The CLI's code is available at `easefilter/cli/main.py`, and can
serve as an example for using this library.

### Manual installation

If you do not wish to use Hatch, you can run this project directly through Python.

First, [install Python](https://www.python.org/downloads/).

Then, install the dependencies:

    pip install -r requirements.txt

Edit the license key field in `config.toml`:

    license_key = "..."

To run the CLI, use:

    python -m easefilter.cli [args]

And to start a Python REPL, change directory here, and run Python normally:

    python

## Visual Studio

Visual Studio can be used to navigate this codebase easily. Open
`ef-python.sln` to open this project.

There are no integrations for debugging/running the code directly in VS. Use
the terminal to install and run this project.

It may be helpful to set up a virtual environment; to do this, select `Tools /
Python / Python Environments` and click `Add environment`.

## CLI examples

Here are a few example CLI commands to try.

To get detailled help, run

    hatch run cli --help

or, for help on a specific subcommand,

    hatch run cli monitor --help

For documentation on event types you can register, see the file `easefilter/enums.py`.

### Monitor filter

Default settings, creates a test directory (`C:\easefilter_demo`) and monitors its contents:

    hatch run cli monitor

Monitor events for all files on your device:

    hatch run cli monitor *

Monitor a specific directory:

    hatch run cli monitor "C:\directory\*"

Enable monitoring all specific events (the `--io-mask` is shorthand for all the bitflags combined):

    hatch run cli monitor --io-mask 192118399729052330

Monitor all file handle creation events:

    hatch run cli monitor --io-sym POST_CREATE

### Control filter

Deny all file writes in a directory:

    hatch run cli control --deny-sym "ALLOW_WRITE_ACCESS,ALLOW_OPEN_WITH_CREATE_OR_OVERWRITE_ACCESS" C:\a_directory\*

Deny all reads in the directory (also denies `cd`-ing into directory):

    hatch run cli control --deny-sym "ALLOW_READ_ACCESS,ALLOW_OPEN_WITH_READ_ACCESS" C:\a_directory\*

Deny file listing in a directory (makes directory seem empty):

    hatch run cli control --deny-sym ALLOW_DIRECTORY_LIST_ACCESS C:\a_directory\*

### Process filter

Monitor all process creations/deletions (try starting a PowerShell to test this):

    hatch run cli process

Monitor specifically `cmd.exe`:

    hatch run cli process --proc-mask C:\Windows\System32\cmd.exe

Monitor all System32 processes:

    hatch run cli process --proc-mask C:\Windows\System32\*

(**BE CAREFUL** with this flag) Prevent `cmd.exe` from running:

    hatch run cli process --proc-control-sym DENY_NEW_PROCESS_CREATION --proc-mask C:\Windows\System32\cmd.exe

### Encryption filter

Transparently encrypt a directory:

    hatch run cli encryption 'C:\my_encrypted_dir\*'

This will prompt for a password. The password prompt does not show the
characters you type in.

Files will only be decrypted while the filter is running. Once stopped, files
will be encrypted and unreadable.

### Registry filter

Monitor all registry events:

    hatch run cli registry

Monitor all registry events for keys with a matching name:

    hatch run cli registry *KeyName*

Monitor all registry events coming from regedit:

    hatch run cli registry --proc-mask C:\Windows\regedit.exe

Monitor all registry key rename events coming from regedit:

    hatch run cli registry --reg-class-sym Reg_Pre_Rename_Key --proc-mask C:\Windows\regedit.exe

Prevent regedit from deleting keys:

    hatch run cli registry --reg-deny-sym REG_ALLOW_DELETE_KEY --proc-mask C:\Windows\regedit.exe

## Development

To format/lint after changing any code, run

    hatch fmt

or, to do a dry-run and avoid making changes,

    hatch fmt --check

These will run [Ruff](https://astral.sh/ruff) and [Pyright](https://github.com/microsoft/pyright).

Tests can be performed with [pytest](https://pytest.org/) like this:

    hatch test

You may specify a test to run by adding its path (e.g. `tests/test_something.py`) as an argument.
Consult [pytest's documentation](https://docs.pytest.org/en/stable/) for more information.


## EaseFilter File System Filter Driver SDK Reference
| Product Name | Description |
| --- | --- |
| [Cloud File System SDK](https://www.easefilter.com/cloud/cloud-file-system-sdk.htm) | EaseFilter Cloud File System SDK Introduction. |
| [CloudTier Storage Tiering SDK](https://www.easefilter.com/cloud/storage-tiering-sdk.htm) | EaseFilter Storage Tiering Filter Driver SDK Introduction. |
| [File Monitor SDK](https://www.easefilter.com/kb/file-monitor-filter-driver-sdk.htm) | EaseFilter File Monitor Filter Driver SDK Introduction. |
| [File Control SDK](https://www.easefilter.com/kb/file-control-file-security-sdk.htm) | EaseFilter File Control Filter Driver SDK Introduction. |
| [File Encryption SDK](https://www.easefilter.com/kb/transparent-file-encryption-filter-driver-sdk.htm) | EaseFilter Transparent File Encryption Filter Driver SDK Introduction. |
| [Registry Filter SDK](https://www.easefilter.com/kb/registry-filter-drive-sdk.htm) | EaseFilter Registry Filter Driver SDK Introduction. |
| [Process Filter SDK](https://www.easefilter.com/kb/process-filter-driver-sdk.htm) | EaseFilter Process Filter Driver SDK Introduction. |
| [EaseFilter SDK Programming](https://www.easefilter.com/kb/programming.htm) | EaseFilter Filter Driver SDK Programming. |

## EaseFilter SDK Sample Projects
| Sample Project | Description |
| --- | --- |
| [CloudTier Storage Tiering Demo](https://www.easefilter.com/cloud/cloudtier-storage-tiering-demo.htm) | A HSM File System Filter Driver Demo. |
| [CloudTier S3 Tiering Demo](https://www.easefilter.com/cloud/cloudtier-s3-intelligent-tiering-demo.htm) | CloudTier S3 Intelligent Tiering Demo. |
| [Cloud File DR S3 Demo](https://www.easefilter.com/cloud/cloud-file-dr-demo.htm) | Cloud File DR S3 Demo. |
| [Amazon S3 File Explorer Demo](https://www.easefilter.com/cloud/s3-browser-demo.htm) | Amazon S3 File Explorer Demo. |
| [Auto File DRM Encryption](https://www.easefilter.com/kb/auto-file-drm-encryption-tool.htm) | Auto file encryption with DRM data embedded. |
| [Transparent File Encrypt](https://www.easefilter.com/kb/AutoFileEncryption.htm) | Transparent on access file encryption. |
| [Secure File Sharing with DRM](https://www.easefilter.com/kb/DRM_Secure_File_Sharing.htm) | Secure encrypted file sharing with digital rights management. |
| [File Monitor Example](https://www.easefilter.com/kb/file-monitor-demo.htm) | Monitor file system I/O in real time, tracking file changes. |
| [File Protector Example](https://www.easefilter.com/kb/file-protector-demo.htm) | Prevent sensitive files from being accessed by unauthorized users or processes. |
| [FolderLocker Example](https://www.easefilter.com/kb/FolderLocker.htm) | Lock file automatically in a FolderLocker. |
| [Process Monitor](https://www.easefilter.com/kb/Process-Monitor.htm) | Monitor the process creation and termination, block unauthorized process running. |
| [Registry Monitor](https://www.easefilter.com/kb/RegMon.htm) | Monitor the Registry activities, block the modification of the Registry keys. |
| [Secure Sandbox Example](https://www.easefilter.com/kb/Secure-Sandbox.htm) |A secure sandbox example, block the processes accessing the files out of the box. |
| [FileSystemWatcher Example](https://www.easefilter.com/kb/FileSystemWatcher.htm) | File system watcher, logging the file I/O events. |
| [ZeroTrust Example](https://www.easefilter.com/kb/zero-trust-file-access-control-demo.htm) | Zero trust file access control with encryption feature. |

## Filter Driver Reference

* [Understand MiniFilter Driver](https://www.easefilter.com/kb/understand-minifilter.htm)
* [Understand File I/O](https://www.easefilter.com/kb/File_IO.htm)
* [Understand I/O Request Packets(IRPs)](https://www.easefilter.com/kb/understand-irps.htm)
* [Filter Driver Developer Guide](https://www.easefilter.com/kb/DeveloperGuide.htm)
* [MiniFilter Filter Driver Framework](https://www.easefilter.com/kb/minifilter-framework.htm)
* [Isolation Filter Driver](https://www.easefilter.com/kb/Isolation_Filter_Driver.htm)

## License

This repository contains two distinct licensing categories:

1. **Wrapper/Implementation Code (MIT License):** 
   The source code, scripts, and configuration files provided directly in this repository are licensed under the [MIT License](LICENSE). You are free to modify, distribute, and use this specific integration code.

2. **EaseFilter SDK (Commercial License):**
   This project requires the EaseFilter SDK (including its binaries, filter drivers, and core libraries) to function. The EaseFilter SDK is **proprietary commercial software** and is strictly excluded from the MIT License.   
   To use the EaseFilter SDK for commercial purposes or in a production environment, you must obtain a valid commercial license directly from [EaseFilter](https://www.easefilter.com/license.pdf).

## Support
If you have questions or need help, please contact support@easefilter.com 

[Home](https://www.easefilter.com/) | [Solution](https://www.easefilter.com/solutions.htm) | [Download](https://www.easefilter.com/download.htm) | [Demos](https://www.easefilter.com/online-fileio-test.aspx) | [Blog](https://blog.easefilter.com/) | [Programming](https://www.easefilter.com/kb/programming.htm)
