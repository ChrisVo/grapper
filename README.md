# Grapper

Grapper is a Python script that helps you browse files in a directory, select multiple files using `fzf`, and generate a Markdown-formatted output of their contents. Optionally, it can copy the output to your system clipboard. The script is designed to exclude directories specified via an environment variable and works across macOS, Windows, and Linux.

## Quick Install

Download and run the script directly from GitHub:

```bash
curl -L https://raw.githubusercontent.com/ChrisVo/grapper/main/grapper -o grapper
chmod +x grapper
./grapper <directory>
```

**Windows Users**: Run `python grapper <directory>` instead of `./grapper`.

**Note**: You’ll need `fzf` installed. See [Requirements](#requirements) for details.

## Features

- Browse and select files in a specified directory using `fzf`.
- Exclude directories from the search using the `GRAPPER_EXCLUDE` environment variable.
- Generate a Markdown document with file contents, using file paths as headers.
- Copy the generated Markdown to the clipboard (platform-dependent).
- Cross-platform clipboard support (macOS: `pbcopy`, Windows: `clip`, Linux: `xclip` or `xsel`).

## Requirements

- **Python 3.x**: The script uses Python 3 and is compatible with modern versions.
- **`fzf`**: A command-line fuzzy finder (must be installed and available in your PATH).
- **Clipboard Tools (optional)**:
  - macOS: No additional tools required (`pbcopy` is built-in).
  - Windows: No additional tools required (`clip` is built-in).
  - Linux: Either `xclip` or `xsel` must be installed for clipboard functionality.

## Installation

1. Download the script using the `curl` command above.
2. Make it executable (Linux/macOS):
   ```bash
   chmod +x grapper
   ```
3. Ensure `fzf` is installed:
   - On macOS: `brew install fzf`
   - On Linux: `sudo apt-get install fzf` (Ubuntu/Debian) or equivalent
   - On Windows: Install via package manager (e.g., `choco install fzf`) or manually
4. (Optional) Install clipboard tools for Linux:
   - `sudo apt-get install xclip` or `sudo apt-get install xsel`

## Usage

Run the script by providing a directory as an argument:

```bash
grapper <directory>
```

### Steps

1. The script changes to the specified directory.
2. It uses `find` to list all files, excluding directories specified in the `GRAPPER_EXCLUDE` environment variable.
3. `fzf` opens an interactive interface:
   - Use arrow keys or type to filter files.
   - Press `Space` to toggle selection of multiple files.
   - Press `Enter` to confirm your selection.
4. The script generates Markdown output with the contents of selected files.
5. You’re prompted to copy the output to the clipboard (`y` or `yes` to confirm).

### Example

```bash
export GRAPPER_EXCLUDE="node_modules venv"  # Exclude these directories
grapper ./my_project
```

- Select files in `fzf`, press `Enter`.
- Output might look like:

  ```
  ## src/main.py

  print("Hello, world!")

  ## src/utils.py

  def helper():
      return 42

  ## END OF FILE
  ```

- Enter `y` to copy to the clipboard.

## Environment Variables

- **`GRAPPER_EXCLUDE`**: A space-separated list of directory names to exclude from the file search (e.g., `node_modules venv .git`).

## Platform-Specific Notes

- **macOS**: Uses `pbcopy` for clipboard support.
- **Windows**: Uses `clip` for clipboard support.
- **Linux**: Requires `xclip` or `xsel`. If neither is installed, clipboard copying will fail with an error message.

## Error Handling

- If the directory doesn’t exist: `Error: <directory> is not a directory`.
- If `fzf` isn’t installed: `Error: fzf is not installed`.
- If no files are selected: `No files selected`.
- If a file can’t be read: `Skipping <file>: <error>`.
