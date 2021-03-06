# MS17-010

This repository is for public my work on MS17-010. I have no plan to do any support. **All support issues will not get response from me**.

## What you'll need to run this
* apt install python-impacket (see troubleshooting tips at bottom of this guide if you run into other issues)
* A reverse.exe file in your / directory (think msfvenom) - if you're using zzz_exploit.py

## Usage
* python zzz_exploit.py <ip> [pipe_name]

## Files

 * **BUG.txt** MS17-010 bug detail and some analysis
 * **checker.py** Script for finding accessible named pipe
 * **eternalblue_exploit7.py** Eternalblue exploit for windows 7/2008
 * **eternalblue_exploit8.py** Eternalblue exploit for windows 8/2012 x64
 * **eternalblue_poc.py** Eternalblue PoC for buffer overflow bug
 * **eternalblue_kshellcode_x64.asm** x64 kernel shellcode for my Eternalblue exploit. This shellcode should work on Windows Vista and later
 * **eternalblue_kshellcode_x86.asm** x86 kernel shellcode for my Eternalblue exploit. This shellcode should work on Windows Vista and later
 * **eternalblue_sc_merge.py** Script for merging eternalblue x86 and x64 shellcode. Eternalblue exploit, that support both x86 and x64, with merged shellcode has no need to detect a target architecture
 * **eternalchampion_leak.py** Eternalchampion PoC for leaking info part
 * **eternalchampion_poc.py** Eternalchampion PoC for controlling RIP
 * **eternalchampion_poc2.py** Eternalchampion PoC for getting code execution
 * **eternalromance_leak.py** Eternalromance PoC for leaking info part
 * **eternalromance_poc.py** Eternalromance PoC for OOB write
 * **eternalromance_poc2.py** Eternalromance PoC for controlling a transaction which leading to arbitrary read/write
 * **eternalsynergy_leak.py** Eternalsynergy PoC for leaking info part
 * **eternalsynergy_poc.py** Eternalsynergy PoC for demonstrating heap spraying with large paged pool
 * **infoleak_uninit.py** PoC for leaking info from uninitialized transaction data buffer
 * **mysmb.py** Extended Impacket SMB class for easier to exploit MS17-010 bugs
 * **npp_control.py** PoC for controlling nonpaged pool allocation with session setup command
 * **zzz_exploit.py** Exploit for Windows 2000 and later (requires access to named pipe)


## Anonymous user

Anonymous user (null session) get more restriction on default settings of new Windows version. To exploit Windows SMB without authentication, below behavior should be aware.

* Since Windows Vista, default settings does not allow anonymous to access any named pipe
* Since Windows 8, default settings does not allow anonymous to access IPC$ share (IPC$ might be acessible but cannot do much)


## About NSA exploits

* **Eternalblue** requires only access to IPC$ to exploit a target while other exploits require access to named pipe too. So the exploit always works against Windows < 8 in all configuration (if tcp port 445 is accessible). However, Eternalblue has a chance to crash a target higher than other exploits.
* **Eternalchampion** requires access to named pipe. The exploit has no chance to crash a target.
* **Eternalromance** requires access to named pipe. The exploit can target Windows < 8 because the bug for info leak is fixed in Windows 8. The exploit should have a chance to crash a target lower than Eternalblue. I never test a reliable of the exploit.
* **Eternalsynergy** requires access to named pipe. I believe this exploit is modified from Eternalromance to target Windows 8 and later. Eternalsynergy uses another bug for info leak and does some trick to find executable memory (I do not know how it works because I read only output log and pcap file).

## Troubleshooting
* If you get an error called "ImportError: No module named v5" , clone impacket from here and dump it in the same directory as the python file: https://github.com/SecureAuthCorp/impacket
* If you get "ImportError: No module named pyasn1.type.univ", pip install pyasn1
* If you get "ImportError: No module named Cryptodome.Hash", pip install pycryptodomex
* If you get "impacket.smb.SessionError: SMB SessionError: STATUS_ACCESS_DENIED({Access Denied}", the script is working but your target may not be vulnerable, check with metasploit module scanner/smb/smb_ms17_010 (you should probably have checked first before you got to this step :D)


## Disclaimer

This work is for educational purposes only and should be used at most for Pentesting. Please respect your local laws.
