# FAU-Crawler: Kennzeichnung und Regeln für den schonenden Abruf von Websites

**Version:** 1.0\
**Abgeleitet aus:** FAU Crawler User-Agent Specification --- LLM/Agent
Project Rules, Version 1.5

## Kurzbeschreibung

Automatisierte Crawler, Bots und Scraper, die im Verantwortungsbereich
einer Einrichtung oder eines Projekts der FAU betrieben werden, sollen
sich gegenüber Webservern eindeutig zu erkennen geben und Websites
möglichst schonend und effizient abrufen.

Dafür wird ein einheitlicher HTTP-User-Agent verwendet:

``` text
FAU-<ORG>-<BOT>/<VERSION> (+<INFO-URL>;mailto:<CONTACT>)
```

Beispiel:

``` text
FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/;mailto:webmaster@fau.de)
```

Die wichtigsten Regeln sind:

-   Jeder Crawler soll über seinen User-Agent einer FAU-Einrichtung und
    einem konkreten Bot zugeordnet werden können.
-   Der User-Agent enthält eine Versionsnummer, eine Informationsseite
    und eine erreichbare Kontaktadresse.
-   `robots.txt` muss vor einem systematischen Crawl berücksichtigt
    werden. Dort angegebene Sitemaps sollen bevorzugt für die Ermittlung
    der vorhandenen URLs verwendet werden.
-   Für die Ermittlung und den Abruf von Inhalten gilt grundsätzlich die
    Reihenfolge: `robots.txt` → Sitemap → `llms.txt`/`llms-full.txt` →
    API bzw. strukturierte Daten → regulärer HTML-Crawl.
-   Verweist eine HTML-Seite über Metadaten oder `rel`-Links auf eine
    geeignete JSON-, REST- oder andere maschinenlesbare Repräsentation,
    soll diese nach Möglichkeit genutzt werden.
-   Ein Crawler darf pro Origin/Host höchstens **3 Requests pro
    Sekunde** starten. Das Limit gilt insgesamt und nicht jeweils
    getrennt für parallele Prozesse oder Worker.
-   Serverantworten wie `429 Too Many Requests`,
    `503 Service Unavailable` und `Retry-After` müssen berücksichtigt
    werden.
-   Crawler sollen grundsätzlich ohne dauerhaften Sitzungszustand und
    ohne Cookies arbeiten. Technisch notwendige Cookies sind zulässig;
    Tracking-, Werbe-, Analytics-, Personalisierungs- oder
    Consent-Cookies sollen nicht verwendet werden.
-   Der User-Agent ist nur eine Selbstauskunft und keine
    Authentisierung. Ein `FAU-`-User-Agent darf daher niemals allein
    besondere Zugriffsrechte erhalten.

Die folgenden Abschnitte erläutern diese Regeln genauer.

## 1. Geltungsbereich und Ziel

Die Vorgabe richtet sich an automatisierte Systeme, die Websites oder
Webressourcen im Verantwortungsbereich einer FAU-Einrichtung, eines
FAU-Projekts oder eines entsprechenden Dienstes abrufen.

Dazu zählen beispielsweise:

-   Suchmaschinen-Crawler,
-   Forschungs-Crawler,
-   Link- und Qualitätsprüfungen,
-   Accessibility- und Compliance-Prüfungen,
-   Crawler für KI- und RAG-Systeme,
-   Bots zur Erfassung von Metadaten,
-   Scraper für definierte wissenschaftliche oder administrative Zwecke.

Ziel ist nicht, eine bestimmte Crawler-Software vorzuschreiben. Die
Vorgabe definiert vielmehr, wie sich ein solcher Dienst gegenüber
fremden Webservern identifiziert und wie er sich beim Abruf verhalten
soll.

Websitebetreiber sollen anhand ihrer Server-Logs erkennen können,
welcher FAU-Dienst einen Abruf durchgeführt hat, wer dafür
verantwortlich ist und wie der Betreiber bei Problemen Kontakt aufnehmen
kann.

## 2. Aufbau des User-Agents

Das vorgesehene Schema lautet:

``` text
FAU-<ORG>-<BOT>/<VERSION> (+<INFO-URL>;mailto:<CONTACT>)
```

Ein vollständiges Beispiel ist:

``` text
FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/;mailto:webmaster@fau.de)
```

