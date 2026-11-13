# Cómo se movían

Lo que más me ha llamado la atención es que no era un ransomware que entra y cifra en minutos. Era gente que se tomaba su tiempo.

Entraban (phishing, RDP, alguna vulnerabilidad), se quedaban semanas, se movían con calma usando Cobalt Strike, Mimikatz, Rclone para sacar los datos, desactivaban las defensas, borraban las copias de seguridad… y al final soltaban el cifrador, que iba muy rápido porque estaba pensado para eso.

El cifrado en sí era lo de menos. Lo importante era todo lo que había pasado antes: el acceso silencioso, el movimiento lateral, la exfiltración. Cuando llegaba el cifrado ya tenían los datos y el control.

Eso es lo que da un poco de miedo. No es un virus automático. Es gente que estudia la red y elige el momento.

Herramientas que se repiten mucho en los informes: Cobalt Strike, Mimikatz, Rclone, PsExec, y el propio binario de Conti (multihilo, AES-256 + RSA).
