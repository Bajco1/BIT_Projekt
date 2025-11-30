# Modern Honeypot Frameworks - T-Pot Custom Config

Tento repozitár obsahuje konfiguráciu a nasadenie moderného honeypot frameworku **T-Pot** (The All In One Honeypot Platform). Projekt slúži ako funkčný prototyp pre zber Threat Intelligence dát v reálnom čase.

Projekt je súčasťou zadania pre predmet **BIT**.

##  Prehľad architektúry

Riešenie je postavené na Docker kontajneroch riadených cez `docker-compose`. 
Centrálnym bodom je **ELK Stack** (Elasticsearch, Logstash, Kibana) pre vizualizáciu dát.

### Aktívne Honeypoty v tejto konfigurácii:
* **Cowrie:** Interaktívny SSH/Telnet honeypot (zachytáva príkazy útočníka).
* **Dionaea:** Zachytáva útoky na SMB, HTTP, FTP, MSSQL.
* **Conpot:** ICS/SCADA honeypot simulujúci priemyselné systémy (IEC104, IPMI, Kamstrup).
* **Honeytrap:** Monitorovanie TCP/UDP služieb.
* **Heralding:** Honeypot pre odchytávanie prihlasovacích údajov (FTP, POP3, IMAP).
* **Adbhoney:** Simulácia Android zariadení (Android Debug Bridge).
* **Suricata:** Network Security Monitoring (IDS/IPS).
* **Spiderfoot:** OSINT automatizácia.

##  Požiadavky na systém

Pre spustenie tejto konfigurácie je vyžadovaný server/VM s nasledujúcimi parametrami:
* **OS:** Debian 11 (Bullseye) alebo Debian 12 (Bookworm)
* **RAM:** Minimálne 8GB (odporúčané 16GB pre plynulý chod ELK stacku)
* **Disk:** 128GB SSD
* **Sieť:** Verejná IP adresa (pre reálny zber dát), povolené všetky porty.

##  Inštalácia

### 1. Príprava systému
Uistite sa, že máte čistú inštaláciu Debianu a nainštalovaný `git`.

```bash
sudo apt-get update && sudo apt-get install git -y
```
## Custom docker-compose.yml

V tomto repozitári je vložený mnou upravený docker-compose.yml, ktorý som použil na vlastnú inštaláciu honeypotov a opravu chýb a konfliktov v portoch s bežiacimi službami na serveri, preto sa odporúča tento honeypot inštalovať na čistý OS
