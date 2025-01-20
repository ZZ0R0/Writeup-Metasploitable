# Rapport Nmap - Metasploitable 2

Ce document présente les résultats d’un scan Nmap réalisé sur la cible `10.6.0.7`. Ce serveur est une machine **Metasploitable 2**, souvent utilisée à des fins d’entraînement sur les failles de sécurité et l’exploitation de services vulnérables.

---

## Informations Générales

- **Adresse IP**: 10.6.0.7  
- **MAC Address**: BC:24:11:85:BF:FA (Proxmox Server Solutions GmbH)  
- **Type de machine**: Général, système basé sur Linux (noyau 2.6.x)  
- **Distance réseau**: 1 saut  
- **Uptime estimée**: 3.757 jours (~3 jours et 18 heures)  
- **OS détecté**: Linux (2.6.9 - 2.6.33)

---

## Ports Ouverts et Services Identifiés

### 1. FTP (21/tcp)
- **État**: Open  
- **Service**: vsftpd 2.3.4  
- **Authentification anonyme**: Autorisée (`Anonymous FTP login allowed`)  
- **Informations supplémentaires**: vsFTPd 2.3.4 présente une vulnérabilité bien connue (backdoor dans certaines versions compromise).

### 2. SSH (22/tcp)
- **État**: Open  
- **Service**: OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)  
- **Clés SSH**:  
  - DSA 1024  
  - RSA 2048  

### 3. Telnet (23/tcp)
- **État**: Open  
- **Service**: Linux telnetd  

### 4. SMTP (25/tcp)
- **État**: Open  
- **Service**: Postfix smtpd  
- **SSLv2**: Supporté (faible sécurité)  
- **Certificat SSL**:  
  - Subject / Issuer: `ubuntu804-base.localdomain` (périmé depuis 2010)  

### 5. DNS (53/tcp)
- **État**: Open  
- **Service**: ISC BIND 9.4.2  
- **Version**: 9.4.2  

### 6. HTTP (80/tcp)
- **État**: Open  
- **Service**: Apache 2.2.8 (Ubuntu) DAV/2  
- **Titre HTTP**: Metasploitable2 - Linux  
- **Méthodes HTTP prises en charge**: GET, HEAD, POST, OPTIONS  

### 7. SMB (139/tcp et 445/tcp)
- **État**: Open  
- **Service**: Samba smbd 3.0.20-Debian / 3.X - 4.X  
- **Nom NetBIOS**: METASPLOITABLE (Workgroup: WORKGROUP)  
- **Partage réseau**: Peut être vulnérable à un accès non authentifié ou à d’autres faiblesses Samba classiques.  

### 8. Exec (512/tcp), Login (513/tcp), et tcpwrapped (514/tcp)
- **512/tcp (rexec)**: Service exec (netkit-rsh)  
- **513/tcp (rlogin)**: Service login  
- **514/tcp**: tcpwrapped (aucun détail exact sur le service, potentiellement rsh, syslog ou similaire)  

### 9. Java RMI (1099/tcp et 40659/tcp)
- **État**: Open  
- **Service**: GNU Classpath grmiregistry  
- **Version**: Indications d’un registre RMI Java vulnérable (rmi-based exploits)  

### 10. BindShell (1524/tcp)
- **État**: Open  
- **Service**: Metasploitable root shell  
- **Remarque**: Il s’agit d’un bind shell délibérément présent sur Metasploitable pour démontrer une faille critique (accès root à distance).  

### 11. FTP (2121/tcp)
- **État**: Open  
- **Service**: ProFTPD 1.3.1  
- **Remarque**: ProFTPD 1.3.1 peut contenir des vulnérabilités (par exemple, exploit mod_copy).  

### 12. MySQL (3306/tcp)
- **État**: Open  
- **Service**: MySQL 5.0.51a  
- **Authentification**: Par défaut, Metasploitable peut avoir un compte root sans mot de passe.  

### 13. DistCCD (3632/tcp)
- **État**: Open  
- **Service**: distccd v1 (GNU 4.2.4 Ubuntu)  
- **Vulnérabilité connue**: distcc (CVE-2004-2687) permettant l’exécution de commandes arbitraires.  

### 14. PostgreSQL (5432/tcp)
- **État**: Open  
- **Service**: PostgreSQL 8.3.x  
- **SSL/TLS**: Utilise un certificat expiré identique à celui de la partie SMTP.  

