# WordPress-Entwicklungsumgebung für das CMS-Angebot des RRZE

( Version: 1.0,  Date: 25.08.2026,  Source: https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Entwicklungsumgebung.md )

## Zweck

Diese Anleitung beschreibt eine lokale oder intern abgeschottete WordPress-Entwicklungsumgebung für Plugins und Themes, die im CMS-Angebot des RRZE eingesetzt werden sollen. Sie ergänzt den RRZE WordPress Plugin Engineering Standard und die Empfehlung zur [eigenen Testinstanz](https://www.wp.rrze.fau.de/entwicklung/eigene-testinstanz/).

Die Umgebung dient Entwicklung, Integration, Sicherheits- und Barrierefreiheitstests. Sie ist keine Produktionsumgebung und darf keine echten Zugangsdaten, personenbezogenen Produktivdaten oder produktiven API-Schlüssel enthalten.

## Grundsatz: Immer Multisite

Die Referenzumgebung arbeitet als **domainbasierte WordPress-Multisite**. Damit lassen sich Netzwerk- und Site-Einstellungen, Rollen, zentrale API-Schlüssel, Aktivierungsarten, Upload-Pfade und große Netzwerke realitätsnah prüfen.

Eine reine Single-Site-Installation genügt nicht. Single Site kann zusätzlich getestet werden, ersetzt die Multisite-Prüfung aber nicht.

Mindestens zwei Sites gehören in jedes Netz:

| Site | Zweck |
| --- | --- |
| Netzwerk-Hauptsite | Deutsche Standardsite für Inhalte, Administration und Übersetzungen. |
| Englische Subdomain-Site | Prüfung englischer Inhalte, Übersetzungen und site-spezifischer Einstellungen. |

Weitere Test-Sites sind sinnvoll für Migrationen, Plugin-spezifische Fälle und Theme-Varianten. Für Tests von Fakultätsthemes können zusätzliche Sites je Theme eingerichtet werden.

Für lokale Domains wird ein neutraler, menschenbetreuter Name verwendet, zum Beispiel `my-testsite.test` für die deutsche Hauptsite und `en.my-testsite.test` für die englische Site. Der Testnetzname darf nicht den Präfix `rrze` enthalten und keine echte öffentliche Domain verwenden.

## Technische Grundlage

Die Umgebung benötigt einen Webserver, PHP, MySQL oder MariaDB und WordPress. Git ist Pflicht, damit Themes und Plugins aus ihren Repositories geprüft und aktualisiert werden können.

XAMPP, LAMPP, Local, eine virtuelle Maschine oder eine containerbasierte Umgebung können verwendet werden. Entscheidend ist, dass die Umgebung reproduzierbar ist, lokale Domains für die Multisite auflösen kann und nicht öffentlich erreichbar ist.

Node.js und npm werden benötigt, wenn das getestete Projekt Node-Abhängigkeiten oder ein Asset-Buildsystem besitzt. Composer wird ergänzt, wenn das Plugin PHP-Abhängigkeiten nutzt. WP-CLI ist für wiederholbare Installation, Multisite-Verwaltung und automatisierte Tests sehr empfehlenswert.

## WordPress-Konfiguration

WordPress wird als domainbasierte Multisite eingerichtet. Network Admin muss erreichbar sein und sowohl Site- als auch Netzwerkaktivierung müssen testbar sein.

Für die Entwicklung sind Fehler sichtbar zu machen, ohne sie auf normalen Frontend-Seiten auszugeben:

```php
define('WP_DEBUG', true);
define('WP_DEBUG_LOG', true);
define('WP_DEBUG_DISPLAY', false);
define('SCRIPT_DEBUG', true);
```

Warnungen, Deprecated-Meldungen, Datenbankfehler und Fehler in der Browserkonsole sind keine Nebensache. Sie müssen vor einer Freigabe bewertet und behoben werden.

## Themes

| Theme | Einsatz |
| --- | --- |
| [FAU Elemental](https://github.com/RRZE-Webteam/FAU-Elemental) | Aktuelles Referenztheme für Tests von Frontend, Block Editor und Designintegration. Auf mindestens der deutschen und englischen Site aktivieren. |
| [FAU Einrichtungen](https://github.com/RRZE-Webteam/FAU-Einrichtungen) | Nur für Kompatibilitäts- und Regressionstests bestehender Installationen, sofern das Projekt diese unterstützt. |
| Aktuelles WordPress-Twenty-Theme | Immer als neutrales Vergleichstheme installieren. Verwendet wird das neueste Twenty-Theme, das mit der jeweils getesteten WordPress-Version ausgeliefert wird. |
| Vorheriges WordPress-Twenty-Theme | Zusätzlich für Rückwärtskompatibilitäts- und Vergleichstests installieren. |

FAU Elemental wird auf mindestens der deutschen und englischen Site aktiviert. Das aktuelle Twenty-Theme wird auf einer zusätzlichen Test-Site oder vorübergehend auf einer Test-Site aktiviert, um die Theme-Unabhängigkeit zu prüfen. Ein Plugin darf sich nicht stillschweigend auf die HTML-Struktur eines bestimmten Themes verlassen. Eine ausdrückliche Theme-Abhängigkeit muss dokumentiert und testbar sein.

## Referenz-Plugins

Diese Plugins bilden je nach Projektumfang die relevante RRZE-Referenzumgebung:

| Plugin | Aufgabe | Einsatz in der Testumgebung |
| --- | --- | --- |
| [RRZE Settings](https://github.com/RRZE-Webteam/rrze-settings/) | Netzwerkoptionen, API-Schlüssel, Editor- und Blocksteuerung. | Netzwerkweit aktivieren. |
| [RRZE Updater](https://github.com/RRZE-Webteam/rrze-updater) | Installation und Updates aus Git-Repositories. | Für Updater- und Deploymenttests einsetzen. |
| [RRZE Elements Blocks](https://github.com/RRZE-Webteam/rrze-elements-blocks) | Referenz für Blöcke und Gestaltungsintegration. | Bei Block- und Editor-Tests aktivieren. |
| [RRZE Legal](https://github.com/RRZE-Webteam/rrze-legal) | Consent- und Datenschutzverhalten. | Bei Cookies, externen Medien oder Drittanbietern aktivieren. |
| [RRZE Video](https://github.com/RRZE-Webteam/rrze-video) | Videoeinbindung und externe Medien. | Bei entsprechender Funktionalität aktivieren. |
| [FAU oEmbed](https://github.com/RRZE-Webteam/fau-oembed) | Einbettung über oEmbed. | Bei entsprechenden Integrationen aktivieren. |
| [RRZE Multisite Manager](https://github.com/RRZE-Webteam/rrze-multisite-manager) | Referenz für Multisite-Verwaltung und Site-Management. | Bei Multisite-Verwaltungsfunktionen netzwerkweit aktivieren. |
| [RRZE Log](https://github.com/RRZE-Webteam/rrze-log/) | Zentrale Protokollierung über `rrze.log.*`-Actions. | Für Plugin-Entwicklung und Log-Tests netzwerkweit aktivieren. |
| [Plugin Check](https://wordpress.org/plugins/plugin-check/) | Prüfung von Plugin-Anforderungen. | Für jeden Produktionskandidaten ausführen; kein Produktiv-Feature. |
| [Wordfence](https://wordpress.org/plugins/wordfence/) | Prüfung der Kompatibilität mit einem Sicherheitsplugin und seiner Administration. | Die kostenfreie Version genügt; bei sicherheitsrelevanten Funktionen aktivieren. |
| [Loco Translate](https://wordpress.org/plugins/loco-translate/) | Unterstützung bei Übersetzungsdateien. | Bei Übersetzungsarbeiten optional einsetzen. |

Shariff Wrapper, Statify und Redirection sind hilfreiche Fremdplugins aus der RRZE-Empfehlung. Sie werden nur installiert, wenn das Projekt ihre Wechselwirkungen testen muss.

## Build und Versionsdaten

Für Blöcke, Block-Editor-Erweiterungen, JSX/TSX und WordPress-spezifische JavaScript-Abhängigkeiten wird `@wordpress/scripts` eingesetzt. Es erzeugt die WordPress-konformen Block-Assets und behandelt die WordPress-Abhängigkeiten.

Ein schlankes Node-Skript mit `esbuild` und `sass` darf normale CSS- und JavaScript-Assets erzeugen sowie Version, Kompatibilitätsdaten, Plugin-Header, `readme.txt` und `README.md` synchronisieren. Es ersetzt `wp-scripts` nicht bei Block- oder WordPress-spezifischen Build-Aufgaben.

Ein Projekt ohne Blöcke und ohne WordPress-spezifischen JavaScript-Build darf auf `wp-scripts` verzichten. Sobald ein Projekt Blöcke oder Block-Editor-Code enthält, werden beide Aufgaben bei Bedarf kombiniert: Node-Skripte für Assets und Metadaten, `wp-scripts` für WordPress- und Block-Code.

## Rollen und Testdaten

Für aussagekräftige Tests werden getrennte Testkonten benötigt:

| Rolle | Prüft |
| --- | --- |
| Super Admin / Network Admin | Netzwerkoptionen, zentrale Dienste, Lizenz- und API-Schlüssel. |
| Site Admin | Wesentliche Einstellungen der einzelnen Website. |
| Editor | Redaktionelle Abläufe und Blockbedienung. |
| Autor oder Contributor | Eingeschränkte redaktionelle Funktionen, sofern das Plugin sie anbietet. |

Die Testdaten enthalten deutsche und englische Beiträge, Seiten, Medien und bei Bedarf Custom Post Types. Formulare werden mit gültigen, ungültigen und unvollständigen Eingaben getestet. Externe Dienste erhalten nach Möglichkeit Testdaten oder Mock-Antworten.

Netzwerkweit gültige Schlüssel werden ausschließlich als Netzwerkoption getestet. Sie dürfen für Site Admins weder sichtbar noch veränderbar sein.

## Prüfungen vor einer Freigabe

Vor einer Freigabe werden, soweit für das Projekt relevant, mindestens diese Prüfungen durchgeführt:

- Build, Linting und projektspezifische Tests;
- PHP-Prüfungen und WordPress Plugin Check ohne Fehler;
- Aktivierung und Deaktivierung auf einer Site sowie netzwerkweit, falls unterstützt;
- Rollen- und Berechtigungsprüfungen in Network Admin und Site Admin;
- Block-Editor: Einfügen, Bearbeiten, Aktualisierung und Abwärtskompatibilität;
- Frontend und Backend auf deutscher und englischer Site;
- Tastaturbedienung, sichtbarer Fokus, Formularlabels und Fehlermeldungen;
- Browserkonsole, Fehlerprotokoll und Ausfall externer Dienste;
- Update und Migration aus einer unterstützten Vorversion.

Das Prüfergebnis dokumentiert klar, was bestanden, fehlgeschlagen, nicht anwendbar oder mangels Umgebung nicht ausgeführt wurde. Eine statische Codeprüfung darf nicht als durchgeführter WordPress-, Multisite-, PCP- oder Barrierefreiheitstest ausgegeben werden.

## Aktualisierung und Pflege

WordPress, PHP, Themes, Referenz-Plugins und Entwicklungswerkzeuge müssen regelmäßig auf die im CMS-Angebot des RRZE relevanten Versionen aktualisiert werden. Vor größeren WordPress- und PHP-Updates wird mindestens ein vollständiger Smoke-Test der wichtigen Plugins und Themes durchgeführt.

Die Entwicklungsumgebung ist dokumentiert und über Konfiguration, Skripte oder Infrastrukturdefinition reproduzierbar. Lokale Zugangsdaten, Testdaten mit Personenbezug und temporäre Artefakte gehören nicht in Git.
