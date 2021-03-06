---
title: Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack| Microsoft-Dokumentation
description: Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/19/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 59adaed427eb0d791e18438826aff4f6477e831a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247271"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Die allgemeinen Parameter umfassen Werte wie z.B. Umgebungsvariablen und Benutzeranmeldeinformationen, die für sämtliche Tests bei Validation-as-a-Service (VaaS) erforderlich sind. Diese Werte werden auf Workflowebene definiert, wenn Sie einen Workflow erstellen oder ändern. Bei der Planung von Tests werden diese Werte an jeden Test im Workflow als Parameter übergeben.

> [!NOTE]
> Für jeden Test ist ein eigener Satz von Parametern definiert. Zum Planungszeitpunkt müssen Sie unter Umständen unabhängig von den allgemeinen Parametern einen Wert eingeben, oder Sie können den Wert des allgemeinen Parameters überschreiben.

## <a name="environment-parameters"></a>Umgebungsparameter

Umgebungsparameter beschreiben die Azure Stack-Umgebung, die getestet wird. Diese Werte müssen durch Generieren und Hochladen einer Azure Stack-Stempelinformationsdatei für die jeweilige Instanz, die Sie testen, angegeben werden.

> [!NOTE]
> In offiziellen Validierungsworkflows können Umgebungsparameter nach der Workflowerstellung nicht mehr geändert werden.

### <a name="generate-the-stamp-information-file"></a>Generieren der Stempelinformationsdatei

1. Melden Sie sich bei der DVM oder einem anderen Computer mit Zugriff auf die Azure Stack-Umgebung an.
2. Führen Sie die folgenden Befehle in einem PowerShell-Fenster mit erhöhten Rechten aus:
    ```PowerShell  
    $CloudAdminUser = "<cloud admin username>"
    $CloudAdminPassword = ConvertTo-SecureString "<cloud admin password>" -AsPlainText -Force
    $stampInfoCreds = New-Object System.Management.Automation.PSCredential($CloudAdminUser, $CloudAdminPassword)
    $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation' -Credential $stampInfoCreds
    ConvertTo-Json $params > stampinfoproperties.json
    ```

### <a name="locate-values-in-the-ece-configuration-file"></a>Suchen von Werten in der ECE-Konfigurationsdatei

Umgebungsparameterwerte können auch manuell in der **ECE-Konfigurationsdatei** gesucht werden, die sich unter `C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787` auf der DVM befindet.

## <a name="test-parameters"></a>Testparameter

Allgemeine Testparameter umfassen vertrauliche Informationen, die nicht in Konfigurationsdateien gespeichert werden können. Diese müssen manuell bereitgestellt werden.

Parameter    | BESCHREIBUNG
-------------|-----------------
Mandantenadministratorbenutzer                            | Azure Active Directory-Mandantenadministrator, der vom Dienstadministrator im AAD-Verzeichnis bereitgestellt wurde. Dieser Benutzer führt auf Mandantenebene Aktionen wie das Bereitstellen von Vorlagen zum Einrichten von Ressourcen (VMs, Speicherkonten usw.) und Ausführen von Workloads aus. Weitere Informationen zum Bereitstellen von Mandantenkonten finden Sie unter [Hinzufügen eines neuen Azure Stack-Mandanten](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-new-user-aad).
Dienstadministratorbenutzer             | Der Azure Active Directory-Administrator des Mandanten des AAD-Verzeichnisses, der während der Azure Stack-Bereitstellung angegeben wird. Suchen Sie in der ECE-Konfigurationsdatei nach `AADTenant`, und wählen Sie den Wert im `UniqueName`-Element aus.
Cloudadministratorbenutzer               | Azure Stack-Domänenadministratorkonto (z.B. `contoso\cloudadmin`). Suchen Sie in der ECE-Konfigurationsdatei nach `User Role="CloudAdmin"`, und wählen Sie den Wert im `UserName`-Element aus.
Diagnoseverbindungszeichenfolge          | Eine SAS-URL zu einem Azure Storage-Konto, in das während der Testausführung Diagnoseprotokolle kopiert werden. Anweisungen zum Generieren der SAS-URL finden Sie unter [Generieren der Diagnose-Verbindungszeichenfolge](#generate-the-diagnostics-connection-string). |

> [!IMPORTANT]
> Die **Diagnose-Verbindungszeichenfolge** muss vor dem Fortfahren gültig sein.

### <a name="generate-the-diagnostics-connection-string"></a>Generieren der Diagnose-Verbindungszeichenfolge

Die Diagnose-Verbindungszeichenfolge ist für das Speichern von Diagnoseprotokollen während der Testausführung erforderlich. Verwenden Sie das Azure Storage-Konto, das Sie während der Einrichtung erstellt haben (siehe [Einrichten Ihrer Validation-as-a-Service-Ressourcen](azure-stack-vaas-set-up-resources.md)), um eine SAS-URL (Shared Access Signature) zu erstellen, über die VaaS Zugriff auf Ihr Storage-Konto zum Hochladen von Protokollen erhält.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Aktivieren Sie die Option **Blob** unter **Zulässige Dienste**. Deaktivieren Sie alle verbleibenden Optionen.

1. Aktivieren Sie unter **Zugelassene Ressourcentypen** die Optionen **Dienst**, **Container** und **Objekt**.

1. Aktivieren Sie unter **Zugelassene Berechtigungen** die Optionen **Lesen**, **Schreiben**, **Auflisten**, **Hinzufügen** und **Erstellen**. Deaktivieren Sie alle verbleibenden Optionen.

1. Legen Sie die **Startzeit** auf die aktuelle Zeit und die **Endzeit** auf drei Monate nach der aktuellen Zeit fest.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]

> [!NOTE]  
> Die SAS-URL läuft zu der beim Generieren der URL angegebenen Endzeit ab.  
Stellen Sie bei der Planung von Tests sicher, dass die URL zusätzlich zu der für die Testausführung erforderlichen Zeit für mindestens 30 Tage gültig ist. (Drei Monate werden empfohlen.)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Wichtige Begriffe zu Validation-as-a-Service](azure-stack-vaas-key-concepts.md)