# Modern Honeypot Frameworks - T-Pot Custom Config

Tento repozitár obsahuje konfiguráciu a nasadenie moderného honeypot frameworku T-Pot (The All In One Honeypot Platform). Projekt slúži ako funkčný prototyp pre zber Threat Intelligence dát v reálnom čase.

Projekt je súčasťou zadania pre predmet BIT.

## Prehľad architektúry

Riešenie je postavené na Docker kontajneroch riadených cez `docker-compose`. Centrálnym bodom je ELK Stack (Elasticsearch, Logstash, Kibana) pre vizualizáciu dát.

### Aktívne Honeypoty v tejto konfigurácii
* **Cowrie:** Interaktívny SSH/Telnet honeypot (zachytáva príkazy útočníka).
* **Dionaea:** Zachytáva útoky na SMB, HTTP, FTP, MSSQL.
* **Conpot:** ICS/SCADA honeypot simulujúci priemyselné systémy (IEC104, IPMI, Kamstrup).
* **Honeytrap:** Monitorovanie TCP/UDP služieb.
* **Heralding:** Honeypot pre odchytávanie prihlasovacích údajov (FTP, POP3, IMAP).
* **Adbhoney:** Simulácia Android zariadení (Android Debug Bridge).
* **Suricata:** Network Security Monitoring (IDS/IPS).
* **Spiderfoot:** OSINT automatizácia.

## Custom konfigurácia (docker-compose.yml)

V tomto repozitári je priložený upravený súbor `docker-compose.yml`. Táto verzia rieši nasledujúce aspekty:
1. **Riešenie konfliktov portov:** Úprava mapovania portov, ktoré kolidovali s bežiacimi systémovými službami.
2. **Výber služieb:** Povolenie špecifických modulov (Conpot, Adbhoney) a zakázanie nadbytočných služieb pre optimalizáciu výkonu.

**Dôležité upozornenie:** Odporúča sa inštalovať tento honeypot výhradne na čistý operačný systém (Clean Install), aby sa predišlo konfliktom s existujúcimi službami (napr. webserver na porte 80 alebo SSH na porte 22).

## Požiadavky na systém

Pre spustenie tejto konfigurácie je vyžadovaný server/VM s nasledujúcimi parametrami:
* **OS:** Debian 11 (Bullseye) alebo Debian 12 (Bookworm) - vyžadovaná čistá inštalácia.
* **RAM:** Minimálne 8GB (odporúčané 16GB pre plynulý chod ELK stacku).
* **Disk:** 128GB SSD (pre ukladanie logov).
* **Sieť:** Verejná IP adresa pre reálny zber dát (povolené všetky prichádzajúce spojenia).

## Inštalácia

Proces inštalácie pozostáva z nasadenia základného T-Pot prostredia a následnej aplikácie custom konfigurácie z tohto repozitára.

### 1. Príprava systému a stiahnutie T-Pot
Na čistom systéme Debian najskôr aktualizujte balíčky a nainštalujte git:

```bash
sudo apt-get update && sudo apt-get install git -y
```
Následne naklonujte oficiálny repozitár T-Pot a spustite inštalátor. Tento krok je nevyhnutný pre prípravu závislostí, Docker prostredia a systémových nastavení.

```bash
git clone https://github.com/telekom-security/tpotce
cd tpotce/iso/installer/
sudo ./install.sh --type=user
```

Poznámka: Počas inštalácie zvoľte typ 'USER'. Po dokončení sa systém reštartuje.

### 2. Aplikácia custom konfigurácie
Po reštarte sa prihláste do systému. Pôvodný konfiguračný súbor vygenerovaný inštalátorom nahradíme súborom z tohto repozitára.

Stiahnite tento repozitár (alebo len súbor docker-compose.yml):

git clone https://github.com/Bajco1/BIT_Projekt custom-tpot

Nahraďte konfiguráciu:

```bash
# Záloha pôvodnej konfigurácie
sudo cp /opt/tpot/etc/tpot.yml /opt/tpot/etc/tpot.yml.old

# Nahradenie novou konfiguráciou
sudo cp custom-tpot/docker-compose.yml /opt/tpot/etc/tpot.yml
```

### 3. Spustenie
Spustite služby s novou konfiguráciou:

```bash
sudo systemctl start tpot
```

Stav kontajnerov môžete overiť príkazom:
```bash
docker ps
```
## Prístup k dátam
Po úspešnom štarte sú služby dostupné cez webový prehliadač (všetky vyžadujú VPN alebo SSH tunel, ak nie sú povolené priamo na firewalli):

``Admin UI (Cockpit): https://<IP_ADRESA>:64294``

``Kibana (Dashboardy): https://<IP_ADRESA>:64297``

``Spiderfoot: https://<IP_ADRESA>:64303``
