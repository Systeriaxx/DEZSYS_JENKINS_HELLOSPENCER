# CI/CD Pipeline "Hello Spencer"

**Projekt:** Python/Flask REST-API mit automatisierter Jenkins-Pipeline

**Repository:** `https://github.com/ThomasMicheler/DEZSYS_JENKINS_HELLOSPENCER.git`

**Ziel:** Automatisierung von Build, Test und Deployment (Grundanforderung / gk)

## 1. Systemumgebung

* **CI/CD-Plattform:** Jenkins (ausgeführt als Docker-Container mit Host-Docker-Zugriff via `/var/run/docker.sock`).
* **Pipeline-Agent:** Dynamischer Docker-Container mit dem Image `python:3.11`.
* **Applikation:** Flask-Webserver (Port `5556`) mit persistentem Text-Zähler (`count.txt`).

## 2. Pipeline-Phasen (Stages) & Ergebnisse

Die Pipeline durchläuft laut `Jenkinsfile` folgende Schritte fehlerfrei:

1. **Pre-Build Cleanup:** Beendet eventuell noch im Hintergrund laufende alte Instanzen der Applikation.
2. **Checkout:** Lädt den aktuellen Quellcode aus dem GitHub-Repository (`main`-Branch).
3. **Build:** Aktualisiert `pip`, installiert alle Abhängigkeiten (`flask`, `requests`, `pytest`) und initialisiert die Datei `count.txt` für den API-Zähler.
4. **Test (Unit Tests):** Führt die Modultests in `tests/test_hello.py` aus. **Ergebnis:** 3 von 3 Tests bestanden (`PASSED`).
5. **Run (Deployment):** Startet die Flask-App im Hintergrund (`nohup`) und verifiziert die Erreichbarkeit mittels `curl`. **Ergebnis:** API antwortet erfolgreich mit Status `200` und erhöhtem Zählerstand.
6. **Test API (Integrationstests):** Führt Blackbox-API-Tests über `tests/test_api.py` aus (Endpunkt-Validierung und Antwortzeit < 1 Sekunde). **Ergebnis:** Alle Tests erfolgreich (`OK`).
7. **Keep Alive:** Hält den Container künstlich aktiv (`sleep infinity`), damit die App für manuelle Reviews erreichbar bleibt.

## 3. Fazit

Die Pipeline läuft vollständig automatisiert und fehlerfrei durch. Alle Phasen von der Code-Beschaffung über die Testabdeckung (Unit- und Integrationstests) bis hin zum simulierten Live-Betrieb wurden erfolgreich validiert. Die Kriterien für die Middleware-Aufgabenstellung sind damit erfüllt.