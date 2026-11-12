# TryHackMe - Notas y Writeups

Notas de aprendizaje y writeups de rooms y retos de [TryHackMe](https://tryhackme.com), enfocados en el path **Cyber Security 101** y prácticas de ciberseguridad ofensiva y defensiva.

> ⚠️ **Spoilers:** las flags, contraseñas, hashes y respuestas finales están omitidos (`[REDACTADO]`). Aquí se explica el *cómo*: metodología y comandos.

## Qué hay aquí

| Carpeta / archivo | Contenido |
|-------------------|-----------|
| [`PROGRESO.md`](PROGRESO.md) | Bitácora con fecha de cada room completado |
| [`CHEATSHEET.md`](CHEATSHEET.md) | Chuleta de comandos: recon, cracking, shells, privesc |
| [`rooms/`](rooms/) | Notas de conceptos por room |
| [`writeups/`](writeups/) | Writeups completos de retos resueltos |

## Rooms con notas

| Room | Tema |
|------|------|
| [John the Ripper: The Basics](rooms/john-the-ripper-the-basics.md) | Cracking de hashes, reglas, patrón `*2john` |
| [Hydra](rooms/hydra.md) | Fuerza bruta a formularios web y SSH |
| [Gobuster: The Basics](rooms/gobuster-the-basics.md) | Enumeración de directorios, DNS y vhosts |
| [Blue](rooms/blue.md) | EternalBlue (MS17-010) con Metasploit |
| [Web Application Basics](rooms/web-application-basics.md) | HTTP, métodos, status codes, security headers |
| [Moniker Link (CVE-2024-21413)](rooms/moniker-link-cve-2024-21413.md) | Bypass en Outlook y captura de netNTLMv2 |

## Capstone: Cyber Security Contractor

Serie de 7 contratos cronometrados (10 preguntas cada uno), de dominios distintos. Los 7 completados.

| # | Contrato | Dominio |
|---|----------|---------|
| 1 | [Arian Retail Group](writeups/cyber-security-contractor/arian-retail-group.md) | Auditoría de configuración Windows |
| 2 | [NovaByte Analytics](writeups/cyber-security-contractor/novabyte-analytics.md) | Auditoría de configuración Linux |
| 3 | [TryMapMe](writeups/cyber-security-contractor/trymapme.md) | Escaneo de red con Nmap |
| 4 | [AstraVault](writeups/cyber-security-contractor/astravault.md) | Web app pentest (IDOR, XSS, SQLi, JWT) |
| 5 | [SkyLedger Cloud](writeups/cyber-security-contractor/skyledger-cloud.md) | Blue team: logs, IDS (Snort), pcap |
| 6 | [SentinelWorks](writeups/cyber-security-contractor/sentinelworks.md) | Explotación encadenada (web → SSH → root) |
| 7 | [Ledger Solutions](writeups/cyber-security-contractor/ledger-solutions.md) | Análisis estático de malware |

## Pendientes de documentar
Shells Overview · Bug bounty (práctica de recon) · SOC Simulator (sesiones hechas, writeup pendiente).

## Herramientas
`nmap` · `gobuster` · `hydra` · `john` · `Metasploit` · `Burp Suite` · `Wireshark` · `Snort` · `Splunk` · `Responder`

## Relacionado
Writeups de web hacking en [OverTheWire-Writeups](https://github.com/fbustamante55/OverTheWire-Writeups) (Natas).

## Aviso
Material educativo para estudio personal. Las IPs y valores son específicos de cada despliegue de VM y cambian al re-desplegar. Uso exclusivo en entornos autorizados (labs de TryHackMe).
