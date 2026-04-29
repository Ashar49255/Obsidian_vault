
## 🔹 Command
sudo chattr +i filename
## 🔹 Purpose
Make a file **IMMUTABLE** (locked).

---

## 🔹 What `+i` Does

✔ Prevents **modify**  
✔ Prevents **delete**  
✔ Prevents **rename / move**  
✔ Prevents **chmod / chown**

➡ Applies to **all users including root**

---

## 🔹 What `+i` Does NOT Do

❌ Does NOT stop **reading** the file  
❌ Does NOT provide secrecy

➡ **Root can still read the file**

---

## 🔓 Remove Immutable

sudo chattr -i filename

---

## 🔍 Check Attribute

lsattr filename

---

## 🔹 Use Cases (DevOps / Production)

- Protect critical configs (`/etc/resolv.conf`)
    
- Lock production `.env` files
    
- Prevent accidental `rm -rf`
    
- Protect files from automation/scripts
    

---

## 🔹 When NOT to Use

- Frequently updated files
    
- When **confidentiality** is required  
    ➡ Use **encryption** instead
    

---

## 🔐 Comparison (Very Important)

|Feature|chmod|ACL|chattr +i|Encryption|
|---|---|---|---|---|
|Control access|✅|✅|❌|❌|
|Prevent modify|❌|❌|✅|✅|
|Stop root modify|❌|❌|✅|✅|
|Stop root read|❌|❌|❌|✅|