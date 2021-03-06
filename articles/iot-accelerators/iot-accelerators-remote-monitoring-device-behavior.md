---
title: Verhalten simulierter Geräte in der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie JavaScript verwenden, um das Verhalten eines simulierten Geräts in der Remoteüberwachungslösung zu definieren.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: c2151a4b1eb2a853ed343f6720b4f53af5e5e449
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449519"
---
# <a name="implement-the-device-model-behavior"></a>Implementieren des Gerätemodellverhaltens

Der Artikel [Grundlegendes zum Gerätemodellschema](iot-accelerators-remote-monitoring-device-schema.md) beschreibt das Schema, das ein simuliertes Gerätemodell definiert. Dieser Artikel bezieht sich auf zwei Arten der JavaScript-Datei, die das Verhalten eines simulierten Geräts implementiert:

- **State**-JavaScript-Dateien, die in festen Intervallen ausgeführt werden, um den internen Status des Geräts zu aktualisieren.
- **Method**-JavaScript-Dateien, die ausgeführt werden, wenn die Lösung eine Methode auf dem Gerät aufruft.

> [!NOTE]
> Gerätemodellverhalten gelten nur für simulierte Geräte, die im Gerätesimulationsdienst gehostet werden. Wenn Sie ein echtes Gerät erstellen möchten, finden Sie weitere Informationen unter [Herstellen einer Verbindung zwischen Ihrem Gerät und dem Solution Accelerator für Remoteüberwachung](iot-accelerators-connecting-devices.md).

In diesem Artikel werden folgende Vorgehensweisen behandelt:

>[!div class="checklist"]
> * Steuern des Status eines simulierten Geräts
> * Definieren, wie ein simuliertes Gerät auf einen Methodenaufruf von der Remoteüberwachungslösung reagiert
> * Debuggen Ihrer Skripts

[!INCLUDE [iot-accelerators-device-behavior](../../includes/iot-accelerators-device-behavior.md)]

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt, wie Sie das Verhalten Ihres eigenen benutzerdefinierten simulierten Gerätemodells definieren. In diesem Artikel haben Sie erfahren, wie Sie Folgendes durchführen:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Steuern des Status eines simulierten Geräts
> * Definieren, wie ein simuliertes Gerät auf einen Methodenaufruf von der Remoteüberwachungslösung reagiert
> * Debuggen Ihrer Skripts

Jetzt wissen Sie, wie Sie das Verhalten eines simulierten Geräts festlegen, und sollten als Nächstes das [Erstellen eines neuen simulierten Geräts](iot-accelerators-remote-monitoring-create-simulated-device.md) kennenlernen.

Weitere Entwicklerinformationen zur Remoteüberwachungslösung finden Sie hier:

* [Referenzhandbuch für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Handbuch zur Problembehandlung für Entwickler](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
