# Conti Ransomware: Análisis Detallado del Grupo y el Malware

> **Nota**: Este documento es puramente informativo y educativo. Describe un grupo de ciberdelincuentes histórico (activo principalmente entre 2019 y 2022). No contiene instrucciones para realizar ataques ni material de explotación.

## Introducción

**Conti** fue uno de los ransomware más exitosos y destructivos de su época. Operaba bajo el modelo **Ransomware-as-a-Service (RaaS)** y estaba vinculado al ecosistema de ciberdelincuencia ruso conocido como **Wizard Spider** (el mismo grupo detrás de TrickBot y Ryuk).

Fue especialmente conocido por:
- Su velocidad de cifrado (multihilo, muy rápido).
- El uso sistemático de **doble extorsión** (robo de datos + cifrado).
- Una estructura casi corporativa (roles, salarios, playbooks internos).
- El impacto en grandes organizaciones, incluyendo sistemas de salud y gobiernos.

El grupo se disolvió formalmente a mediados de 2022 tras filtraciones masivas de chats internos y código fuente, aunque muchos de sus operadores migraron a grupos sucesores.

## Historia y Orígenes

| Fecha | Evento |
|-------|--------|
| Finales de 2019 | Primera aparición de Conti (evolución técnica de Ryuk). |
| 2020 | Se establece como RaaS completo. Se lanza el sitio de filtraciones (leak site). |
| 2021 | Pico de actividad. Ataques de alto perfil (HSE de Irlanda, etc.). Ingresos estimados muy elevados. |
| Febrero 2022 | Conti anuncia apoyo público a la invasión rusa de Ucrania. |
| Febrero-Marzo 2022 | Filtración masiva de ~60.000+ mensajes internos (ContiLeaks) por un investigador ucraniano. |
| Mayo 2022 | Cierre del sitio de filtraciones, paneles de negociación e infraestructura principal. |
| 2022 en adelante | Fragmentación en grupos sucesores (Black Basta, Royal/BlackSuit, Karakurt, etc.). |

Conti nació como sucesor técnico de **Ryuk**, manteniendo técnicas de ofuscación similares pero con un motor de cifrado mucho más rápido y eficiente.

## Modelo de Negocio (RaaS)

Conti funcionaba de forma similar a una empresa:
- Los desarrolladores principales creaban y mantenían el ransomware y la infraestructura.
- Los **afiliados** (affiliates) realizaban las intrusiones y el despliegue.
- A diferencia de muchos otros RaaS (que pagan comisión), Conti a menudo pagaba **salarios fijos** a sus operadores/afiliados.
- Tenían roles definidos: desarrollo, penetración, negociación, soporte, etc.
- Usaban un sitio de filtraciones público (Conti News) para publicar datos de víctimas que no pagaban.

Estimaciones del FBI y otras fuentes hablan de más de 1.000 víctimas y pagos de rescate superiores a 150 millones de dólares (con picos anuales cercanos a 180 millones según algunas estimaciones).

## Metodología de Ataque (TTPs)

Conti era un ransomware **operado por humanos** (human-operated). El cifrado solía ser la última fase tras días o semanas de acceso persistente.

### Acceso inicial
- Phishing (adjuntos maliciosos o enlaces) que desplegaban **TrickBot**, **BazarLoader/BazarBackdoor** o **IcedID**.
- Credenciales robadas o RDP expuestos.
- Explotación de vulnerabilidades en sistemas expuestos.

### Post-explotación y movimiento lateral
- Uso intensivo de **Cobalt Strike**.
- Herramientas como Mimikatz (dump de credenciales), PsExec, WMI, WinRM.
- Explotación de vulnerabilidades conocidas (Zerologon, PrintNightmare, EternalBlue en algunos casos).
- Enumeración de Active Directory, Kerberoasting, DCSync, etc.
- Desactivación de herramientas de seguridad y eliminación de copias de seguridad (shadow copies).

### Exfiltración
- Herramientas como **Rclone** para subir datos a almacenamiento en la nube (MEGA, etc.) antes del cifrado.

### Cifrado e impacto
- Motor multihilo muy rápido (hasta 32 hilos, uso de I/O Completion Ports).
- Cifrado AES-256 por archivo + clave RSA embebida (específica por víctima).
- Uso de Windows Restart Manager para desbloquear archivos abiertos.
- Exclusión de ciertas extensiones (.exe, .dll, .lnk) para mantener la operatividad parcial del sistema.

### Doble extorsión
1. Robo de datos sensibles.
2. Cifrado de sistemas.
3. Amenaza de publicar los datos en el leak site si no se paga.

## Ataques Notables

- **HSE (Health Service Executive) de Irlanda** (mayo 2021): Paralizó gran parte del sistema sanitario nacional. Costes de recuperación estimados en decenas o cientos de millones. Conti llegó a ofrecer el descifrador gratis en un momento, pero manteniendo la amenaza de los datos.
- **Gobierno de Costa Rica** (abril-mayo 2022): Ataque a múltiples agencias gubernamentales. El país declaró estado de emergencia nacional.
- Numerosos hospitales, empresas y administraciones públicas en EE.UU., Europa y otros lugares.

## Las Filtraciones de 2022 (ContiLeaks)

Tras el anuncio de Conti de apoyo a Rusia en la invasión de Ucrania (febrero 2022), un investigador de seguridad ucraniano (infiltrado o con acceso) filtró:
- Decenas de miles de mensajes de chats internos (Jabber/XMPP y otros).
- Código fuente del ransomware.
- Manuales y playbooks internos.
- Información sobre infraestructura.

Esto expuso la estructura interna del grupo, sus métodos, negociaciones y hasta discusiones políticas. La marca Conti quedó gravemente dañada y el grupo cerró su infraestructura principal en mayo de 2022.

## Sucesores

Los operadores y afiliados no desaparecieron. Se redistribuyeron en nuevos grupos que heredaron técnicas, herramientas y personal:
- **Black Basta**
- **Royal** (luego BlackSuit)
- **Karakurt** (más enfocado en extorsión pura)
- Otros grupos relacionados o con solapamientos (Akira, etc.)

## Aspectos Técnicos Resumidos

- Plataforma principal: Windows.
- Ofuscación de strings y API calls.
- Cifrado rápido con claves únicas por archivo.
- Uso de living-off-the-land (LOLBins) y herramientas dual-use (Cobalt Strike, Mimikatz, Rclone, etc.).
- Identificador MITRE ATT&CK: Software S0575 (Conti), vinculado al grupo Wizard Spider (G0102).

## Conclusión

Conti representó el pico de la profesionalización del ransomware a escala industrial. Combinó técnicas avanzadas, una estructura organizativa sólida y el modelo de doble extorsión. Su colapso público por filtraciones internas es uno de los casos más estudiados de cómo un grupo de ciberdelincuencia puede implosionar por factores políticos y operativos.

Muchas de las técnicas que usó siguen siendo relevantes en grupos actuales.

---

### Referencias principales
- MITRE ATT&CK (S0575 Conti)
- Advisories de CISA/FBI/NSA
- Informes de SentinelOne, Cisco Talos, Flashpoint, Wired, KrebsOnSecurity y otros sobre ContiLeaks
- Análisis públicos de la evolución técnica de Ryuk → Conti

*Documento creado para fines educativos en el repositorio bitacora / Conti-RASS.*