### 15. VNC (5900/tcp)
- **État**: Open  
- **Service**: VNC (Protocole 3.3)  
- **Authentification**: VNC Authentication (2) (possibilité de bruteforce).  

### 16. X11 (6000/tcp)
- **État**: Open  
- **Service**: X11 (Access denied)  

### 17. IRC (6667/tcp et 6697/tcp)
- **État**: Open  
- **Service**: UnrealIRCd (3.2.8.1)  
- **Vulnérabilité**: Versions mal configurées d’UnrealIRCd sont sujettes à une backdoor.  

### 18. AJP13 (8009/tcp)
- **État**: Open  
- **Service**: Apache Jserv Protocol v1.3  
- **Usage**: Connecteur Tomcat / Apache (peut être vulnérable si mal configuré).  

### 19. Tomcat (8180/tcp)
- **État**: Open  
- **Service**: Apache Tomcat/Coyote JSP engine 1.1  
- **Version**: Tomcat 5.5 (Interface manager par défaut potentiellement accessible).  

### 20. Ruby DRb (8787/tcp)
- **État**: Open  
- **Service**: Ruby DRb RMI (Ruby 1.8)  
- **Vulnérabilité**: Accès DRb non protégé peut permettre l’exécution de code.  

---

## Scripts et Résultats NSE Pertinents

- **ftp-anon** : La connexion anonyme est autorisée sur le port 21.  
- **ssl-cert (25/tcp, 5432/tcp)** : Certificat expiré, daté de 2010, émis pour `ubuntu804-base.localdomain`.  
- **p2p-conficker** : Aucune infection Conficker détectée.  
- **smb-os-discovery** : Samba version 3.0.20-Debian, nom de la machine `metasploitable.localdomain`.  
- **nbstat** : Nom NetBIOS `METASPLOITABLE`.  

---

## Observations Importantes en Matière de Sécurité

1. **Services multiples et vulnérables** : Metasploitable 2 est intentionnellement vulnérable, avec de nombreux services (FTP, Telnet, SSH, R* services, etc.) configurés de façon peu sécurisée.
2. **Comptes par défaut / pas de mot de passe** : Historiquement, les services MySQL ou PostgreSQL sur Metasploitable peuvent être accessibles avec des mots de passe faibles ou par défaut.
3. **Versions obsolètes** : Les versions de vsFTPd (2.3.4), ProFTPD (1.3.1), Apache, etc. sont connues pour contenir plusieurs vulnérabilités.
4. **Protocole SSLv2** : Toujours supporté sur le service SMTP, ce qui représente un niveau de cryptographie très faible (supprimé dans la plupart des configurations récentes).
5. **Exposition des services r* (rexec, rlogin)** : Ces services sont anciennement considérés comme très peu sûrs (transmissions en clair, éventuelles faiblesses d’authentification).

---

## Recommandations

> **Note :** Dans un environnement de production, ces recommandations seraient essentielles. Dans le cas de Metasploitable 2, le but est l’apprentissage, l’identification d’exploits, et la sensibilisation aux vulnérabilités courantes.

1. **Limiter les services exposés** : Désactiver ou filtrer les ports non essentiels (Telnet, rlogin, rexec, etc.).
2. **Mettre à jour les versions** : Migrer vers des versions récentes (OpenSSH, vsFTPd, ProFTPD, Apache Tomcat, etc.).
3. **Désactiver l’authentification anonyme** : Sur FTP et autres services.
4. **Sécuriser les bases de données** : Mettre en place des mots de passe forts sur MySQL / PostgreSQL, limiter l’accès réseau.
5. **Désactiver SSLv2** : Favoriser TLS1.2+ pour le chiffrement.
6. **Restreindre l’accès RMI / DRb** : Mettre en place l’authentification, limiter l’accès via pare-feu.

---

## Conclusion

Cette analyse met en évidence la présence de nombreux ports ouverts et de services vulnérables, principalement parce que **Metasploitable 2** est volontairement conçu comme une machine de test pour la pratique d’attaques et d’exploitations de failles de sécurité. Dans un cadre d’apprentissage ou de laboratoire, cette machine permet d’exercer différents scénarios d’intrusion et de post-exploitation.

Pour tout environnement en production, les points majeurs incluent la **mise à jour des services**, la **fermeture des ports inutilisés**, la **mise en place de mots de passe forts**, et la **suppression des services obsolètes**.

---

**Fin du rapport**  
