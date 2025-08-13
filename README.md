# SafeNotes

A collection of bash scripts for managing encrypted notes using GPG and Neovim.

A simple way to create and search encrypted notes through your terminal.

## Dependencies

### Required
- **gpg** - GNU Privacy Guard for encryption/decryption
- **neovim (nvim)** - Text editor with GPG plugin support
- **vim-gnupg plugin** - Neovim plugin for transparent GPG encryption/decryption

### Optional
- **fzf** - Required only for interactive file selection in `search-notes`
- **trash** - Used by `new-note` to safely delete empty notes (falls back to `rm` if not available)

## Installation

1. Install dependencies:
   ```bash
   # On Arch Linux
   sudo pacman -S gnupg neovim fzf trash-cli
   
   # On Ubuntu/Debian
   sudo apt install gnupg neovim fzf trash-cli
   
   # On macOS with Homebrew
   brew install gnupg neovim fzf trash
   ```

2. Install vim-gnupg plugin for Neovim. With vim-plug:
   ```vim
   Plug 'jamessan/vim-gnupg'
   ```

3. Make scripts executable:
   ```bash
   chmod +x new-note search-notes
   ```

4. Add scripts to your PATH

## Configuration

Configuration is managed through the `safenotes-config` file. You can customize:

### Environment Variables
- `NOTES_DIR` - Directory where notes are stored (default: `$HOME/Private/notes`)
- `GPG_RECIPIENT` - Email address for GPG encryption 

### File Settings (edit `safenotes-config`)
- `FILE_EXTENSION` - File extension before `.gpg` (`txt` or `md`)
- `TIMESTAMP_FORMAT` - Date/time format for filenames

Example configuration:
```bash
export NOTES_DIR="$HOME/Documents/encrypted-notes"
export GPG_RECIPIENT="your-email@example.com"
```

## Usage

### new-note

Creates and opens a new encrypted note with Neovim.

```bash
# Create note with timestamp only
new-note

# Create note with custom name prefix
new-note "meeting"
```

**Filename format:** `{optional-name}-{timestamp}.{extension}.gpg`

Examples:
- `2024-01-15-143052.txt.gpg`
- `meeting-2024-01-15-143052.txt.gpg`

### search-notes

Searches through all encrypted notes for a given string (case-insensitive).

```bash
# Interactive mode - search and select file to edit
search-notes "project deadline"

# Files-only mode - just list matching filenames
search-notes -f "project deadline"
search-notes --files-only "project deadline"
```

**Interactive mode:** Uses fzf to display matching files with preview, allows selection for editing.

**Files-only mode:** Simply prints the filenames that contain matches.

## File Structure

- `new-note` - Script to create new encrypted notes
- `search-notes` - Script to search through encrypted notes
- `safenotes-config` - Shared configuration file
- `README.md` - This documentation

## Security Notes

- Notes are encrypted with GPG using the configured recipient
- The vim-gnupg plugin handles transparent encryption/decryption in Neovim
- Empty notes are automatically deleted after editing
