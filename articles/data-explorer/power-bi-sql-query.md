---
title: 'Schnellstart: Visualisieren von Daten mithilfe einer SQL-Abfrage in Power BI'
description: 'In diesem Schnellstart erfahren Sie, wie Sie eine der drei Optionen zum Visualisieren von Daten in Power BI verwenden: eine SQL-Abfrage für einen Azure Data Explorer-Cluster.'
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 11/14/2018
ms.openlocfilehash: 4eb6fdd5b5a78f0afb00265ebd957231464a9083
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977283"
---
# <a name="quickstart-visualize-data-using-the-azure-data-explorer-connector-for-power-bi"></a>Schnellstart: Visualisieren von Daten mithilfe des Azure Data Explorer-Connectors für Power BI

Azure-Daten-Explorer ist ein schneller und hochgradig skalierbarer Dienst zur Untersuchung von Daten (Protokoll- und Telemetriedaten). Power BI ist eine Business Analytics-Lösung, mit der Sie Ihre Daten visualisieren und die Ergebnisse in Ihrer Organisation teilen können.

Azure Data Explorer bietet drei Optionen für die Verbindungsherstellung mit Daten in Power BI: Verwenden des integrierten Connectors, Importieren einer Abfrage aus Azure Data Explorer oder Verwenden einer SQL-Abfrage. In diesem Schnellstart wird erläutert, wie Sie mit einer SQL-Abfrage Daten abrufen und in einem Power BI-Bericht visualisieren.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für diesen Schnellstart benötigen Sie Folgendes:

* Ein Organisations-E-Mail-Konto, das Azure Active Directory angehört, um eine Verbindung mit dem [Azure Data Explorer-Hilfecluster](https://dataexplorer.azure.com/clusters/help/databases/samples) herstellen zu können.

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) (wählen Sie **KOSTENLOS HERUNTERLADEN** aus)

## <a name="get-data-from-azure-data-explorer"></a>Abrufen von Daten aus Azure Data Explorer

Sie stellen zunächst eine Verbindung mit dem Azure Data Explorer-Hilfecluster her, und anschließend fügen Sie eine Teilmenge der Daten aus der Tabelle *StormEvents* ein. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

In der Regel verwenden Sie die native Abfragesprache mit Azure Data Explorer. Hier arbeiten Sie jedoch mit SQL-Abfragen, die ebenfalls unterstützt werden. Azure Data Explorer übersetzt die SQL-Abfrage für Sie in eine native Abfrage.

1. Wählen Sie in Power BI Desktop auf der Registerkarte **Start** die Option **Daten abrufen** und anschließend **Mehr** aus.

    ![Datensammlung](media/power-bi-sql-query/get-data-more.png)

1. Suchen Sie nach *Azure SQL-Datenbank*, wählen Sie **Azure SQL-Datenbank** und dann **Verbinden** aus.

    ![Suchen und Abrufen von Daten](media/power-bi-sql-query/search-get-data.png)

1. Füllen Sie das Formular auf dem Bildschirm **SQL Server-Datenbank** mit den folgenden Informationen aus.

    ![Datenbank, Tabelle und Abfrageoptionen](media/power-bi-sql-query/database-table-query.png)

    **Einstellung** | **Wert** | **Feldbeschreibung**
    |---|---|---|
    | Server | *help.kusto.windows.net* | Die URL für den Hilfecluster (ohne *https://*). Für andere Cluster hat die URL das Format *\<Clustername\>.\<Region\>.kusto.windows.net*. |
    | Datenbank | *Beispiele* | Die Beispieldatenbank, die in dem Cluster gehostet wird, mit dem Sie eine Verbindung herstellen. |
    | Datenkonnektivitätsmodus | *Import* | Bestimmt, ob Power BI die Daten importiert oder direkt eine Verbindung mit der Datenquelle herstellt. Für diesen Connector sind beide Optionen geeignet. |
    | Befehlstimeout | Nicht ausfüllen | Bestimmt, wie lange die Abfrage ausgeführt wird, bevor ein Timeoutfehler ausgelöst wird. |
    | SQL-Anweisung | Kopieren Sie die Abfrage unter dieser Tabelle. | Die SQL-Anweisung, die Azure Data Explorer in eine native Abfrage übersetzt. |
    | Weitere Optionen | Übernehmen Sie die Standardwerte. | Die weiteren Optionen gelten nicht für Azure Data Explorer-Cluster. |
    | | | |

    ```SQL
    SELECT TOP 1000 *
    FROM StormEvents
    ORDER BY DamageCrops DESC
    ```

1. Falls Sie noch keine Verbindung mit dem Hilfecluster hergestellt haben, melden Sie sich an. Melden Sie sich mit einem Microsoft-Konto an, und wählen Sie dann **Verbinden** aus.

    ![Anmelden](media/power-bi-sql-query/sign-in.png)

1. Klicken Sie auf dem Bildschirm **help.kusto.windows.net: Beispiele**  auf **Laden**.

    ![Laden von Daten](media/power-bi-sql-query/load-data.png)

    Die Tabelle wird im Hauptfenster von Power BI in der Berichtsansicht geöffnet, in der Sie anhand der Beispieldaten Berichte erstellen können.

## <a name="visualize-data-in-a-report"></a>Visualisieren von Daten in einem Bericht

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Bericht, den Sie für diesen Schnellstart erstellt haben, nicht mehr benötigen, löschen Sie die Power BI Desktop-Datei (PBIX-Datei).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Quickstart: Visualize data using an imported query in Power BI](power-bi-connector.md) (Schnellstart: Visualisieren von Daten mithilfe einer importierten Abfrage in Power BI)