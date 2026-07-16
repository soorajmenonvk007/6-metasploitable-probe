Metasploitable Vulnerability Assessment Lab

A hands-on penetration testing lab performed against Metasploitable in an isolated VirtualBox environment. This exercise covers VM setup, automated vulnerability scanning, web server enumeration, and manual banner grabbing.

Lab Environment

ComponentDetailsAttacker VMKali LinuxTarget VMMetasploitable 2HypervisorVirtualBoxNetwork ModeHost-Only / Internal Network (isolated from LAN/internet)Target IP(s)192.168.0.103 / 192.168.0.108


⚠️ Metasploitable is intentionally vulnerable. It should never be exposed to a bridged network or the public internet.



Objectives


Deploy Metasploitable in a local, isolated lab
Run a full vulnerability scan using OpenVAS
Export scan results as a PDF report
Run a Nikto web server scan and export an HTML report
Manually grab a service banner using Netcat


Tools Used


VirtualBox — VM hosting and network isolation
OpenVAS (Greenbone Vulnerability Manager) — automated vulnerability scanning
Nikto — web server misconfiguration/vulnerability scanner
Netcat (nc) — manual banner grabbing


Deliverables

FileDescriptionopenvas_scan_metasploitable_vm.pdfFull OpenVAS scan report (67 findings after filtering)nikto_report_html.htmNikto scan of the Metasploitable web serverScreenshot_...pngNetcat FTP banner grab (port 21)

Key Findings

OpenVAS — Critical/High Highlights


vsftpd 2.3.4 backdoor (CVE-2011-2523) — shell on port 6200/tcp
Ingreslock backdoor on port 1524/tcp — passwordless root shell
rlogin / rexec — passwordless root access
UnrealIRCd 3.2.8.1 — authentication spoofing vulnerability
DistCC RCE (CVE-2004-2687) — arbitrary command execution
Java RMI insecure default config — remote code execution
MySQL / PostgreSQL default credentials — root access with empty/weak password
VNC weak password — brute-forced in scan
Samba MS-RPC RCE (CVE-2007-2447)
Apache Tomcat AJP "Ghostcat" (CVE-2020-1938)
Multiple outdated web apps (TWiki, phpMyAdmin, jQuery) with known XSS/CSRF/LFI issues
OS is Ubuntu 8.04 — end-of-life since 2013, no security patches


Nikto — Web Server Highlights


Apache/2.2.8 and PHP/5.2.4 — severely outdated
phpinfo.php exposed — leaks system/config details
PHP Easter eggs accessible via special query strings
Directory indexing enabled on /doc/, /icons/, /test/
TRACE method enabled — vulnerable to Cross-Site Tracing (XST)
Missing security headers: CSP, HSTS, X-Content-Type-Options, Referrer-Policy, Permissions-Policy
mod_negotiation with MultiViews enabled — allows filename brute-forcing
phpMyAdmin reachable without host restrictions


Netcat Banner Grab

$ nc -nv 192.168.0.108 21
(UNKNOWN) [192.168.0.108] 21 (ftp) open
220 (vsFTPd 2.3.4)

Confirms the FTP service is running the backdoored vsftpd 2.3.4 build.
