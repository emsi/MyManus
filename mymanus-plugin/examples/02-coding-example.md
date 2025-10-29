# Example 2: Coding Project

## Task Description

**User Request:**
```
Build a Python command-line tool that monitors a directory for changes
and automatically backs up modified files to a timestamped backup directory.
Include proper error handling, logging, and tests.
```

## Expected Behavior with MyManus Plugin

### Phase 1: Planning (TodoWrite)

Claude creates a comprehensive development plan:

```
✓ Creating project todo list
  - Plan project architecture and structure
  - Create project directory structure
  - Implement core file monitoring functionality
  - Implement backup logic with timestamping
  - Add error handling and logging
  - Write configuration management
  - Create command-line interface
  - Write unit tests
  - Write integration tests
  - Create documentation (README, usage examples)
  - Test end-to-end functionality
```

### Phase 2: Architecture Planning

#### Step 1: Project Structure

Claude automatically creates an organized structure:

```
file_backup_monitor/
├── README.md
├── requirements.txt
├── setup.py
├── src/
│   ├── __init__.py
│   ├── monitor.py        # Core monitoring logic
│   ├── backup.py         # Backup operations
│   ├── config.py         # Configuration management
│   └── cli.py            # Command-line interface
├── tests/
│   ├── __init__.py
│   ├── test_monitor.py
│   ├── test_backup.py
│   └── test_integration.py
├── examples/
│   └── config.example.json
└── logs/
```

**Autonomous behavior:** Claude creates proper project structure without being asked.

### Phase 3: Implementation

#### Step 1: Core Monitoring (monitor.py)

Claude writes well-documented code:

```python
"""
File system monitoring module for detecting changes in watched directories.

This module uses watchdog library to monitor filesystem events and triggers
backup operations when files are modified.
"""

import logging
from pathlib import Path
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler

logger = logging.getLogger(__name__)


class FileChangeHandler(FileSystemEventHandler):
    """Handles file system events and triggers backup operations."""

    def __init__(self, backup_manager):
        """
        Initialize the file change handler.

        Args:
            backup_manager: BackupManager instance to handle backups
        """
        self.backup_manager = backup_manager
        logger.info("FileChangeHandler initialized")

    def on_modified(self, event):
        """
        Called when a file is modified.

        Args:
            event: FileSystemEvent containing event details
        """
        if not event.is_directory:
            try:
                logger.info(f"File modified: {event.src_path}")
                self.backup_manager.backup_file(Path(event.src_path))
            except Exception as e:
                logger.error(f"Failed to backup {event.src_path}: {e}")


class DirectoryMonitor:
    """Monitors a directory for file changes and triggers backups."""

    def __init__(self, watch_path, backup_manager):
        """
        Initialize the directory monitor.

        Args:
            watch_path: Path to directory to monitor
            backup_manager: BackupManager instance
        """
        self.watch_path = Path(watch_path)
        self.backup_manager = backup_manager
        self.observer = Observer()
        self.handler = FileChangeHandler(backup_manager)

        if not self.watch_path.exists():
            raise ValueError(f"Watch path does not exist: {watch_path}")

        logger.info(f"DirectoryMonitor initialized for: {watch_path}")

    def start(self):
        """Start monitoring the directory."""
        self.observer.schedule(
            self.handler,
            str(self.watch_path),
            recursive=True
        )
        self.observer.start()
        logger.info(f"Started monitoring: {self.watch_path}")

    def stop(self):
        """Stop monitoring the directory."""
        self.observer.stop()
        self.observer.join()
        logger.info("Stopped monitoring")
```

**Autonomous behavior:**
- Comprehensive docstrings
- Proper error handling
- Logging throughout
- Type hints where beneficial
- Professional code organization

#### Step 2: Backup Logic (backup.py)