Die einzelnen Bestandteile haben unterschiedliche Aufgaben.

### 2.1 `FAU`

`FAU` ist das feste Präfix.

Es zeigt an, dass der Crawler im Verantwortungsbereich einer
Organisationseinheit oder eines Projekts der
Friedrich-Alexander-Universität Erlangen-Nürnberg betrieben wird.

Das Präfix darf nicht für private oder externe Systeme verwendet werden,
die nicht unter entsprechender Verantwortung der FAU stehen.

Wichtig ist dabei: Das Präfix beweist die Herkunft nicht. Jeder
beliebige Client kann technisch einen User-Agent senden, der mit `FAU-`
beginnt.

### 2.2 `<ORG>` -- verantwortliche Organisation

`ORG` bezeichnet die verantwortliche FAU-Einrichtung oder
Organisationseinheit.

Beispiele:

``` text
RRZE
UB
TF
INF12
```

Wenn bereits eine etablierte Kurzbezeichnung vorhanden ist, sollte diese
verwendet werden.

Die Bezeichnung sollte stabil bleiben und keine Leerzeichen enthalten.
Zulässig bzw. vorgesehen sind insbesondere Buchstaben, Ziffern,
Bindestrich und Unterstrich.

### 2.3 `<BOT>` -- Name des Crawlers

`BOT` benennt den eigentlichen Crawler.

Beispiele könnten sein:

``` text
Legalcheck
SearchBot
ResearchCrawler
Scraper
```

Auch allgemeine funktionale Namen wie `Bot`, `Crawler` oder `Scraper`
sind zulässig.

Nicht sinnvoll sind dagegen Bezeichnungen, die lediglich die verwendete
Technik nennen. Ein Bot sollte beispielsweise nicht einfach so heißen:

``` text
Python
PHP
Java
curl
wget
python-requests
Guzzle
```

Der User-Agent soll schließlich den Dienst identifizieren und nicht
lediglich verraten, mit welcher Programmiersprache oder Bibliothek er
implementiert wurde.

### 2.4 `<VERSION>` -- Versionsnummer

Jeder Crawler gibt eine Versionsnummer an.

Empfohlen ist mindestens das Schema:

``` text
MAJOR.MINOR
```

Beispielsweise:

``` text
1.0
1.4
2.0
2.3.1
```

Die Versionsnummer bezieht sich auf den Crawler und dessen Verhalten.
Sie ist nicht die Versionsnummer von PHP, Python, curl oder einer
verwendeten HTTP-Bibliothek.

Ändert sich das Crawl-Verhalten wesentlich, sollte auch die
Versionsnummer angepasst werden.

### 2.5 `<INFO-URL>` -- Informationsseite

Produktiv eingesetzte Crawler sollten eine dauerhaft erreichbare
HTTPS-Seite angeben.

Beispiel:

``` text
https://www.rrze.fau.de/bots/legalcheck/
```

Dort sollten insbesondere Informationen zu folgenden Punkten zu finden
sein:

-   Name und Zweck des Crawlers,
-   verantwortliche FAU-Einrichtung,
-   Kontaktmöglichkeit,
-   typisches Crawl-Verhalten,
-   gegebenenfalls verwendete Quellnetze oder IP-Adressen,
-   gegebenenfalls besondere Rate-Limits oder technische Hinweise.

Die Informationsseite ermöglicht es einem Websitebetreiber, einen
unbekannten Zugriff aus seinem Server-Log schnell einzuordnen.

### 2.6 `<CONTACT>` -- Kontaktadresse

Zusätzlich wird eine funktionierende Mailadresse angegeben:

``` text
mailto:webmaster@fau.de
```

Eine Funktionsadresse ist gegenüber einer persönlichen Adresse zu
bevorzugen. Dadurch bleibt die Kontaktmöglichkeit auch bei Urlaub,
Stellenwechsel oder organisatorischen Veränderungen bestehen.

## 3. Schreibweise von `;mailto:`

Die aktuelle kanonische Schreibweise lautet ohne Leerzeichen:

``` text
;mailto:
```

Damit ergibt sich beispielsweise:

``` text
FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/;mailto:webmaster@fau.de)
```

Neue Crawler sollen diese Form erzeugen.

Aus Kompatibilitätsgründen bleibt jedoch auch die ältere Schreibweise
mit einem Leerzeichen gültig:

