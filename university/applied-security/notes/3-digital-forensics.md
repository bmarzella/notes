![[week15-forensics.pdf]]

# Filesystems vs. block
## Filesystem
- Abstraction of files
- Stores metadata
- Filesystem = software
- Can get filesystem from raw block device image
## Block
- Ideally want a block device image
- No abstraction
- Deleted blocks may remain if not overwritten
# Filesystem Forensics
- Main goal: Determine what is currently stored in a filesystem 
- Determine how and when that state was created
- Possibly determine previous contents of the storage 
- Make sure this state was not fabricated 
- Look for anomalies in the system 
- DO NOT MODIFY ANYTHING (and keep log of what and why if unavoidable)

### DON'T
- Boot and copy onto an external drive
- Move disk between systems, mount filesystem and copy data
- Move disk between systems, mount then take image
All of these will change and lose data
### DO
- Shut down
	- If we want to freeze and don't care about volatile
- Document state of the system, peripherals, serial numbers, location, date/time, internal connection of disks 
- Remove hard disks 
- Use a hardware write blocker 
	- Prevents writing to hard disk
- Use a second system to take the image with dedicated software
- Take an image of each disk separately 
- Store hash of the image, store securely
- Run your full analysis only on the images you took, reassemble RAID or similar storage in software