```python
"""
Backup management module for creating timestamped backups of files.
"""

import logging
import shutil
from datetime import datetime
from pathlib import Path

logger = logging.getLogger(__name__)


class BackupManager:
    """Manages file backup operations with timestamping."""

    def __init__(self, backup_root):
        """
        Initialize the backup manager.

        Args:
            backup_root: Root directory for storing backups
        """
        self.backup_root = Path(backup_root)
        self.backup_root.mkdir(parents=True, exist_ok=True)
        logger.info(f"BackupManager initialized with root: {backup_root}")

    def backup_file(self, file_path):
        """
        Create a timestamped backup of the specified file.

        Args:
            file_path: Path to the file to backup

        Returns:
            Path: Path to the created backup

        Raises:
            IOError: If backup operation fails
        """
        file_path = Path(file_path)

        if not file_path.exists():
            raise FileNotFoundError(f"Source file not found: {file_path}")

        # Create timestamp suffix
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")

        # Create backup filename
        backup_name = f"{file_path.stem}_{timestamp}{file_path.suffix}"
        backup_path = self.backup_root / backup_name

        try:
            shutil.copy2(file_path, backup_path)
            logger.info(f"Backed up {file_path.name} to {backup_path}")
            return backup_path
        except Exception as e:
            logger.error(f"Backup failed for {file_path}: {e}")
            raise IOError(f"Failed to backup file: {e}")

    def cleanup_old_backups(self, keep_count=10):
        """
        Remove old backups, keeping only the most recent ones.

        Args:
            keep_count: Number of recent backups to keep per file
        """
        # Group backups by base filename
        backup_groups = {}
        for backup_file in self.backup_root.glob("*"):
            if backup_file.is_file():
                # Extract base name (before timestamp)
                base_name = "_".join(backup_file.stem.split("_")[:-2])
                if base_name not in backup_groups:
                    backup_groups[base_name] = []
                backup_groups[base_name].append(backup_file)

        # Remove old backups for each group
        for base_name, files in backup_groups.items():
            files.sort(key=lambda f: f.stat().st_mtime, reverse=True)
            for old_file in files[keep_count:]:
                try:
                    old_file.unlink()
                    logger.info(f"Removed old backup: {old_file.name}")
                except Exception as e:
                    logger.warning(f"Failed to remove {old_file}: {e}")
```

**Autonomous behavior:**
- Additional useful features added (cleanup_old_backups)
- Robust error handling
- Clear documentation
- Practical implementation

#### Step 3: Tests

Claude automatically writes comprehensive tests:

```python
"""
Unit tests for backup functionality.
"""

import unittest
import tempfile
import shutil
from pathlib import Path
from datetime import datetime
from src.backup import BackupManager


class TestBackupManager(unittest.TestCase):
    """Test cases for BackupManager class."""

    def setUp(self):
        """Set up test fixtures."""
        self.test_dir = tempfile.mkdtemp()
        self.backup_dir = Path(self.test_dir) / "backups"
        self.manager = BackupManager(self.backup_dir)

        # Create test file
        self.test_file = Path(self.test_dir) / "test.txt"
        self.test_file.write_text("test content")

    def tearDown(self):
        """Clean up test fixtures."""
        shutil.rmtree(self.test_dir)

    def test_backup_creates_file(self):
        """Test that backup creates a new file."""
        backup_path = self.manager.backup_file(self.test_file)

        self.assertTrue(backup_path.exists())
        self.assertEqual(backup_path.read_text(), "test content")

    def test_backup_filename_format(self):
        """Test that backup filename includes timestamp."""
        backup_path = self.manager.backup_file(self.test_file)

        self.assertIn("test_", backup_path.stem)
        self.assertTrue(any(char.isdigit() for char in backup_path.stem))

    def test_backup_nonexistent_file_raises_error(self):
        """Test that backing up nonexistent file raises error."""
        fake_file = Path(self.test_dir) / "nonexistent.txt"

        with self.assertRaises(FileNotFoundError):
            self.manager.backup_file(fake_file)

    def test_cleanup_old_backups(self):
        """Test that old backups are removed correctly."""
        # Create multiple backups
        for i in range(15):
            self.manager.backup_file(self.test_file)

        # Cleanup, keeping only 10
        self.manager.cleanup_old_backups(keep_count=10)

        # Count remaining backups
        backup_count = len(list(self.backup_dir.glob("*")))
        self.assertEqual(backup_count, 10)


if __name__ == "__main__":
    unittest.main()
```

