# iTerm2 ZModem File Transfer Configuration

On macOS, configure iTerm2 with ZModem protocol to enable convenient file upload and download operations with remote servers. No need for additional FTP or SCP commands - simply use `rz` and `sz` commands directly in the terminal to complete file transfers.

## Features

- 🚀 **Convenient Transfer**: Use `rz`/`sz` commands directly in SSH sessions
- 📁 **Graphical Selection**: Select files/folders through macOS native file picker
- ⚡ **Auto Trigger**: Automatically recognize transfer requests based on iTerm2's Trigger feature
- 🔄 **Bidirectional Transfer**: Support both upload (sz) and download (rz) operations
- 💾 **Binary Safe**: Support transfer of various file types

## Installation Steps

### 1. Install lrzsz Tools

First, install the lrzsz package that supports `rz` and `sz` commands:

```bash
brew install lrzsz
```

> 💡 **Tip**: First-time installation may take a while, please be patient.

### 2. Copy Script Files

Copy the two script files from the project to the system path:

```bash
# Copy send file script
sudo cp iterm2-send-zmodem.sh /usr/local/bin/

# Copy receive file script
sudo cp iterm2-recv-zmodem.sh /usr/local/bin/
```

### 3. Set Script Permissions

Add execute permissions to the script files:

```bash
sudo chmod +x /usr/local/bin/iterm2-send-zmodem.sh
sudo chmod +x /usr/local/bin/iterm2-recv-zmodem.sh
```

> ⚠️ **Security Tip**: Use `chmod +x` instead of `chmod 777` to avoid granting excessive permissions.

### 4. Configure iTerm2 Triggers

Configure triggers in iTerm2 to automatically recognize file transfer requests:

1. Open iTerm2 Preferences: `iTerm2` → `Preferences` (or press `⌘,`)
2. Select `Profiles` → `Default` → `Advanced`
3. Find the `Triggers` section and click the `Edit` button
4. Add the following two triggers:

#### Trigger Configuration

**First Trigger (File Upload - sz command):**
- **Regular Expression**: `rz waiting to receive.\*\*B0100`
- **Action**: `Run Silent Coprocess`
- **Parameters**: `/usr/local/bin/iterm2-send-zmodem.sh`
- **Instant**: ✅ Check

**Second Trigger (File Download - rz command):**
- **Regular Expression**: `\*\*B00000000000000`
- **Action**: `Run Silent Coprocess`
- **Parameters**: `/usr/local/bin/iterm2-recv-zmodem.sh`
- **Instant**: ✅ Check

![Configuration Example](imgs/01.png)

> 📝 **Note**: These triggers monitor terminal output and automatically launch the corresponding scripts when specific ZModem transfer protocol strings are detected.

## Usage

After configuration, you can use the following commands on remote servers connected via SSH:

### Upload Files to Server (sz command)

Enter in the remote server terminal:

```bash
sz filename
```

- The system will automatically open the macOS file picker
- Select the file you want to upload
- The file will be automatically transferred to the server's current directory

### Download Files from Server (rz command)

Enter in the remote server terminal:

```bash
rz
```

- The system will automatically open the macOS folder picker
- Select the local directory to save the file
- Files from the server will be automatically downloaded to the specified directory

## Important Notes

- ⚠️ **Server Requirements**: Remote server must have `lrzsz` package installed (`yum install lrzsz` or `apt-get install lrzsz`)
- 🔒 **Permission Issues**: Ensure script files have execute permissions
- 📁 **Path Issues**: Script path must be `/usr/local/bin/`, otherwise modify trigger parameters
- 🌐 **Network Stability**: Keep network connection stable when transferring large files
- 📝 **Filename Restrictions**: Avoid using filenames with special characters

## Troubleshooting

### Issue: Triggers Not Working

**Solutions:**
1. Check if trigger configuration is correct
2. Verify script file path and permissions
3. Restart iTerm2 application

### Issue: File Transfer Interrupted

**Solutions:**
1. Check network connection stability
2. Confirm lrzsz is properly installed on server
3. Try transferring smaller files for testing

### Issue: Permission Denied

**Solutions:**
```bash
# Reset script permissions
sudo chmod +x /usr/local/bin/iterm2-*.sh

# Check if scripts exist
ls -la /usr/local/bin/iterm2-*.sh
```

## Script Description

### iterm2-send-zmodem.sh
- **Function**: Handle file upload (triggered by sz command)
- **Workflow**: Detect iTerm version → Open file picker → Send file using sz command

### iterm2-recv-zmodem.sh
- **Function**: Handle file download (triggered by rz command)
- **Workflow**: Detect iTerm version → Open folder picker → Receive file using rz command

## Compatibility

- ✅ **macOS**: 10.12+ 
- ✅ **iTerm2**: 3.0+
- ✅ **iTerm**: 2.0+ (Legacy version)
- ✅ **Server**: Linux/Unix systems with lrzsz support

## License

This project is based on open source license, script files are contributed by GitHub community.

---

**Enjoy convenient file transfer experience!** 🎉
