---
title: Includedatei
description: Includedatei
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 9a9f3cd2668f0e78441548592f38ade8ddc0fe46
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52331025"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen der Registrierungsinformationen der Anwendung zu Ihrer App

In diesem Schritt müssen Sie die Umleitungs-URL der Registrierungsinformationen für die Anwendung konfigurieren und dann die Anwendungs-ID zu Ihrer JavaScript-SPA-Anwendung hinzufügen.

### <a name="configure-redirect-url"></a>Konfigurieren der Umleitungs-URL

Konfigurieren Sie das Feld `Redirect URL` mit der URL für Ihre Seite „index.html“ basierend auf Ihrem Webserver, und klicken Sie dann auf *Aktualisieren*.

> #### <a name="visual-studio-instructions-for-obtaining-the-redirect-url"></a>Visual Studio-Anleitung zum Abrufen der Umleitungs-URL
> Führen Sie diese Schritte aus, um die Umleitungs-URL zu erhalten:
> 1. Wählen Sie im **Projektmappen-Explorer** das Projekt aus, und sehen Sie sich das **Eigenschaftenfenster** an. Wenn kein **Eigenschaftenfenster** angezeigt wird, können Sie **F4** drücken.
> 2. Kopieren Sie den Wert aus **URL** in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3. Fügen Sie den Wert oben auf dieser Seite als **Umleitungs-URL** ein, und wählen Sie dann **Aktualisieren** aus.

<p/>

> #### <a name="setting-redirect-url-for-node"></a>Festlegen der Umleitungs-URL für Node
> Für Node.js können Sie den Webserverport in der Datei *server.js* festlegen. In diesem Tutorial wird der Port 30662 als Referenz genutzt, aber Sie können auch einen beliebigen anderen Port verwenden, der verfügbar ist. Befolgen Sie die unten angegebene Anleitung zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> Legen Sie `http://localhost:30662/` oben auf dieser Seite als **Umleitungs-URL** fest, oder verwenden Sie `http://localhost:[port]/` bei einem benutzerdefinierten TCP-Port (wobei *[port]* die benutzerdefinierte TCP-Portnummer ist). Klicken Sie anschließend auf **Aktualisieren**.

### <a name="configure-your-javascript-spa-application"></a>Konfigurieren Ihrer JavaScript-Einzelseitenanwendung

1. Fügen Sie in der Datei `index.html`, die beim Einrichten des Projekts erstellt wurde, die Informationen für die Anwendungsregistrierung hinzu. Fügen Sie in den Tags `<script></script>` oben im Text der Datei `index.html` den folgenden Code hinzu:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    authority: "https://login.microsoftonline.com/common",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
