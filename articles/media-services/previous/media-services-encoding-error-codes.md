---
title: Fehlercodes bei der Azure Media Services-Codierung | Microsoft-Dokumentation
description: Dieses Thema enthält Fehlercodes, die zurückgegeben werden können, wenn bei der Ausführung einer Verschlüsselungsaufgabe ein Fehler auftritt.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 5ab537d94c56f1d5a9a0b32e3a18a416baf74a75
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2019
ms.locfileid: "55995746"
---
# <a name="encoding-error-codes"></a>Verschlüsselungsfehlercodes

Die folgende Tabelle enthält die Fehlercodes, die zurückgegeben werden können, wenn bei der Ausführung einer Codieraufgabe ein Fehler auftritt.  Verwenden Sie zum Abrufen von Fehlerdetails im .NET-Code die [ErrorDetails](https://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) -Klasse. Verwenden Sie zum Abrufen von Fehlerdetails im REST-Code die [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) -REST-API.

| ErrorDetail.Code | Mögliche Ursachen des Fehlers |
| --- | --- |
| Unknown |Unbekannter Fehler beim Ausführen der Aufgabe |
| ErrorDownloadingInputAssetMalformedContent |Kategorie von Fehlern, die Fehler beim Herunterladen von Eingabe-Assets abdeckt, z. B. fehlerhafte Dateinamen, Dateien mit Nulllänge, falsche Formate usw. |
| ErrorDownloadingInputAssetServiceFailure |Kategorie von Fehlern, die Probleme auf der Dienstseite abdeckt, z. B. Netzwerk- oder Speicherfehler beim Herunterladen. |
| ErrorParsingConfiguration |Kategorie von Fehlern, bei denen die Aufgabe <see cref="MediaTask.PrivateData"/> (Konfiguration) nicht gültig ist, z.B. wenn die Konfiguration keine gültige Systemvoreinstellung ist oder ungültige XML-Daten enthält. |
| ErrorExecutingTaskMalformedContent |Kategorie von Fehlern während der Ausführung der Aufgabe, wenn Probleme in den Eingabemediendateien zu Fehlern führen. |
| ErrorExecutingTaskUnsupportedFormat |Kategorie von Fehlern, bei denen die bereitgestellten Dateien mit dem Medienprozessor nicht verarbeitet werden können: keine Unterstützung des Medienformats oder fehlende Übereinstimmung mit der Konfiguration. Beispiel: Der Versuch, eine Audioausgabe für ein Asset zu erstellen, die nur Video ermöglicht. |
| ErrorProcessingTask |Kategorie anderer Fehler, die für den Medienprozessor beim Verarbeiten der Aufgabe auftreten und sich nicht auf den Inhalt beziehen. |
| ErrorUploadingOutputAsset |Kategorie von Fehlern beim Hochladen des Ausgabe-Assets. |
| ErrorCancelingTask |Kategorie von Fehlern, die auftreten, wenn versucht wird, die Aufgabe abzubrechen. |
| TransientError |Kategorie von Fehlern, mit der vorübergehende Probleme abgedeckt werden (z.B. temporäre Netzwerkprobleme mit Azure Storage) |

Öffnen Sie ein **Supportticket** , um Hilfe vom [Media Services](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)-Team anzufordern.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwandte Artikel
* [Ausführen von Aufgaben für die erweiterte Codierung durch Anpassen der Media Encoder Standard-Voreinstellungen](media-services-custom-mes-presets-with-dotnet.md)
* [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
