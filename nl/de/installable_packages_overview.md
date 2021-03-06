---

copyright:
  years: 2018
lastupdated: "2018-07-19"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}

# Installierbare Pakete verwenden
{: #installable-packages-overview}

Installierbare Pakete stellen ein neues Design für die Verwaltung und Interaktion mit Paketen in {{site.data.keyword.openwhisk}} dar.{: shortdesc}

## Übersicht
{: #overview}

**Was sind installierbare Pakete?**

Pakete sind gebündelte Gruppen zusammengehöriger Aktionen und Feeds. Jedes Paket ist für eine ganz bestimmte Interaktion mit Services und Ereignisprovidern konzipiert. Installierbare Pakete sind Pakete, die Ihnen nach Bedarf zum Auswählen, Installieren und Bearbeiten zur Verfügung stehen.

**Unterschied zwischen installierbaren und vorinstallierten Paketen**

[Vorinstallierte Pakete](openwhisk_packages.html#browse-packages) werden in {{site.data.keyword.openwhisk_short}} automatisch im Namensbereich `/whisk.system` registriert. Wenn Sie Berechtigungsnachweise oder andere Parameter in einem vorinstallierten Paket speichern wollen, müssen Sie [Paketbindungen](openwhisk_packages.html#openwhisk_package_bind) erstellen.

Installierbare Pakete befinden sich nicht innerhalb des {{site.data.keyword.openwhisk_short}}-Systems. Stattdessen sind installierbare Pakete extern in einzelnen Github-Repositorys enthalten. Diese Pakete können Sie mit dem Tool [wskDeploy](https://github.com/apache/incubator-openwhisk-wskdeploy#whisk-deploy-wskdeploy) direkt in Ihrem eigenen Namensbereich installieren und so umbenennen, dass sie einen angepassten Namen erhalten. Da das Paket in Ihrem eigenen Namensbereich installiert ist, können Sie die darin enthaltenen Aktionen und Feeds nach Bedarf ändern.

## Pakete mit wskDeploy installieren
{: #installing}

Vorbereitende Schritte:
  1. [Installieren Sie das {{site.data.keyword.openwhisk_short}}-Plug-in für die {{site.data.keyword.Bluemix_notm}}-CLI](bluemix_cli.html#cloudfunctions_cli).
  2. Installieren Sie den Befehl [`wskdeploy` ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk-wskdeploy/releases) und fügen Sie die heruntergeladene Binärdatei zu Ihrem Pfad (PATH) hinzu.

So installieren Sie ein Paket:

1. Klonen Sie das Repository für das Paket. Sie finden Paketrepositorys auf den einzelnen Seiten für jedes Paket in dieser Dokumentationsreihe.
    ```
    git clone https://github.com/<package_repo>
    ```
    {: pre}

2. Navigieren Sie zu dem Paketverzeichnis, das eine Datei `manifest.yaml` oder `manifest.yml` enthält. Die Datei `manifest.yaml` gibt die Gesamtstruktur des Pakets einschließlich Paket und Aktionen, die in Ihrem Namensbereich installiert werden sollen, sowie aller Metadaten an, die die mit dem Befehl `wskdeploy` eingebunden werden müssen. Weitere Informationen zu Dateien vom Typ `manifest.yaml` enthält die [Dokumentation zu wskdeploy ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#wskdeploy-utility-by-example).
    ```
    cd <filepath>/<package_name>
    ```
    {: pre}

3. Stellen Sie das Paket bereit. Manche Pakete erfordern bestimmte Umgebungsvariablen, damit das Paket ordnungsgemäß funktionieren kann.
    ```
    wskdeploy -m manifest.yaml
    ```
    {: pre}

### Beispiel für die Verwendung des {{site.data.keyword.cos_full_notm}}-Pakets
{: #example}

Wenn Sie ein Beispiel für die Installation eines Pakets sehen möchten, prüfen Sie das [{{site.data.keyword.cos_short}}-Paket](cloud_object_storage_actions.html). {{site.data.keyword.cos_full}} ist ein Service, der Benutzern das Speichern jeglicher Arten von Dateien wie Bildern, Videos, Musik und Text ermöglicht. Für die Interaktion mit den Dateien wird in einem Bucket ein Cloud-basierter Datenspeicher von Schlüssel/Wert-Paaren gespeichert. Damit Sie das [{{site.data.keyword.cos_short}}-Paket](cloud_object_storage_actions.html) verwenden können, müssen Sie zuerst eine {{site.data.keyword.cos_short}}-Serviceinstanz und anschließend ein Bucket erstellen. Das Bucket wird als Umgebungsvariable verwendet, die zum Installieren dieses Pakets erforderlich ist.

Nachdem Sie die Serviceinstanz und das Bucket erstellt haben, sind die folgenden Befehle zum Installieren des Pakets erforderlich:

1. Klonen Sie das Repository für das Paket
    ```
    git clone https://github.com/ibm-functions/package-cloud-object-storage.git
    ```
    {: pre}

2. Navigieren Sie zu dem Paketverzeichnis, das die Datei `manifest.yaml` enthält. In diesem Beispiel wird die Node.js-Laufzeitversion des {{site.data.keyword.cos_short}}-Pakets verwendet.
    ```
    cd package-cloud-object-storage/runtimes/nodejs
    ```
    {: pre}

3. Implementieren Sie das Paket. Verwenden Sie dazu Ihr Bucket als Umgebungsvariable. Die Abhängigkeit von der Umgebungsvariablen `PACKAGE_NAME` ermöglicht Ihnen, dem Paket einen angepassten Namen zu geben.
    ```
    PACKAGE_NAME=<custom_package_name> BUCKET=<bucket_name> wskdeploy
    ```
    {: pre}