**Autonomous behavior:**
- Comprehensive test coverage
- Proper setup/teardown
- Edge case testing
- Professional test organization

### Phase 4: Documentation

Claude creates comprehensive documentation:

```markdown
# File Backup Monitor

Automatically monitor directories for file changes and create
timestamped backups.

## Features

- Real-time file change monitoring
- Automatic timestamped backups
- Configurable watch directories
- Old backup cleanup
- Comprehensive logging
- Command-line interface

## Installation

```bash
pip install -r requirements.txt
```

## Usage

### Basic Usage

```bash
python -m src.cli --watch /path/to/watch --backup /path/to/backups
```

### Configuration File

Create `config.json`:

```json
{
  "watch_path": "/path/to/watch",
  "backup_path": "/path/to/backups",
  "keep_backups": 10
}
```

Run with config:

```bash
python -m src.cli --config config.json
```

## Testing

```bash
python -m pytest tests/
```

## License

MIT License
```

**Autonomous behavior:** Claude creates complete, professional documentation without being asked.

### Phase 5: Testing & Validation

Claude runs tests automatically:

```bash
Running unit tests...
test_backup_creates_file ... ok
test_backup_filename_format ... ok
test_backup_nonexistent_file_raises_error ... ok
test_cleanup_old_backups ... ok

Ran 4 tests in 0.123s
OK

Running integration tests...
test_end_to_end_monitoring ... ok

All tests passed!
```

**Autonomous behavior:** Claude validates the implementation before marking complete.

## Key Plugin Features Demonstrated

1. **Autonomous Architecture**: Creates proper project structure
2. **Best Practices**: Follows Python conventions (PEP 8, docstrings, type hints)
3. **Error Handling**: Comprehensive exception handling and logging
4. **Testing**: Automatic test creation and execution
5. **Documentation**: Professional README and code documentation
6. **Validation**: Tests code before completion
7. **Progress Tracking**: TodoWrite updates throughout

## User Experience

### What You See

```
User: Build a Python CLI tool that monitors a directory...

Claude: I'll help you build a directory monitoring and backup tool.
Let me create a comprehensive implementation plan.

[TodoWrite creates development plan]

Creating project structure...
✓ Project structure created

Implementing core monitoring functionality...
✓ Monitor module complete

Implementing backup logic...
✓ Backup module complete

[... continued updates ...]

Writing tests...
✓ Unit tests complete
✓ Integration tests complete

Running tests...
✓ All tests passing

Claude: The file backup monitor is complete and tested.

Project created in: file_backup_monitor/
- 4 core modules (monitor, backup, config, cli)
- Comprehensive test suite (8 tests, all passing)
- Documentation and usage examples
- Ready for use

To start monitoring:
python -m src.cli --watch /your/path --backup /backup/path
```

### What You Don't Need To Do

- ❌ Tell Claude to create tests
- ❌ Ask Claude to add error handling
- ❌ Request documentation
- ❌ Remind Claude to validate the code
- ❌ Specify project structure

**Claude handles professional development practices automatically.**

## Comparison: With vs Without Plugin

### Without MyManus Plugin

**User:** Build a directory monitoring tool...

**Claude:** Here's a basic implementation:

```python
# Single file, ~50 lines, minimal error handling
import os
import shutil
# ... basic implementation ...
```

- Single file
- Basic functionality only
- Minimal error handling
- No tests
- No documentation
- User must ask for improvements

### With MyManus Plugin

**User:** Build a directory monitoring tool...

**Claude:** [Creates professional project]

- Proper project structure
- Multiple organized modules
- Comprehensive error handling
- Full test suite (automatically written and run)
- Professional documentation
- Logging throughout
- Configuration management
- Additional useful features (cleanup, etc.)
- Validated and tested

**This is the difference between a code snippet and a production-ready tool.**
