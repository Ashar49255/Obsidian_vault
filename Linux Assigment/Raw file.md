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
# Creating Large Files in Linux (Quick Guide)

## 1️⃣ `fallocate` — **Fastest Method**
**Meaning:**  
Tell the system to reserve disk space immediately.
fallocate -l 5G file.raw
### What happens?
- A 5GB file is created **instantly**
- Disk space is **actually occupied**
- No real data is written (space is just reserved)
**Use when:**
- You need a large file quickly
- VM disks, labs, testing, practice

---

## 2️⃣ `dd` — **Writes Real Data**
**Meaning:**  
Write actual data (usually zeros) into the file.
dd if=/dev/zero of=file.raw bs=1G count=5
### Breakdown:
- `if=/dev/zero` → source of zero data  
- `of=file.raw` → output file   
- `bs=1G` → write 1GB at a time  
- `count=5` → write 5 times 

### Result:
- A real 5GB file  
- Every byte contains data (zeros)   
- **Slower**, but most realistic
**Use when:**
- Disk performance testing   
- Storage labs   
- Interview-preferred example 
---

## 3️⃣ `truncate` — **Fake Size (Sparse File)** 😄
**Meaning:**  
Set file size without allocating disk space yet.
truncate -s 5G file.raw
### What happens?
- File _appears_ to be 5GB   
- Disk space is **not used immediately**   
- Space is allocated only when data is written    

**Use when:**
- Dummy files    
- Script testing
- Quick demos
---

## 🔍 Verify the Difference
ls -lh file.raw   # shows file size
du -h file.raw    # shows actual disk usage

### Example Result

|Method|ls size|du usage|
|---|---|---|
|truncate|5G|0|
|fallocate|5G|5G|
|dd|5G|5G|

---

## 🧠 Easy Memory Formula
- ⚡ **fallocate** → fast + real space
- 🐢 **dd** → slow + real data
- 🎭 **truncate** → fake size (sparse file)