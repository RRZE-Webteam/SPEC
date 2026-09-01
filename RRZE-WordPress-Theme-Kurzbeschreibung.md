# Vorgaben an WordPress-Themes

( Version: 1.1,  Date: 01.09.2026,  Source: https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Theme-Kurzbeschreibung.md )

Für Themes, welche auf der zentralen CMS-Instanz der FAU eingesetzt werden sollen, müssen verbindliche technische, organisatorische und redaktionelle Rahmenbedingungen erfüllt sein. Themes, welche diese Bedingungen nicht einhalten, können nicht auf der zentralen CMS-Instanz eingesetzt werden.

Die vollständigen und jeweils maßgeblichen Vorgaben stehen im GitHub-Repository:

- [RRZE-WordPress-Theme.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Theme.md)
- [RRZE-WordPress-Theme-LLM-Shortcut.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Theme-LLM-Shortcut.md)
- [RRZE-WordPress-Plugin.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Plugin.md)
- [RRZE-WordPress-Entwicklungsumgebung.md](https://github.com/RRZE-Webteam/SPEC/RRZE-WordPress-Entwicklungsumgebung.md)

## Grundlegende Anforderungen

1. Für das jeweilige Theme muss stets ein fachkompetenter Ansprechpartner vorhanden sein, der im Falle von Problemen oder Fehlern zeitnah reagiert.
2. Das Theme muss als Mindestanforderung kompatibel zur jeweils aktuellen WordPress- und PHP-Version sein.
3. Neue Themes für das CMS-Angebot des RRZE müssen Block Editor Themes sein. Classic Themes sollen nicht mehr neu verwendet werden; Ausnahmen sind nur als ausdrücklich abgestimmte Übergangs- oder Kompatibilitätslösung möglich.
4. Themes dürfen keine Funktionen übernehmen, die in die Plugin-Domain gehören. Funktionale Erweiterungen, Datenverarbeitung, externe Dienste, komplexe Workflows, Custom Post Types, Rollenlogik oder vergleichbare Fachlogik gehören in Plugins.
5. Themes dürfen keine eigenen Pagebuilder enthalten, voraussetzen oder integrieren. Der WordPress Block Editor ist die verbindliche Grundlage.
6. Themes dürfen keine eigenen Plugin-Installer, Marketplace-Installer, Update-Installer oder vergleichbare Mechanismen zum Nachladen von Plugins oder ausführbarem Code mitbringen.
7. Alle Ausgaben, die durch das Theme erzeugt werden, müssen nach WCAG 2.2 in der Konformitätsstufe AA barrierefrei sein. Bei Formularen und anderen Eingabeworkflows ist WCAG 2.2 AAA ein gewünschtes Qualitätsziel, aber keine zwingende Mindestanforderung.
8. Die Bereitstellung des Themes muss über WordPress, GitHub oder GitLab erfolgen. Eine manuelle Aktualisierung über zugesandte ZIP-Dateien ist nicht Teil des Standardprozesses.
9. Die programmiertechnische Fehlerfreiheit ist zu gewährleisten. Das Theme muss mit [Theme Check](https://wordpress.org/plugins/theme-check/) geprüft werden; produktiv darf es keine Warnings oder Fatal Errors erzeugen.
10. Themes müssen in einer geeigneten WordPress-Testumgebung mit aktivem Debug-Logging entwickelt und geprüft werden. Für RRZE-CMS-Themes ist Multisite-Verhalten zu berücksichtigen.
11. JavaScript- und CSS-Dateien dürfen nur dort geladen werden, wo sie benötigt werden. Produktionsdateien müssen minifiziert sein; Source Maps gehören nicht in produktive Auslieferungen.
12. JavaScript-Bibliotheken, CSS-Bibliotheken, Schriften, Icons und vergleichbare Laufzeitressourcen sind lokal mit dem Theme auszuliefern oder aus WordPress Core zu verwenden. Öffentliche CDNs sind standardmäßig nicht zulässig.
13. Für nicht triviale CSS-Entwicklung soll ein Präprozessor wie SASS/SCSS mit geeignetem Buildprozess verwendet werden. Vendor-Präfixe sollen über Werkzeuge wie Autoprefixer erzeugt werden.
14. Das Theme muss grundsätzlich ohne zusätzliche Plugins lauffähig sein. Wenn es mit bestimmten Plugins visuell oder fachlich zusammenspielt, müssen diese Abhängigkeiten dokumentiert und begründet werden.
15. Einschränkungen der WordPress-Standardfunktionen, etwa im Site Editor oder bei „Zusätzliches CSS“, müssen über unterstützte WordPress-Mechanismen wie `theme.json`, `functions.php` oder geeignete Filter umgesetzt, begründet und dokumentiert werden.
16. Wenn ein Theme eigene Blöcke, Block-Variationen, Block-Styles oder Patterns bereitstellt, gelten die Block-Anforderungen der vollständigen Theme-Spezifikation und der Plugin-Spezifikation entsprechend.
17. `README.md`, `readme.txt`, `style.css`, `package.json` sowie Versions- und Kompatibilitätsangaben müssen konsistent gepflegt werden. Die `README.md` muss auf die maßgebliche Benutzerdokumentation verweisen.
18. Werden durch ein Theme externe Ressourcen abgerufen oder angezeigt, sind Datenschutz, Consent, Barrierefreiheit und Dokumentation zu berücksichtigen. Automatisierte externe Abrufe müssen die FAU-Crawler-Regeln und deren User-Agent-Vorgaben einhalten.

## Betriebsbedingungen auf der CMS-Instanz der FAU

Bei einem Einsatz auf der zentralen CMS-Instanz der FAU gelten außerdem diese Bedingungen:

- WordPress- und PHP-Updates haben Vorrang vor der Funktionsfähigkeit einzelner Themes. Entwicklerinnen und Entwickler müssen ihre Themes rechtzeitig auf kommende Versionen vorbereiten.
- Themes, die nach einem Update Fehler verursachen, Sicherheitsrisiken erzeugen oder nicht mehr gepflegt werden, können deaktiviert oder aus dem unterstützten Portfolio entfernt werden.
- Themes ohne erreichbaren fachkompetenten Ansprechpartner oder ohne dauerhafte Wartungsverantwortung sind für den produktiven Betrieb nicht geeignet.
- Die Präfixe `rrze-`, `fau-`, `utn-` und `cms-` dürfen nur nach Abstimmung mit dem RRZE verwendet werden.

## Kontakt

Für Rückfragen:

- RRZE-Webteam
- E-Mail: [webmaster@fau.de](mailto:webmaster@fau.de)
- Website: [www.rrze.fau.de](https://www.rrze.fau.de), [www.wp.rrze.fau.de](https://www.wp.rrze.fau.de)
- Matrix-Channel: `#web:fau.de`
