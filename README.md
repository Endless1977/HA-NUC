# Home Assistant + Claude op een Intel NUC

Na deze handleiding draait Home Assistant OS op de NUC en kun je in de zijbalk Claude Code openen om je smart home mee in te richten. Reken op ongeveer een uur. Alles wat op de NUC staat wordt gewist.

## Wat je nodig hebt

- [ ] Intel NUC met interne SSD (minimaal 32 GB) en minimaal 4 GB RAM
- [ ] Netwerkkabel naar de router (geen wifi tijdens de installatie)
- [ ] Monitor en toetsenbord, alleen nodig tijdens de installatie
- [ ] USB-stick van minimaal 8 GB
- [ ] Een andere pc om de USB-stick te maken
- [ ] Een betaald Claude-abonnement (Pro of hoger) of een API-key: [abonnementen bekijken](https://claude.com/pricing)

| Download | Waarvoor | Link |
| --- | --- | --- |
| Ubuntu Desktop | Tijdelijk opstarten van de NUC | [ubuntu.com/download/desktop](https://ubuntu.com/download/desktop) |
| Rufus (Windows) | Ubuntu op de USB-stick zetten | [rufus.ie](https://rufus.ie) |
| balenaEtcher (Mac/Linux) | Alternatief voor Rufus | [etcher.balena.io](https://etcher.balena.io) |
| Home Assistant OS | Het besturingssysteem zelf | [nieuwste release](https://github.com/home-assistant/operating-system/releases/latest) |

## Stap 1: BIOS van de NUC instellen

Home Assistant start alleen in UEFI-modus op, dus dit moet eerst goed staan.

1. Zet de NUC aan en druk direct een paar keer op **F2**.
2. Zet **Boot mode** op **UEFI** (niet Legacy).
3. Zet **Secure Boot** uit.
4. Zet **After Power Failure** op **Power On**, zodat de NUC na een stroomstoring vanzelf weer opstart.
5. Sla op met **F10** en zet de NUC uit.

## Stap 2: Ubuntu-USB-stick maken

Deze stick gebruik je eenmalig om Home Assistant op de SSD van de NUC te zetten. Doe dit op je andere pc.

1. Download het Ubuntu Desktop-bestand (`.iso`) via [ubuntu.com/download/desktop](https://ubuntu.com/download/desktop).
2. Download en open [Rufus](https://rufus.ie) (op een Mac: [balenaEtcher](https://etcher.balena.io)).
3. Kies de USB-stick, kies het Ubuntu-bestand en klik op **Start**. Laat Rufus de standaardinstellingen gebruiken (partitieschema **GPT**, doelsysteem **UEFI**).
4. Haal de stick eruit als Rufus klaar is.

## Stap 3: Home Assistant OS op de NUC zetten

Je start de NUC op vanaf de stick en schrijft Home Assistant OS vanuit Ubuntu naar de interne SSD. Dit is de methode die Home Assistant zelf aanraadt ([officiële handleiding](https://www.home-assistant.io/installation/generic-x86-64/)).

1. Stop de USB-stick en de netwerkkabel in de NUC en zet hem aan. Druk op **F10** voor het opstartmenu en kies de USB-stick.
2. Kies **Try Ubuntu**. Ubuntu installeren is niet nodig.
3. Open Firefox in Ubuntu en ga naar de [nieuwste Home Assistant OS-release](https://github.com/home-assistant/operating-system/releases/latest).
4. Download onder **Assets** het bestand dat begint met `haos_generic-x86-64-` en eindigt op `.img.xz`.
5. Open de app **Schijven** (Disks) via het menu linksonder.
6. Kies links de **interne SSD** van de NUC, niet de USB-stick. Let op de grootte om ze uit elkaar te houden.
7. Klik rechtsboven op **⋮ → Schijfkopie terugzetten** (Restore Disk Image), kies het gedownloade bestand en klik op **Starten met terugzetten**.
8. Als het klaar is: sluit Ubuntu af, haal de USB-stick eruit en zet de NUC weer aan.

## Stap 4: Eerste keer opstarten en account aanmaken

Na ongeveer 5 minuten is Home Assistant bereikbaar vanaf elke pc of telefoon in hetzelfde netwerk. Monitor en toetsenbord mogen dan weg.

1. Open in de browser: <http://homeassistant.local:8123>. Zie je "Preparing Home Assistant", wacht dan nog even.
2. Kies **Mijn smart home aanmaken**, vul naam, gebruikersnaam en wachtwoord in. Bewaar dat wachtwoord goed.
3. Stel locatie, tijdzone (Europe/Amsterdam), eenheden (metrisch) en valuta (EUR) in.
4. Home Assistant toont nu gevonden apparaten. Die kun je meteen toevoegen of later doen.
5. Geef de NUC in de router een **vast IP-adres** (vaak "DHCP-reservering" genoemd), zodat het adres nooit verandert.
6. Installeer eventueel de **Home Assistant-app** op je telefoon (App Store / Play Store).

De snelkoppelingen in de rest van deze handleiding lopen via my.home-assistant.io. De eerste keer vraagt die site om het adres van je Home Assistant: vul `http://homeassistant.local:8123` in (of `http://<vast-IP>:8123`). Daarna openen alle links direct de juiste pagina.

## Stap 5: Back-ups aanzetten

Zet automatische back-ups aan voordat Claude iets mag aanpassen. Gaat er iets mis, dan zet je met één klik de vorige toestand terug.

1. Open [Back-up instellingen](https://my.home-assistant.io/redirect/backup_config/).
2. Kies **Automatische back-ups**, dagelijks, en bewaar er minimaal 7.
3. Sla de back-upsleutel op die Home Assistant toont (bijvoorbeeld in een wachtwoordmanager). Zonder sleutel kun je een back-up niet terugzetten.
4. Maak via [Back-ups](https://my.home-assistant.io/redirect/backup/) direct handmatig je eerste back-up: **Back-up nu maken**.

## Stap 6: Claude Terminal installeren

Claude Terminal is een community-app die Claude Code als terminal in de zijbalk van Home Assistant zet, met direct toegang tot je configuratie ([projectpagina](https://github.com/heytcass/home-assistant-addons)). Het is geen officiële app van Home Assistant of Anthropic. Add-ons heten in Home Assistant tegenwoordig "Apps".

1. Klik op [Claude Terminal-repository toevoegen](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fheytcass%2Fhome-assistant-addons) en bevestig met **Toevoegen**.
2. Open de [App-winkel](https://my.home-assistant.io/redirect/supervisor_store/), ververs de pagina en zoek **Claude Terminal**.
3. Klik op **Installeren**. Dit duurt een paar minuten.
4. Zet **Weergeven in zijbalk** aan en klik op **Starten**.
5. Klik in de zijbalk op **Claude Terminal**. Claude Code start vanzelf.
6. Volg de inlogstappen: kies inloggen met je Claude-account, open de getoonde link, log in en plak de code terug in de terminal.

Gebruik je Home Assistant vooral op je telefoon? Er is ook een variant met een chatscherm in plaats van een terminal: [Claude Code UI-repository toevoegen](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fxionic%2Fhome-assistant-claude-code).

## Stap 7: Eerste opdrachten voor Claude

Typ gewoon in het Nederlands wat je wilt. Claude vraagt toestemming voordat het bestanden aanpast; lees die vraag even door voordat je ja zegt.

- *Bekijk mijn Home Assistant-configuratie en leg uit welke apparaten en integraties ik heb.*
- *Welke apparaten in mijn netwerk kan ik nog aan Home Assistant koppelen?*
- *Maak een automatisering die de lampen in de woonkamer aanzet bij zonsondergang.*
- *Maak een overzichtelijk dashboard met per kamer de lampen en temperatuur.*
- *Controleer mijn configuratie op fouten.*

Na een wijziging aan automatiseringen of instellingen vraagt Claude soms om Home Assistant te herladen. Dat kan via [Hulpmiddelen → YAML](https://my.home-assistant.io/redirect/tools_yaml/).

## Problemen oplossen

| Probleem | Oplossing |
| --- | --- |
| NUC start niet op na stap 3 | Controleer in het BIOS (F2) of UEFI aan en Secure Boot uit staat. |
| "Schijven" toont geen interne SSD | Controleer in het BIOS of de SSD gezien wordt. Alternatief: SSD eruit, via USB-adapter op je pc zetten en het `.img.xz`-bestand er met balenaEtcher op schrijven. |
| homeassistant.local werkt niet | Kijk in de router welk IP-adres de NUC kreeg en gebruik `http://<IP>:8123`, of lees het adres af op de monitor van de NUC. |
| App-winkel toont Claude Terminal niet | Ververs de pagina of kies **⋮ → Controleren op updates** in de App-winkel. |
| Claude doet iets wat je niet wilde | Zet de laatste back-up terug via [Back-ups](https://my.home-assistant.io/redirect/backup/). |

## Snelkoppelingen

Deze links openen direct de juiste pagina in je eigen Home Assistant.

| Pagina | Link |
| --- | --- |
| Home Assistant openen | [homeassistant.local:8123](http://homeassistant.local:8123) |
| Instellingen | [openen](https://my.home-assistant.io/redirect/config/) |
| Integraties (apparaten koppelen) | [openen](https://my.home-assistant.io/redirect/integrations/) |
| Apparaten | [openen](https://my.home-assistant.io/redirect/devices/) |
| Automatiseringen | [openen](https://my.home-assistant.io/redirect/automations/) |
| App-winkel | [openen](https://my.home-assistant.io/redirect/supervisor_store/) |
| Geïnstalleerde apps | [openen](https://my.home-assistant.io/redirect/supervisor_apps/) |
| Back-ups | [openen](https://my.home-assistant.io/redirect/backup/) |
| Updates | [openen](https://my.home-assistant.io/redirect/updates/) |
| Netwerk | [openen](https://my.home-assistant.io/redirect/network/) |
| Logboeken | [openen](https://my.home-assistant.io/redirect/logs/) |
| Systeeminformatie | [openen](https://my.home-assistant.io/redirect/system_health/) |

## Bronnen

- [Home Assistant: installatie op Generic x86-64](https://www.home-assistant.io/installation/generic-x86-64/)
- [Claude Terminal (heytcass/home-assistant-addons)](https://github.com/heytcass/home-assistant-addons)
- [Claude Code UI (xionic/home-assistant-claude-code)](https://github.com/xionic/home-assistant-claude-code)
- [Lijst met My Home Assistant-snelkoppelingen](https://github.com/home-assistant/my.home-assistant.io/blob/main/redirect.json)

*Laatst bijgewerkt: oktober 2026*
