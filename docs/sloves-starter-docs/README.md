# Sloves Starter 🚀

**Python Simple Launcher For Virtual Environment Scripts**

---

## What's This?

Sloves Starter is a single-file Python script launcher that handles virtual environments and dependencies automatically.
The goal is to not take up space in the project and to ensure that all Python packages are for the main project.

```bash
# Just This Command
python Sloves_Starter.py
```

## Features

- ✅ **Auto Virtual Environment** - Creates/activates venv automatically
- ✅ **Smart Dependency Management** - Installs requirements.txt or config-based packages
- ✅ **Interactive Script Selection** - Choose scripts from your project
- ✅ **Cross-Platform** - Windows, Linux, macOS ready
- ✅ **Configuration Driven** - JSON config for all settings
- ✅ **Performance Monitoring** - Nanosecond-level runtime tracking

## Quick Start

1. Drop `Sloves_Starter.py` in your project
2. (Optional) Create `config.json` for customization
3. Run: `python Sloves_Starter.py`

## Example Config

```json
{
    "title": "Sloves Python Script Starter", // Title of the console window
    "console_title": "Sloves Python Script Starter", // Center text caption for the console
    "process_title": "Python Script", // The title to display when entering the program
    "process_exit_title": "Sloves Python Script Starter", // The title to display after exiting the program
    "exit_title": "Sloves Python Script Starter", // The title to display after exiting the launcher
    "python_name": {
        "windows": "python",
        "linux": "python3",
        "macos": "python3",
        "jvm": "python3",
        "default": "python3"
    },
    "pip_name": {
        "windows": "pip",
        "linux": "pip3",
        "macos": "pip3",
        "jvm": "pip3",
        "default": "pip3"
    },
    "requirements": [], // Contents to write into requirements.txt
    "requirements_file": "requirements.txt", // Name of the requirements file
    "cwd": "./", // Working directory of the launcher
    "work_directory": "./", // Working Directory of the object program
    "use_venv": true, // Warning: functionality with this option turned off has not been tested for availability
    "venv_prompt": "venv", // Virtual Environment Prompt
    "script_name": null, // Automatic Search
    "arguments": null, // Pass through using starter parameters
    "python_arguments": null, // Pass through using python parameters
    "restart": false, // Automatic restart after program exit
    "reselect": false, // Automatic restart requires the user to reselect the script to execute
    "run_cmd_need_to_ask": true, // Asked when running an external command
    "ask_default_values": {}, // ASK options that are automatically populated at run time (ID reference [AskID](#AskID)) 
    "divider_line_char": "=", // Divider Line Character
    "inject_environment_variables": {}, // Inject sub-process environment variables
    "text_encoding": "utf-8", // Text File Encoding
    "print_return_code": true, // Whether to print the child process return value
    "print_runtime": true, // Whether to print the child process run time
    "automatic_exit": false, // Exit directly after execution, instead of pausing
    "allow_print": true // Error messages are not affected after closing
}
```
When it can't find one, it tries to create one.

## AskID

- Create requirements file
- Run Cmd
- Create New Configuration File

## Exit Codes

| Code | Description |
| --- | --- |
| -1 | ONLY_PAUSE |
| 0 | SUCCESS |
| 1 | CONFIG_NOT_FOUND |
| 2 | CONFIG_DECODE_ERROR |
| 3 | CONFIG_ENCODE_ERROR |
| 4 | CONFIG_WRITE_ERROR |
| 5 | CONFIG_READ_ERROR |
| 6 | CONFIG_PERMISSION_ERROR |
| 7 | SCRIPT_NOT_FOUND |
| 8 | SCRIPT_NAME_TYPE_ERROR |
| 9 | SCRIPT_NAME_IS_EMPTY |
| 10 | SCRIPT_NAME_NOT_PROVIDED |
| 11 | USER_TERMINATED |
| 12 | CONFIG_PARSING_ERROR |
| 255 | UNKNOWN_ERROR |

When the child process runs, the return value of the child process is returned.
When the program returns an `UNKNOWN_ERROR`, it writes the traceback to the `Traceback.txt` file.

---

## License

[MIT License](LICENSE)