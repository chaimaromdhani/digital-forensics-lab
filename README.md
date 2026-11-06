DFIR : investigation d'une compromission par phishing

Simulation d'une attaque par phishing suivie d'une exfiltration de données, puis investigation complète dans un laboratoire isolé. Projet individuel (mini-projet ING-5 SSIR, TEK-UP).

Scénario

Un employé exécute une fausse facture (Facture_Impayee_Octobre2026.exe). L'attaquant prend le contrôle du poste, explore la machine, installe une persistance et exfiltre des fichiers.

Architecture (réseau host-only 10.10.10.0/24)
Machine	Rôle
Kali Linux	Attaquant
Windows 10 (Sysmon + agent Wazuh)	Victime
Ubuntu	Analyste : Wazuh, Suricata, Volatility 3, YARA
Méthodologie

Cycle de réponse à incident NIST SP 800-61. Collecte des preuves selon l'ordre de volatilité, hachage SHA-256 et chaîne de custody. Techniques caractérisées avec MITRE ATT&CK.

Chaîne d'attaque reconstituée
Étape	Technique	Preuve
Phishing	T1566	Fichier piégé
Exécution	T1204	Création de processus (Sysmon)
C2	T1071	Connexion vers 10.10.10.5:4444
Reconnaissance	T1033 / T1087	whoami, net user, ipconfig
Persistance	T1547.001	Clé de registre WindowsUpdate
Exfiltration	T1041	Vol de fichiers via le canal C2
Résultats
Volatility 3 : processus malveillant actif, connexion C2 établie, lancé depuis explorer.exe.
Wazuh + Sysmon : détection de la chaîne d'attaque (alerte de niveau 15 sur le dépôt de l'exécutable, persistance détectée).
Suricata : alertes réseau remontées dans Wazuh.
YARA : règle écrite par moi, qui classe le fichier comme Meterpreter.
Gravité : critique (20/25 en confidentialité), avec plan de confinement, d'éradication et de reprise.
Outils

Metasploit, Sysmon, Magnet RAM Capture, Wazuh, Suricata, Volatility 3, YARA.

Tous les fichiers « sensibles » sont fictifs. Usage pédagogique en réseau isolé.