``` text
FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/; mailto:webmaster@fau.de)
```

Ein Parser oder Validator darf einen ansonsten korrekten FAU-User-Agent
nicht allein wegen dieses Leerzeichens als ungültig behandeln.

Damit gilt:

-   `;mailto:` ist die bevorzugte und kanonische Form.
-   `; mailto:` ist eine gültige Kompatibilitätsform.

## 4. `robots.txt` zuerst berücksichtigen

Vor einem systematischen Crawl muss der Crawler die für den Server
geltende `robots.txt` berücksichtigen.

Typischerweise befindet sie sich unter:

``` text
/robots.txt
```

Darin können Websitebetreiber unter anderem festlegen, welche Bereiche
von bestimmten Crawlern abgerufen werden dürfen.

Beispiel:

``` text
User-agent: FAU-RRZE-Legalcheck
Disallow: /intern/
```

Die Zugehörigkeit eines Crawlers zur FAU stellt grundsätzlich keine
Ausnahme von diesen Regeln dar.

Ein Crawler sollte die `robots.txt` für eine angemessene Zeit
zwischenspeichern. Es wäre unnötig und kontraproduktiv, sie vor jedem
einzelnen Seitenabruf erneut herunterzuladen.

## 5. Sitemaps verwenden

Die `robots.txt` kann zusätzlich auf eine oder mehrere XML-Sitemaps
verweisen.

Ein Crawler sollte diese Angaben berücksichtigen und vorhandene Sitemaps
bevorzugt für die Ermittlung der URLs einer Website verwenden.

Das ist effizienter und serverfreundlicher als der Versuch, die gesamte
Website ausschließlich durch rekursives Verfolgen aller Links zu
erschließen.

Auch Sitemap-Indexdateien, die auf weitere Sitemaps verweisen, sollten
unterstützt werden.

Eine URL wird allerdings nicht dadurch automatisch zum erlaubten
Crawl-Ziel, dass sie in einer Sitemap steht. Die für den Crawler
geltenden Zugriffsbeschränkungen bleiben bestehen.

## 6. Bevorzugte Reihenfolge bei der Content-Discovery

Ein Crawler sollte vorhandene maschinenlesbare Informationen nutzen,
bevor er eine Website unnötig aufwendig über HTML analysiert.

Die bevorzugte Reihenfolge lautet:

1.  `robots.txt`
2.  dort deklarierte Sitemaps und Sitemap-Indizes
3.  `llms.txt` bzw. `llms-full.txt`
4.  geeignete APIs oder andere strukturierte maschinenlesbare
    Repräsentationen
5.  regulärer HTML-Crawl

Diese Reihenfolge ist eine Effizienzregel und keine Berechtigungsregel.

Zugriffsschutz, Authentisierung, `robots.txt` und strengere Vorgaben
eines Servers haben immer Vorrang.

## 7. `llms.txt` und `llms-full.txt`

Websites können Informationen speziell für KI-Systeme und LLM-basierte
Dienste über Dateien wie

``` text
/llms.txt
```

oder

``` text
/llms-full.txt
```

bereitstellen.

Ein geeigneter Crawler sollte solche Informationen berücksichtigen, wenn
sie vorhanden und für seinen Zweck relevant sind.

Dabei ist zu beachten, dass diese Dateien eine Konvention darstellen und
nicht auf jeder Website vorhanden sind. Ihr Fehlen ist daher kein
Fehler.

Ebenso dürfen Angaben in einer solchen Datei keine bestehenden
Zugriffsbeschränkungen außer Kraft setzen.

## 8. APIs und strukturierte Inhalte bevorzugen

Viele Content-Management-Systeme stellen Inhalte nicht nur als HTML,
sondern zusätzlich über APIs oder andere strukturierte Formate bereit.

Wenn eine solche Schnittstelle den benötigten Inhalt vollständig und
zuverlässig liefert, sollte ein Crawler sie gegenüber einem aufwendigen
HTML-Crawl bevorzugen.

Das kann beispielsweise JSON-Daten einer REST-API betreffen.

Dadurch können unter anderem Navigation, Layoutinformationen,
wiederkehrende Seitenelemente und anderer für die eigentliche
Inhaltsanalyse unnötiger HTML-Ballast vermieden werden.

Das reduziert:

