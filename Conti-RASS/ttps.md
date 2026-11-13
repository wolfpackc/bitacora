# TTPs y metodología de ataque

Conti era un ransomware **operado por humanos**. El cifrado era la última fase tras días o semanas de acceso persistente y movimiento lateral.

## Cadena de ataque típica

1. **Acceso inicial**
   - Phishing con adjuntos maliciosos (Excel, etc.) que desplegaban TrickBot, BazarLoader o IcedID.
   - Credenciales RDP robadas o VPN comprometidas.
   - Explotación de vulnerabilidades en sistemas expuestos (ProxyShell, Fortinet, etc.).

2. **Establecimiento de foothold y C2**
   - Despliegue de **Cobalt Strike** Beacons.
   - Persistencia mediante tareas programadas, servicios o herramientas de acceso remoto (AnyDesk).

3. **Reconocimiento y escalada**
   - Enumeración de Active Directory (BloodHound, PowerView, nltest, net).
   - Dump de credenciales (Mimikatz, LSASS dump).
   - Kerberoasting, DCSync, Zerologon (CVE-2020-1472).
   - PrintNightmare en algunos casos.

4. **Movimiento lateral**
   - PsExec, WMI, WinRM, RDP con credenciales robadas.
   - Propagación a controladores de dominio y servidores críticos.

5. **Exfiltración**
   - **Rclone** para sincronizar datos a almacenamiento en la nube (MEGA principalmente).

6. **Preparación del impacto**
   - Desactivación de antivirus y EDR.
   - Eliminación de shadow copies (`vssadmin`).
   - Parada de servicios (bases de datos, backups, correo, etc.).

7. **Cifrado**
   - Motor multihilo (hasta 32 hilos) usando I/O Completion Ports.
   - AES-256 por archivo + clave RSA embebida (específica por víctima).
   - Uso de Windows Restart Manager para desbloquear archivos abiertos.
   - Deja nota `CONTI_README.txt`.

## Herramientas frecuentes

| Categoría | Herramientas |
|------------|--------------|
| Acceso inicial | TrickBot, BazarLoader, IcedID, phishing |
| C2 / Post-explotación | Cobalt Strike |
| Credenciales | Mimikatz, LSASS dump, Kerberoasting |
| Movimiento lateral | PsExec, WMI, RDP |
| Exfiltración | Rclone |
| Cifrado | Binario Conti propio |

## Características técnicas del cifrador

- Ofuscación de strings y API calls (XOR y rutinas personalizadas).
- Mutex `CONTI`.
- Exclusión de ciertas extensiones (.exe, .dll, .lnk) para no romper completamente el sistema.
- Velocidad muy alta comparada con otros ransomware de la época.

MITRE ATT&CK: Software **S0575** (Conti), vinculado a Wizard Spider (**G0102**).
