---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 8146489a913ce863cee7534331231a248a3ea7ac
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56890926"
---
1. Rufen Sie auf dem Mac das [Azure-Portal] auf. Klicken Sie auf **Alle Dienste** > **App Services** und dann auf das Back-End, das Sie soeben erstellt haben. Wählen Sie in den Einstellungen der mobilen App Ihre bevorzugte Sprache aus:

    - Objective-C &ndash; **Schnellstart** > **iOS (Objective-C)**
    - Swift &ndash; **Schnellstart** > **iOS (Swift)**

    Klicken Sie unter **3. Konfigurieren Sie Ihre Clientanwendung**, und klicken Sie auf **Herunterladen**. Damit laden Sie ein vollständiges Xcode-Projekt herunter, das für eine Verbindung mit dem Back-End vorkonfiguriert ist. Öffnen Sie das Projekt in Xcode.

1. Klicken Sie auf die Schaltfläche **Ausführen** , um das Projekt zu erstellen und die App im iOS-Simulator zu starten.

1. Klicken Sie in der App auf das Plussymbol (**+**), geben Sie einen sinnvollen Text ein, wie z. B. *Tutorial abschließen*, und klicken Sie dann auf die Schaltfläche „Speichern“. Damit wird eine POST-Anforderung an das Azure-Back-End gesendet, das Sie zuvor bereitgestellt haben. Das Back-End fügt Daten aus der Anforderung in die TodoItem-SQL-Tabelle ein und gibt Informationen über die neu gespeicherten Elemente an die mobile App zurück. Die mobile App zeigt diese Daten in der Liste an.

   ![Schnellstart-App unter iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Azure-Portal]: https://portal.azure.com/
