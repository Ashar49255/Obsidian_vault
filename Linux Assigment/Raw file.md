<font color="#c00000">What is Raw file?</font>
Raw file = like empty hard disk
In this file not Text 
like nano , vim , cat is not work in Raw file

<font color="#ff0000">why we are not open Raw File?</font>
because this is binary disk data 

<font color="#ff0000">Exact Raw file use?</font>
raw file is use for virtual disk, practice store, like we use AWS EBS / GCP Disk



| 1   | A raw file is a binary file used as a virtual disk.                |
| --- | ------------------------------------------------------------------ |
| 2   | **It must be formatted with a filesystem and mounted before use.** |
# Real-World Mapping (important)

| Lab          | Real World      |     |
| ------------ | --------------- | --- |
| file.raw     | AWS EBS         |     |
| mkfs         | Format disk     |     |
| mount        | Attach disk     |     |
| /mnt/rawdisk | EC2 mount point |     |
