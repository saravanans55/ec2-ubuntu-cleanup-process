# ec2-ubuntu-cleanup-process


#System cleanup process along with a reordered execution for optimal performance:

---

### 1. **Check System RAM Usage**  
Use `top` to monitor current RAM and CPU usage in real-time:
```bash
top
```

### 2. **Check Disk Usage by Specific Folder**  
To view the size of a particular folder, use:
```bash
du -sh /path/to/folder
```

### 3. **Check Overall Disk Space Usage**  
Get a summary of overall disk space usage with:
```bash
df -h
```

### 4. **Remove Unused/Old Packages and Cache**  
Clean up old cache and unused packages to free space:
```bash
sudo apt-get clean
sudo apt-get autoremove
```

### 5. **MongoDB Journal Cleanup**  
Before removing MongoDB journals, check the database stats:
```bash
mongo
use admin
db.stats()
```

Stop MongoDB, remove the journal files, and restart MongoDB:
```bash
sudo service mongod stop
sudo rm -rf /var/lib/mongodb/journal/*
sudo service mongod start
```

Clean old system logs to free space:
```bash
sudo journalctl --vacuum-time=1w
```

### 6. **Remove Disabled Snap Packages**  
Clean up disabled Snap packages:
```bash
sudo snap list --all | grep 'disabled' | awk '{print $1, $3}' | while read snapname revision; do sudo snap remove "$snapname" --revision="$revision"; done
```

### 7. **Clear Log Files**  
Remove old log files that may be taking up space:
```bash
sudo rm -rf /var/log/*.gz
sudo rm -rf /var/log/*log
```

---

By executing the commands in this order, you:
- First assess the system's current status (`top`, `du`, `df`).
- Then proceed to clean up unwanted packages and MongoDB journals.
- Finally, remove any disabled snaps and logs to complete the cleanup process.
