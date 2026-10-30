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

- Manual: inspect live device through UI 
	- Might do that at a border for example
	- Take your phone, scroll through social media
	- Not good really
- Backups: offline (e.g. iTunes) or cloud 
- Logical: dump through external interface, e.g. USB 
- Jailbreak OS: obtain root rights, then image 
- Low-level exploits, e.g. before OS boots 
	- BitPixie!
- Physical: open device, connect to JTAG or remove/read Flash memory
	- Physical probing

Police sometimes have the power to ask for pins at threat of jailtime.
# Analysing Disk Images
1. Hash & backup the image 
2. Reorder physical drives to logical groups (RAID, LVM) if necessary 
3. Recover partition tables 
4. Identify file systems 
5. Determine content of file systems 
6. Check for “interesting” files 
7. Create a timeline of events 
8. Check for possibly deleted files 
9. Check for anomalies
# File Forensics
- File type can be determined by extension/metadata/content 
- Find suitable viewers for files (native applications do not show everything) 
- Look for old (deleted) versions of file – only works if file not overwritten in-place 
- Tools often support searching for known hashes and in file contents 
	- Known bad: CSAM, other illicit content via Project VIC or CAID 
	- Known good: e.g. OS and known applications (https://www.nist.gov/itl/csd/secure- systems-and-applications/national-software-reference-library-nsrl) 
	- Keyword search, …
# "Undeleting"
Deleting a file marks block as unused, but data remains until overwritten. So, we can recover them.
- Old android would screenshot when you open task switcher for a thumbnail, which would then be deleted.
- We could, for example, recover camera photos if someone opened the task switcher with their camera open
# Possible anomalies to watch out for
Might see manufactured evidence, which can be noticed by:
- Partition tables with alignment not used by the OS installer: Maybe the partition was fabricated with another tool 
- Unusual ordering of data on the drive: Data may have been copied in one go 
- No fragments of old data in the free space: Drive might have been cleaned and cloned from other system 
- Access pattern of file groups not in sync: Files put in browser cache, but not the cache index 
- Unusual speed/times: Files in the download folder downloaded with the speed of a USB3 HDD while user had slow internet connection 
- Least significant bits of timer values biased