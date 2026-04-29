`head` start ki lines, `tail` end ki lines, aur `less` large files ko safely view karne ke liye use hota hai.

====================================================
VIM EDITOR – COMPLETE PRACTICAL 
====================================================

-------------------------
1) WHAT IS VIM?
-------------------------
Vim is a powerful text editor in Linux used for:
- Editing configuration files
- Reading logs
- Working on servers

It is widely used by DevOps engineers and Linux administrators.

-------------------------
2) OPEN / CREATE FILE
-------------------------
Command:
vim filename

Example:
vim test.txt

- If the file does not exist → a new file is created
- If the file exists → it opens for editing

-------------------------
3) VIM MODES (MOST IMPORTANT)
-------------------------
Vim has 3 main modes:

1. Normal Mode
   - For running commands
   - Default mode

2. Insert Mode
   - For typing / writing

3. Command Mode
   - For saving, quitting, searching, and replacing

IMPORTANT:
ESC key → always returns to Normal Mode

-------------------------
4) INSERT MODE (WRITING)
-------------------------
From Normal Mode:

i  → start typing before the cursor  
a  → start typing after the cursor  
o  → open a new line below

After typing, press ESC to return to Normal Mode

-------------------------
5) SAVE & EXIT COMMANDS
-------------------------
:w       → save file  
:q       → quit  
:wq      → save and quit  
:q!      → quit without saving  
:w new.txt → save as a new file

-------------------------
6) CURSOR MOVEMENT
-------------------------
h → left  
l → right  
j → down  
k → up  

gg → go to start of file  
G  → go to end of file

-------------------------
7) DELETE COMMANDS
-------------------------
dd → delete whole line  
x  → delete a single character  
dw → delete a word  

Undo:
u → undo last action

-------------------------
8) COPY (YANK) & PASTE
-------------------------
yy → copy the whole line  
p  → paste below  
P  → paste above

-------------------------
9) SEARCH IN FILE
-------------------------
/word → search for "word"

Example:
/error

n → go to next match  
N → go to previous match

-------------------------
10) SEARCH & REPLACE
-------------------------
:%s/old/new/g → replace all occurrences

Example:
:%s/http/https/g

Meaning: Replace all "old" with "new" in the entire file

-------------------------
11) PRACTICAL WORKFLOW
-------------------------
vim practice.txt  
i  
(type your text)  
ESC  
:w  
:wq

-------------------------
12) REAL DEVOPS USE CASES
-------------------------
sudo vim /etc/hosts  
sudo vim /etc/nginx/nginx.conf  
sudo vim /etc/fstab  
sudo vim /etc/ssh/sshd_config

-------------------------
13) COMMON PROBLEMS & SOLUTIONS
-------------------------
Problem: Vim is stuck  
Solution: Press ESC  

Problem: Cannot type  
Solution: Press i (Insert mode)  

Problem: Cannot exit  
Solution: Use :q!  

-------------------------
14) GOLDEN RULES (REMEMBER)
-------------------------
ESC → Normal mode  
i   → Insert mode  
:w  → Save  
:q  → Quit

====================================================
END
====================================================