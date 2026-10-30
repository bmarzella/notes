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
	- Asuming we want to freeze and don't care about volatile
- Document state of the system, peripherals, serial numbers, location, date/time, internal connection of disks 
- Remove hard disks 
- Use a hardware write blocker 
	- Prevents writing to hard disk
- Use a second system to take the image with dedicated software
- Take an image of each disk separately 
- Store hash of the image, store securely
	- Let's us verify nothing has changed
- Run your full analysis only on the images you took, reassemble RAID or similar storage in software
### Read-only access to a filesystem
- Just don’t modify something willingly 
	- (Your operating system will still modify the access time of the files) 
- Mount the filesystem read-only 
	- (might still trigger something like auto-defragmentation) 
- Use write-barriers on the operating system 
	- (The OS might still change power-management settings on the storage) 
- Add a hardware write blocker 
	- (A hardware RAID controller after the blocker might start a rebuild) 
- Bypass hardware RAID controller, take images 
	- (The disk itself might still start to shuffle around free/unused blocks) 
- Bypass storage controller on the hardware, sometimes possible to get a dump of the drive rather than what the chip on the drive says
	- (That’s pretty good, but often difficult)
### Write Blockers
- Prevents writes to a medium being imaged
- Often combined with imagine options

## Imaging Mobile Devices
More difficult
- Shutdown usually locks it
	- Need exploits or a backdoor to get around, murky stuff
- Remote wipe features
- Some devices have memory integrated into PCB, can't just take it out