-   übertragene Daten,
-   Serverlast,
-   Parsing-Aufwand,
-   bei KI-Anwendungen häufig auch die Zahl unnötiger Tokens.

## 9. Maschinenlesbare Alternativen aus HTML erkennen

Manchmal kennt ein Crawler die API einer Website zunächst nicht.

CMS und andere Webanwendungen können jedoch im HTML-Dokument selbst auf
alternative Repräsentationen verweisen. Dazu gehören beispielsweise
`<link>`-Elemente mit geeigneten `rel`-Angaben oder CMS-spezifische
Hinweise auf JSON- und REST-Schnittstellen.

WordPress kann beispielsweise im HTML Informationen bereitstellen, über
die ein Client REST-API- bzw. JSON-Ressourcen entdecken kann.

Wenn ein Crawler beim Abruf einer HTML-Seite eine solche
maschinenlesbare Alternative findet, sollte er prüfen, ob diese für
seine Aufgabe besser geeignet ist.

Liefert sie die benötigten Inhalte vollständig, sollte die strukturierte
Repräsentation bevorzugt und redundante weitere HTML-Verarbeitung
vermieden werden.

Der Crawler darf allerdings nicht automatisch davon ausgehen, dass jede
JSON- oder API-Repräsentation sämtliche Inhalte der HTML-Seite enthält.

## 10. Umgang mit `/.well-known/`

Der Pfad

``` text
/.well-known/
```

wird für verschiedene standardisierte und anwendungsspezifische
Informationen verwendet.

Ein Crawler darf dort gezielt Ressourcen verwenden, die er kennt und für
seinen Zweck unterstützt.

Er soll jedoch nicht versuchen, das Verzeichnis pauschal zu durchsuchen
oder beliebige bekannte Dateinamen durchzuprobieren.

Das bedeutet insbesondere:

-   kein rekursives Crawling von `/.well-known/`,
-   keine systematische Enumeration möglicher Ressourcen,
-   Abruf nur bekannter, unterstützter oder ausdrücklich referenzierter
    Ressourcen.

Die bloße Existenz von `/.well-known/` bedeutet nicht, dass dort für den
jeweiligen Crawler relevante Inhalte liegen.

## 11. Maximal drei Requests pro Sekunde

Ein FAU-Crawler darf standardmäßig höchstens

**3 Requests pro Sekunde und Origin/Host**

starten.

Das Limit gilt für den gesamten Crawler.

Es ist daher nicht zulässig, beispielsweise zehn parallele Worker zu
starten, von denen jeder drei Requests pro Sekunde sendet. Alle
parallelen Prozesse, Threads oder asynchronen Requests müssen sich
gemeinsam an dasselbe Limit halten.

Ein Crawler darf selbstverständlich langsamer arbeiten.

Eine höhere Rate darf nur verwendet werden, wenn dies für den
betreffenden Zielserver ausdrücklich konfiguriert und autorisiert wurde.

Strengere Vorgaben des Zielsystems haben Vorrang.

## 12. Verhalten bei Überlastung und Rate-Limits

Ein verantwortungsvoller Crawler muss darauf reagieren, wenn ein Server
signalisiert, dass zu viele Anfragen erfolgen oder der Dienst
vorübergehend nicht verfügbar ist.

Besonders relevant sind:

``` text
429 Too Many Requests
503 Service Unavailable
Retry-After
```

Gibt der Server einen `Retry-After`-Header zurück, muss dieser
berücksichtigt werden.

Bei HTTP 429 oder 503 darf der Crawler nicht einfach mit unveränderter
Geschwindigkeit weiterarbeiten oder aggressive Wiederholungsversuche
durchführen.

Stattdessen soll die Request-Rate reduziert und bei wiederholten Fehlern
ein exponentieller Backoff verwendet werden. Die Wartezeit zwischen
weiteren Versuchen wird dabei schrittweise erhöht.

## 13. Cookies und Sitzungen

Crawler sollen grundsätzlich **zustandslos -- stateless -- arbeiten**.

Das bedeutet insbesondere, dass Cookies nicht automatisch dauerhaft
gespeichert und bei späteren Requests wieder übertragen werden sollten.

Technisch notwendige Cookies bleiben zulässig. Das kann beispielsweise
erforderlich sein, wenn ein ausdrücklich für den Crawler vorgesehener
Dienst für seine technische Funktion einen Sitzungszustand benötigt.

