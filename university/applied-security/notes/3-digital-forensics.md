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

### DONT
- Boot and copy onto an external drive
- Move disk between systems and copy data
- 