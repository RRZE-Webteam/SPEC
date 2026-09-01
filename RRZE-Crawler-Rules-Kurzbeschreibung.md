# Vorgaben an FAU-Crawler

( Version: 1.0,  Date: 01.09.2026,  Source: https://github.com/RRZE-Webteam/SPEC/RRZE-Crawler-Rules-Kurzbeschreibung.md )

Automatisierte Crawler, Bots und Scraper, die im Verantwortungsbereich einer FAU-Einrichtung, eines FAU-Projekts oder eines entsprechenden Dienstes betrieben werden, müssen sich eindeutig zu erkennen geben und Websites schonend abrufen. Diese Kurzbeschreibung fasst die wichtigsten Regeln zusammen.

Die vollständigen und jeweils maßgeblichen Vorgaben stehen im GitHub-Repository:

- [RRZE-Crawler-Rules.md](https://github.com/RRZE-Webteam/SPEC/RRZE-Crawler-Rules.md)
- [RRZE-Crawler-Rules-LLM-Shortcut.md](https://github.com/RRZE-Webteam/SPEC/RRZE-Crawler-Rules-LLM-Shortcut.md)

## Anforderungen an FAU-Crawler

1. Jeder Crawler muss einen eindeutigen HTTP-User-Agent verwenden, der die verantwortliche FAU-Einrichtung, den Bot, die Version, eine Informationsseite und eine betreute Kontaktadresse nennt.
2. Das vorgesehene Schema lautet:

    ```text
    FAU-<ORG>-<BOT>/<VERSION> (+<INFO-URL>;mailto:<CONTACT>)
    ```

3. Neue Implementierungen sollen die kanonische Schreibweise `;mailto:` ohne Leerzeichen verwenden. Parser und Validatoren sollen die ältere Schreibweise `; mailto:` weiterhin akzeptieren.
4. Der Name des Bots muss den Dienst erkennbar machen. Reine Techniknamen wie `Python`, `curl`, `wget`, `Guzzle` oder `python-requests` sind als Botname nicht geeignet.
5. Der User-Agent muss bei allen crawler-originated HTTP-Requests gesetzt werden, also auch bei HTML-Seiten, WordPress-REST-API-Anfragen, XML-Sitemaps, Feeds, Dateien, Medien und Requests nach Redirects.
6. Ein Crawler darf sich nicht als normaler Browser tarnen, etwa durch einen generischen `Mozilla/5.0`-User-Agent.
7. Vor einem systematischen Crawl muss die jeweilige `robots.txt` abgerufen und beachtet werden. FAU-Zugehörigkeit ist keine Ausnahme von `robots.txt`.
8. In `robots.txt` deklarierte Sitemaps sollen bevorzugt für die URL-Ermittlung verwendet werden. Sitemap-Indexdateien sollen unterstützt werden, wenn Sitemap-Discovery implementiert ist.
9. Für Discovery und Abruf gilt grundsätzlich diese Reihenfolge: `robots.txt`, dort deklarierte Sitemaps, `llms.txt` bzw. `llms-full.txt`, geeignete APIs oder strukturierte Repräsentationen, danach regulärer HTML-Crawl.
10. Maschinenlesbare Alternativen, die im HTML über Metadaten oder `rel`-Links beworben werden, sollen genutzt werden, wenn sie die benötigten Inhalte vollständiger oder effizienter liefern als HTML.
11. Ressourcen unter `/.well-known/` dürfen nur gezielt abgerufen werden, wenn der Crawler die jeweilige Spezifikation unterstützt oder die Ressource ausdrücklich referenziert wurde. Eine pauschale Enumeration ist nicht zulässig.
12. Pro Origin oder Host dürfen standardmäßig höchstens drei Requests pro Sekunde gestartet werden. Das Limit gilt für den gesamten Crawler, also auch über parallele Worker, Prozesse oder Threads hinweg.
13. Strengere Zielsystem-Vorgaben, `Retry-After`, HTTP 429 und HTTP 503 müssen berücksichtigt werden. Bei Überlastung oder Rate-Limits muss der Crawler die Abrufrate reduzieren oder Backoff verwenden.
14. Crawler sollen grundsätzlich zustandslos arbeiten. Cookies und Sessions dürfen nur verwendet werden, wenn sie für den ausdrücklich definierten Zweck technisch erforderlich sind.
15. Tracking-, Analytics-, Werbe-, Personalisierungs- und Consent-Cookies sollen nicht gespeichert oder zurückgesendet werden. Cookie-Consent-Dialoge für Menschen dürfen nicht automatisiert genutzt werden, um optionale Verarbeitung zu aktivieren.
16. Cookies, Sessions oder Browserzustand dürfen nicht genutzt werden, um Authentisierung, Zugriffsschutz, Paywalls, Crawler-Beschränkungen oder Anti-Bot-Maßnahmen zu umgehen.
17. Der User-Agent ist keine Authentisierung. Ein `FAU-`-User-Agent darf niemals allein als Vertrauensnachweis dienen oder besondere Zugriffsrechte auslösen.
18. Wenn die Identität eines Crawlers technisch verifiziert werden muss, ist ein zusätzliches überprüfbares Verfahren erforderlich, zum Beispiel Authentisierung oder kontrollierte Quell-IP-Adressen bzw. Netze.

## Beispiel

```text
FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/;mailto:webmaster@fau.de)
```

## Kontakt

Für Rückfragen:

- RRZE-Webteam
- E-Mail: [webmaster@fau.de](mailto:webmaster@fau.de)
- Website: [www.rrze.fau.de](https://www.rrze.fau.de), [www.wp.rrze.fau.de](https://www.wp.rrze.fau.de)
- Matrix-Channel: `#web:fau.de`
