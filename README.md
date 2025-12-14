# Nishizumi Setups Sync

![Icon](icon.png)

**Versão:** 1.1.0-fullgui

[**Download the latest release here**](https://github.com/nishizumi-maho/nishizumi-setups-sync/releases/latest)

## Setup Manager

This repository provides Python script `nishizumi_setups_sync.py` to copy and synchronize iRacing setup files with advanced features including GUI, profiles, and automatic updates.

## Features

### Core Functionality
- **Multiple Import Modes**: Import setups from ZIP/RAR archives, folders, or skip importing entirely
- **Smart Car Detection**: Automatically identifies car folders with customizable mapping
- **Bidirectional Sync**: Synchronizes setups between source and destination folders
- **Profile System**: Store multiple configurations and switch between them easily
- **Dry-Run Mode**: Preview all operations without making any changes

### Import & Sync
- Customizable team, personal, supplier, and season folder names per profile
- Works with any setup supplier - just select their folder or archive
- NASCAR support: automatically syncs Cup, Xfinity, and Trucks variants
- Super Formula support: syncs Honda and Toyota variants
- Data packs synchronization across car variants
- Optional extra folders from third-party sync tools (configurable location: car root or destination)

### Driver Management
- **Driver Folders**: Create per-driver setups with Common Setups fallback
- **Garage61 Integration**: Automatically fetch and sync driver lists from Garage61 API
- **Manual Driver Management**: Add/remove drivers through GUI or configuration
- Automatic cleanup of unknown driver folders

### Advanced Features
- **Backup System**: Create backups before and/or after sync operations
- **Logging**: Comprehensive logging with configurable levels (DEBUG, INFO, WARN, ERROR)
- **Tray Mode**: Run in system tray with automatic periodic syncing
- **Auto-Update**: Built-in update checker and installer
- **Custom Car Mapping Editor**: GUI tool to manage folder name mappings
- **File Filtering**: Copy only `.sto` files or all file types
- **Plugin Support**: Extensible via hooks (before_sync/after_sync)

## Installation

### Requirements
- Python 3.9 or later
- Required dependencies:

```bash
pip install -r requirements.txt
```

**Dependencies:**
- `PySide6` - For GUI (optional but recommended)
- `requests` - For Garage61 integration and updates (optional)
- `rarfile` - For RAR archive support (optional)

## Usage

### GUI Mode (Default)

Simply run the script to open the graphical interface:

```bash
python nishizumi_setups_sync.py
# or
python nishizumi_setups_sync.py gui
```

### Command Line Interface

The tool supports multiple commands:

```bash
# Run sync with current configuration
python nishizumi_setups_sync.py run

# Dry-run mode (preview without making changes)
python nishizumi_setups_sync.py dry-run
# or
python nishizumi_setups_sync.py run --dry-run

# Import archive or folder
python nishizumi_setups_sync.py import /path/to/setups.zip
python nishizumi_setups_sync.py import /path/to/setups_folder

# Check for updates
python nishizumi_setups_sync.py check-update

# Apply available update
python nishizumi_setups_sync.py update

# Force GUI mode (even if run_on_startup is enabled)
python nishizumi_setups_sync.py gui
```

### Running Silently

For automation or startup scripts:

```bash
# Run with saved options without showing UI
python nishizumi_setups_sync.py run

# On Windows, use pythonw.exe to run without console window
pythonw.exe nishizumi_setups_sync.py run
```

### Tray Mode

Stay running in the background and sync periodically:

```bash
python nishizumi_setups_sync.py run  # with tray_mode enabled in config
```

Or enable "Stay in tray and rescan periodically" in the GUI.

## Configuration

Settings are stored in `user_config.json` with the following structure:

### Basic Settings
- `iracing_folder` - Path to iRacing setups folder
- `source_type` - Import mode: "zip", "folder", or "none"
- `zip_file` - Path to archive when using zip mode
- `source_folder` - Path to source when using folder mode

### Profile System
- `profiles` - Array of profile configurations
- `active_profile` - Index of currently active profile (0-based)

Each profile contains:
- `team_folder` - Team destination folder name
- `personal_folder` - Personal source folder name
- `supplier_folder` - Setup supplier subfolder name
- `season_folder` - Season subfolder name

### Sync Settings
- `sync_source` - Source folder to copy from
- `sync_destination` - Destination folder to copy to
- `hash_algorithm` - "md5" or "sha256" for file comparison
- `copy_all` - Copy all files (true) or only .sto files (false)

### Driver Settings
- `use_driver_folders` - Enable driver-specific folders
- `drivers` - Array of driver names
- `use_garage61` - Enable Garage61 API integration
- `garage61_team_id` - Team ID for Garage61
- `garage61_api_key` - API key for Garage61

### Extra Folders
- `use_external` - Enable extra sync folders
- `extra_folders` - Array of folder definitions with:
  - `name` - Folder name
  - `location` - "car" (car root) or "dest" (inside destination)

### Backup & Logging
- `backup_enabled` - Enable backup system
- `backup_before_folder` - Backup location before sync
- `backup_after_folder` - Backup location after sync
- `enable_logging` - Enable file logging
- `log_file` - Path to log file

### Automation
- `run_on_startup` - Run silently on startup
- `tray_mode` - Stay in system tray
- `tray_interval` - Hours between automatic syncs

## How to Use

### First-Time Setup

1. **Launch the GUI:**
   ```bash
   python nishizumi_setups_sync.py
   ```

2. **Configure iRacing Folder:**
   - Click "Browse" next to "iRacing Setups Folder"
   - Select your iRacing setups directory (typically `Documents\iRacing\setups`)

3. **Choose Import Mode:**
   - **zip** - Import from ZIP/RAR archive
   - **folder** - Import from existing folder
   - **none** - Skip import, only sync existing files

4. **Set Up Profile:**
   - Fill in folder names (names only, not paths):
     - Team Folder Name (destination)
     - Personal Folder Name (source)
     - Supplier/Driver Folder
     - Season Folder

5. **Configure Sync:**
   - Sync Source (copy from) - e.g., "Private"
   - Sync Destination (copy to) - e.g., "Shared"

6. **Optional: Enable Driver Folders**
   - Check "Manually enter drivers" to add driver names
   - Or enable "Use Garage61 API" with your team credentials

7. **Save and Run:**
   - Click "Save Config" to store settings
   - Click "Run Now" to execute sync

### Working with Profiles

Profiles allow you to maintain different configurations:

- **Create Profile**: Add a new entry in the profiles array
- **Switch Profile**: Change `active_profile` to the desired index
- **Profile Contents**: Each profile stores team, personal, supplier, and season folder names

### Extra Folders

Configure additional folders for third-party sync tools:

1. Enable "Use extra sync folders"
2. Click "Add Extra Folder"
3. Enter folder name
4. Choose location:
   - **car** - Folder exists at car root level
   - **dest** - Folder exists inside sync destination

### Custom Car Mapping

If the tool doesn't recognize a car folder:

1. It will prompt you during import (GUI mode)
2. Your choice is saved to `custom_car_mapping.json`
3. Use "Edit Car Mapping" in GUI to review/modify mappings

### Example Configuration

```json
{
  "iracing_folder": "C:\\Users\\YourName\\Documents\\iRacing\\setups",
  "source_type": "zip",
  "zip_file": "C:\\Downloads\\FastSetups_2025S1.zip",
  "backup_enabled": true,
  "backup_before_folder": "D:\\Backups\\SetupsBackup",
  "backup_after_folder": "D:\\Backups\\SetupsAfter",
  "active_profile": 0,
  "profiles": [
    {
      "team_folder": "MyTeam",
      "personal_folder": "DriverOne",
      "supplier_folder": "FastSetups",
      "season_folder": "2025S1"
    }
  ],
  "sync_source": "Private",
  "sync_destination": "Shared",
  "use_driver_folders": true,
  "drivers": ["John Doe", "Jane Smith"],
  "extra_folders": [
    {"name": "CrewChief", "location": "car"},
    {"name": "Telemetry", "location": "dest"}
  ],
  "copy_all": false,
  "enable_logging": true
}
```

## Interface Guide

### Main Window

- **iRacing Setups Folder** - Full path to iRacing setups directory
- **Import Mode** - Select zip, folder, or none
- **Archive/Folder to import** - Path to source (shown based on import mode)
- **Profile Settings** - Current profile configuration
- **Sync Settings** - Source and destination folder names
- **Extra Folders** - Additional sync folders with location control
- **Hash Algorithm** - md5 or sha256 for file comparison
- **Copy everything** - Include all file types, not just .sto

### Driver Folders Section

- **Use Garage61 API for drivers** - Automatic driver list management
  - Garage61 Team ID
  - Garage61 API Key
- **Manually enter drivers** - Manual driver list management
  - Add Driver button
  - Remove Selected button
  - Driver list display

### Backup & Logging

- **Enable backup** - Create backups before/after sync
  - Backup (before) - Pre-sync backup location
  - Backup (after) - Post-sync backup location
- **Enable logging to file** - Write operations to log file
  - Log file path

### Automation

- **Run silently on startup** - Execute without GUI when launched
- **Stay in tray and rescan** - Background mode with periodic sync
  - Interval in hours

### Buttons

- **Save Config** - Store settings without running
- **Run Now** - Save settings and execute sync
- **Check for Updates** - Download latest version if available

## Running Silently on Windows Startup

To run automatically at system startup:

### Method 1: Using Python Script

1. Enable "Run silently on startup" in the GUI
2. Create a shortcut:
   ```
   pythonw.exe "C:\path\to\nishizumi_setups_sync.py" run
   ```
3. Place shortcut in Startup folder:
   - Press `Win + R`
   - Type `shell:startup`
   - Paste shortcut

### Method 2: Using Compiled EXE

1. Create a shortcut to the EXE
2. Add command-line argument if needed: `nishizumi_setups_sync.exe run`
3. Place in Startup folder

### Method 3: Task Scheduler (Advanced)

1. Open Task Scheduler
2. Create Basic Task
3. Trigger: At log on
4. Action: Start a program
5. Program: `pythonw.exe` or `nishizumi_setups_sync.exe`
6. Arguments: `run`
7. Check "Run whether user is logged on or not"

## Dry-Run Mode

Preview all operations without making changes:

```bash
# Command line
python nishizumi_setups_sync.py dry-run

# Or
python nishizumi_setups_sync.py run --dry-run
```

All operations will be logged with `[DRY-RUN]` prefix, showing what would happen without modifying any files.

## Building a Windows EXE

Create a standalone executable using PyInstaller:

### Prerequisites

1. Install Python 3.9+
2. Install PyInstaller:
   ```bash
   pip install pyinstaller
   ```

### Build Process

```bash
# Single-file executable
pyinstaller --onefile --windowed nishizumi_setups_sync.py

# With icon (if available)
pyinstaller --onefile --windowed --icon=icon.ico nishizumi_setups_sync.py
```

The executable will be in the `dist` folder. Configuration is stored in `user_config.json` next to the `.exe`.

### Build Options

- `--onefile` - Single executable file
- `--windowed` - No console window (GUI only)
- `--icon=icon.ico` - Custom application icon
- `--name="Nishizumi Sync"` - Custom executable name

## Troubleshooting

### GUI Won't Start

- **Missing PySide6**: Install with `pip install PySide6`
- **No display available**: Script automatically falls back to CLI mode

### Import Not Working

- **Archive not recognized**: Ensure `rarfile` is installed for RAR support
- **Car folder not mapped**: Use GUI to map unknown folders or edit `custom_car_mapping.json`

### Garage61 Integration Issues

- **Missing requests**: Install with `pip install requests`
- **API errors**: Verify team ID and API key in configuration
- **Driver list not updating**: Check network connection and API credentials

### Sync Not Working

- **Folders not found**: Ensure folder names are correct (names only, not paths)
- **Nothing being copied**: Check if `copy_all` is disabled and source contains only non-.sto files
- **Driver folders not created**: Enable `use_driver_folders` in configuration

### Logging

Enable logging to troubleshoot issues:

1. Check "Enable logging to file" in GUI
2. Specify log file path
3. Run sync operation
4. Review log file for detailed operation history

## Advanced Usage

### Plugin System

Create hooks in `plugins` folder (if supported in your installation):

```python
# plugins/before_sync.py
def execute(config, logger):
    logger.info("Running before sync hook")
    # Your custom code here

# plugins/after_sync.py
def execute(config, logger):
    logger.info("Running after sync hook")
    # Your custom code here
```

### Custom Car Mapping

Edit `custom_car_mapping.json`:

```json
{
  "my custom folder name": "iracing_car_folder_name",
  "another folder": "dallarair18"
}
```

Keys are lowercase source folder names, values are iRacing car folder names.

### Custom configuration files

You can now load or export configuration presets directly from the GUI using the
"Load config…" and "Export current config…" buttons. The "Reset to defaults"
option reverts every setting to the built-in template. To run the CLI with a
specific configuration file, pass `--config /path/to/custom_config.json`.

### Programmatic Usage

Use as a module in your own scripts:

```python
from nishizumi_setups_sync import load_config, run_silent, Logger

config = load_config()
logger = Logger(config)

# Modify config as needed
config["iracing_folder"] = "/path/to/setups"

# Run sync
run_silent(config, logger, dry_run=False)
```

## Support

For issues, feature requests, or contributions, visit the [GitHub repository](https://github.com/nishizumi-maho/nishizumi-setups-sync).

## License

[Add your license information here]

## Changelog

### Version 1.1.0-fullgui
- Added full PySide6 GUI with enhanced features
- Implemented profile system for multiple configurations
- Added dry-run mode for operation preview
- Enhanced CLI with subcommands (run, import, check-update, update, dry-run, gui)
- Added custom car mapping editor
- Improved driver folder management with GUI editor
- Added extra folders UI with location control (car/dest)
- Enhanced logging with configurable levels
- Added tray mode for background operation
- Improved update check and auto-update functionality
- Added plugin/hooks support (before_sync/after_sync)
- Better error handling and user feedback
- Backwards-compatible configuration format
