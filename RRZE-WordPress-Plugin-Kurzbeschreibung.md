# Vorgaben an WordPress-Plugins

( Version: 1.0,  Date: 01.09.2026,  Source: https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Plugin-Kurzbeschreibung.md )

Bei der Nutzung, Entwicklung oder Erweiterung von WordPress-Plugins für das CMS-Angebot des RRZE gelten verbindliche technische, organisatorische und redaktionelle Rahmenbedingungen. Diese Kurzbeschreibung fasst die wichtigsten Punkte zusammen.

Die vollständigen und jeweils maßgeblichen Vorgaben stehen im GitHub-Repository:

- [RRZE-WordPress-Plugin.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Plugin.md)
- [RRZE-WordPress-Plugin-LLM-Shortcut.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Plugin-LLM-Shortcut.md)
- [RRZE-WordPress-Plugin-Testprompts.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Plugin-Testprompts.md)
- [RRZE-WordPress-Entwicklungsumgebung.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Entwicklungsumgebung.md)
- [RRZE-WordPress-Entwicklungsumgebung-LLM-Shortcut.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Entwicklungsumgebung-LLM-Shortcut.md)

## Anforderungen an neue Plugins auf dem CMS-Angebot des RRZE

1. Für das jeweilige Plugin muss stets ein fachkompetenter Ansprechpartner vorhanden sein, der im Falle von Problemen oder Fehlern zeitnah reagiert.
2. Das Plugin muss als Mindestanforderung kompatibel zur jeweils aktuellen WordPress- und PHP-Version der CMS-Instanz des RRZE sein.
3. Das Plugin muss für WordPress Multisite geeignet sein. Netzwerkweite und site-spezifische Einstellungen, Rechte, Daten, Aktivierung, Updates, Migrationen, Uploads, Caches und Cron-Aufgaben müssen sauber getrennt werden.
4. Für produktive Plugins muss eine gepflegte, webbasierte Benutzerdokumentation vorhanden sein. Sie muss Zweck, normale Arbeitsabläufe, relevante Einstellungen, Berechtigungen, Grenzen und typische Probleme auch für nicht-technische Nutzende verständlich erklären.
5. Neue Funktionen zur Einbindung oder Verwaltung von Inhalten in Beiträgen, Seiten oder Custom Post Types müssen den Block Editor unterstützen. Neue Shortcodes für redaktionelle Inhaltsfunktionen sind nicht zulässig. Bestehende Shortcodes dürfen nur aus Gründen der Abwärtskompatibilität weitergeführt werden.
6. Ausgaben im Frontend, Backend und Block Editor müssen barrierefrei nach WCAG 2.2 AA umgesetzt werden. Bei Formularen und anderen Eingabeworkflows ist WCAG 2.2 AAA ein gewünschtes Qualitätsziel, aber keine zwingende Mindestanforderung.
7. Administrationsoberflächen müssen WordPress-typische Bedienmuster nutzen und für Redakteurinnen, Redakteure und Administratoren ohne technisches Spezialwissen verständlich sein. Normale und erweiterte Einstellungen sind klar zu trennen.
8. Rechte müssen serverseitig über WordPress-Capabilities geprüft werden. Das bloße Ausblenden von Bedienelementen in CSS oder JavaScript ist keine Zugriffskontrolle.
9. API-Schlüssel, Lizenzschlüssel, Tokens und vergleichbare Secrets, die netzwerkweit gelten, dürfen nur durch berechtigte Network Admins oder Super Admins verwaltet werden. Site Admins dürfen solche Secrets weder sehen noch ändern oder überschreiben.
10. Externe Dienste, Drittanbieterressourcen, Cookies, Browser-Speicher und personenbezogene Daten dürfen nur mit dokumentiertem Zweck, geklärten Datenschutzfolgen und, wo erforderlich, passendem Consent-Verfahren verwendet werden.
11. Wenn ein Plugin automatisiert externe Websites, Feeds, APIs, Dateien oder andere Webressourcen abruft, muss es die FAU-Crawler-Regeln einhalten und einen passenden User-Agent gemäß [RRZE-Crawler-Rules.md](https://github.com/RRZE-Webteam/SPEC/RRZE-Crawler-Rules.md) setzen.
12. JavaScript und CSS dürfen nur dort geladen werden, wo sie tatsächlich benötigt werden. Produktionsassets müssen minifiziert sein; Source Maps gehören nicht in produktive Auslieferungen.
13. Externe JavaScript-, CSS-, Font- oder Bibliotheksressourcen von öffentlichen CDNs sind standardmäßig nicht zulässig. Benötigte Laufzeitressourcen sind lokal mit dem Plugin auszuliefern oder aus WordPress Core zu verwenden.
14. Das Plugin muss in Git gepflegt werden. GitHub oder GitLab ist das kanonische Repository. Der Branch `main` muss einen direkt lauffähigen Produktionsstand enthalten; ein Build auf dem Produktionsserver oder eine manuelle ZIP-Verteilung ist nicht Teil des Standardprozesses.
15. Version, WordPress-Kompatibilität, PHP-Kompatibilität, Repository- und Autorinformationen müssen zentral in `package.json` gepflegt und in Plugin-Header, `readme.txt` und weitere Zielstellen synchronisiert werden.
16. Vor einer Freigabe müssen Build, Linting, Tests, PHP-Prüfungen, WordPress Plugin Check, Multisite-Verhalten, Rollen- und Rechteprüfung, Barrierefreiheit, Browserkonsole, Übersetzungen und Dokumentation geprüft oder ausdrücklich als nicht ausführbar dokumentiert werden.

## Betriebsbedingungen auf der CMS-Instanz der FAU

Bei einem Einsatz auf der zentralen CMS-Instanz der FAU gelten außerdem diese Bedingungen:

- WordPress- und PHP-Updates haben Vorrang vor der Funktionsfähigkeit einzelner Plugins. Entwicklerinnen und Entwickler müssen ihre Plugins rechtzeitig auf kommende Versionen vorbereiten.
- Plugins, die nach einem Update Fehler verursachen, Sicherheitsrisiken erzeugen oder nicht mehr gepflegt werden, können deaktiviert oder aus dem unterstützten Portfolio entfernt werden.
- Plugins ohne erreichbaren fachkompetenten Ansprechpartner oder ohne dauerhafte Wartungsverantwortung sind für den produktiven Betrieb nicht geeignet.
- Die Präfixe `rrze-`, `fau-`, `utn-` und `cms-` dürfen nur nach Abstimmung mit dem RRZE verwendet werden.
- Kommerzielle Plugins mit kostenpflichtigen Lizenzen können nur über das RRZE beschafft und installiert werden. Für Betrieb, Wartung und Pflege können zusätzliche Kosten entstehen.

## Kontakt

Für Rückfragen:

- RRZE-Webteam
- E-Mail: [webmaster@fau.de](mailto:webmaster@fau.de)
- Website: [www.rrze.fau.de](https://www.rrze.fau.de), [www.wp.rrze.fau.de](https://www.wp.rrze.fau.de)
- Matrix-Channel: `#web:fau.de`