Nicht verwendet werden sollen dagegen Cookies für:

-   Tracking,
-   Webanalyse,
-   Werbung,
-   Personalisierung,
-   Consent Management.

Ein Crawler soll insbesondere keine für menschliche Besucher
vorgesehenen Cookie-Consent-Dialoge bedienen und nicht automatisiert auf
sinngemäße Funktionen wie „Alle akzeptieren" reagieren.

Cookies dürfen außerdem nicht verwendet werden, um Zugriffsschutz,
Authentisierung, Paywalls, Crawler-Beschränkungen oder Anti-Bot- und
Sicherheitsmaßnahmen zu umgehen.

Sind Cookies technisch notwendig, sollen sie nur für die betreffende
Origin bzw. Sitzung und nur so lange wie tatsächlich erforderlich
gespeichert werden.

Cookies einer Origin dürfen nicht an eine nicht zugehörige Origin
weitergegeben werden.

## 14. Der User-Agent ist keine Authentisierung

Ein User-Agent wird vollständig vom Client selbst bestimmt.

Ein Angreifer oder externer Crawler kann daher problemlos behaupten:

``` text
FAU-RRZE-Legalcheck/1.0 (+https://www.rrze.fau.de/bots/legalcheck/;mailto:webmaster@fau.de)
```

Das bedeutet nicht, dass der Request tatsächlich vom RRZE oder überhaupt
von der FAU stammt.

Daher darf allein aufgrund eines `FAU-`-User-Agents insbesondere nicht:

-   ein geschützter Bereich freigegeben werden,
-   eine Authentisierung umgangen werden,
-   eine Sicherheitskontrolle deaktiviert werden,
-   ein privilegierter Zugriff erlaubt werden.

Wenn die Identität eines Crawlers technisch verifiziert werden muss, ist
dafür ein zusätzliches überprüfbares Verfahren erforderlich,
beispielsweise eine geeignete Authentisierung oder kontrollierte
Quell-IP-Adressen bzw. Netze.

## 15. Praktische Checkliste für einen neuen FAU-Crawler

Vor dem produktiven Einsatz sollte geprüft werden:

-   [ ] Der User-Agent folgt dem Schema
    `FAU-<ORG>-<BOT>/<VERSION> (+<INFO-URL>;mailto:<CONTACT>)`.
-   [ ] Die verantwortliche FAU-Einrichtung ist eindeutig angegeben.
-   [ ] Der Bot besitzt einen stabilen Namen.
-   [ ] Der Name bezeichnet nicht lediglich Programmiersprache,
    Bibliothek oder Abrufwerkzeug.
-   [ ] Eine Versionsnummer ist vorhanden.
-   [ ] Eine dauerhaft erreichbare Informationsseite ist angegeben.
-   [ ] Eine betreute Kontaktadresse ist angegeben.
-   [ ] Neue Implementierungen erzeugen die kanonische Schreibweise
    `;mailto:`.
-   [ ] Bestehende User-Agents mit `; mailto:` werden weiterhin
    akzeptiert.
-   [ ] `robots.txt` wird vor einem systematischen Crawl berücksichtigt.
-   [ ] Dort angegebene Sitemaps und Sitemap-Indizes werden ausgewertet.
-   [ ] `llms.txt` und andere geeignete maschinenlesbare Hinweise werden
    berücksichtigt.
-   [ ] Vor unnötigem HTML-Crawling werden geeignete APIs und
    strukturierte Repräsentationen bevorzugt.
-   [ ] HTML-Metadaten werden auf verlinkte maschinenlesbare
    Alternativen geprüft.
-   [ ] `/.well-known/` wird nicht pauschal enumeriert.
-   [ ] Es werden insgesamt höchstens 3 Requests pro Sekunde und
    Origin/Host gestartet.
-   [ ] Parallele Worker verwenden ein gemeinsames Rate-Limit.
-   [ ] `Retry-After`, HTTP 429 und HTTP 503 werden berücksichtigt.
-   [ ] Der Crawler arbeitet standardmäßig ohne persistenten
    Cookie-Zustand.
-   [ ] Cookies werden nicht zum Umgehen von Zugriffsschutz oder
    Sicherheitsmaßnahmen eingesetzt.
-   [ ] Der User-Agent wird niemals als alleiniger Authentisierungs-
    oder Vertrauensnachweis verwendet.
