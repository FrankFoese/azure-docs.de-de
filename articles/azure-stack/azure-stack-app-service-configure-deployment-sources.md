---
title: Konfigurieren von Bereitstellungsquellen für App Services in Azure Stack | Microsoft-Dokumentation
description: Informationen dazu, wie ein Dienstadministrator Bereitstellungsquellen (Git, GitHub, BitBucket, Dropbox und OneDrive) für App Service in Azure Stack konfiguriert.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: f2101c685ff7b3820f826da1d2e1d52b687d26c6
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446630"
---
# <a name="configure-deployment-sources"></a>Konfigurieren von Bereitstellungsquellen

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

App Service in Azure Stack unterstützt die bedarfsgesteuerte Bereitstellung aus mehreren Quellcodeverwaltungsanbietern. Dieses Feature ermöglicht Anwendungsentwicklern eine Bereitstellung direkt aus ihren Repositorys zur Quellcodeverwaltung. Damit Benutzer App Service für das Herstellen einer Verbindung mit ihren Repositorys konfigurieren können, muss ein Cloudadministrator zunächst die Integration zwischen App Service in Azure Stack und dem Quellcodeverwaltungsanbieter konfigurieren.  

Neben lokalem Git werden die folgenden Quellcodeverwaltungsanbieter unterstützt:

* GitHub
* BitBucket
* OneDrive
* Dropbox

## <a name="view-deployment-sources-in-app-service-administration"></a>Anzeigen der Bereitstellungsquellen in der App Service-Verwaltung

1. Melden Sie sich am Azure Stack-Verwaltungsportal (https://adminportal.local.azurestack.external)) als Dienstadministrator an.
2. Navigieren Sie zu **Alle Dienste**, und wählen Sie den **App Service** aus.
    ![App Service Resource Provider Admin][1] (Administrator von App Service-Ressourcenanbieter)
3. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung). Die Liste aller konfigurierten Bereitstellungsquellen wird angezeigt.
    ![App Service Resource Provider Admin: Source Control Configuration][2] (Administrator von App Service-Ressourcenanbieter: Konfiguration der Quellcodeverwaltung)

## <a name="configure-github"></a>Konfigurieren von GitHub

Sie benötigen ein GitHub-Konto, um diese Aufgabe abzuschließen. Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

1. Melden Sie sich bei GitHub an, navigieren Sie zu https://www.github.com/settings/developers, und klicken Sie dann auf **Neue Anwendung registrieren**.
    ![GitHub – Neue Anwendung registrieren][3]
2. Geben Sie in **Application name** einen Anwendungsnamen ein, z.B. **App Service in Azure Stack**.
3. Geben Sie in **Homepage URL** die URL der Startseite ein. Die URL der Homepage muss die Adresse des Azure Stack-Portals sein. Beispiel: https://portal.local.azurestack.external.
4. Geben Sie in **Application Description** eine Anwendungsbeschreibung ein.
5. Geben Sie in **Authorization callback URL** die Autorisierungsrückruf-URL ein. In einer Standardbereitstellung von Azure Stack weist die URL das Format https://portal.local.azurestack.external/TokenAuthorize auf. Wenn die Ausführung in einer anderen Domäne erfolgt, ersetzen Sie „local.azurestack.external“ durch Ihren Domänennamen.
6. Klicken Sie zum Registrieren der Anwendung auf **Register application**. Nun wird eine Seite mit der **Client ID** und dem **Client Secret** (Clientgeheimnis) für die Anwendung angezeigt.
    ![GitHub: Anwendungsregistrierung abgeschlossen][5]
7.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster am Azure Stack-Verwaltungsportal (https://adminportal.local.azurestack.external)) als Dienstadministrator an.
8.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator des App Service-Ressourcenanbieters) aus.
9. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
10. Kopieren Sie die **Client-ID** und das **Clientgeheimnis**, und fügen Sie sie in die entsprechenden Eingabefelder für GitHub ein.
11. Klicken Sie auf **Speichern**.

## <a name="configure-bitbucket"></a>Konfigurieren von BitBucket

Sie benötigen ein Bitbucket-Konto, um diese Aufgabe abzuschließen. Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

1. Melden Sie sich bei BitBucket an, und navigieren Sie unter Ihrem Konto zu **Integrations** (Integrationen).
    ![BitBucket-Dashboard: Integrationen][7]
2. Klicken Sie unter „Access Management“ (Zugriffsverwaltung) auf **OAuth** und **Add Consumer** (Consumer hinzufügen).
    ![Bitbucket – OAuth-Consumer hinzufügen][8]
3. Geben Sie unter **Name** einen Namen für den Consumer ein, z.B. **App Service in Azure Stack**.
4. Geben Sie in **Description** eine Beschreibung der Anwendung ein.
5. Geben Sie in **Callback URL** die Rückruf-URL ein. In einer Standardbereitstellung von Azure Stack weist die Rückruf-URL das Format https://portal.local.azurestack.external/TokenAuthorize auf. Wenn die Ausführung in einer anderen Domäne erfolgt, ersetzen Sie „azurestack.local“ durch Ihren Domänennamen. Die URL muss die hier aufgeführte Groß-/Kleinschreibung befolgen, damit die BitBucket-Integration erfolgreich ist.
6. Geben Sie die **URL** ein. Diese URL muss die des Azure Stack-Portals sein, z.B. https://portal.local.azurestack.external.
7. Wählen Sie unter **Permissions** die erforderlichen Berechtigungen aus:
    - **Repositorys**: *Lesen*
    - **Webhooks**: *Lesen und Schreiben*
8. Klicken Sie auf **Speichern**. Unter **OAuth consumers** (OAuth-Consumer) wird diese neue Anwendung nun zusammen mit **Key** (Schlüssel) und **Secret** (Geheimnis) angezeigt.
    ![BitBucket: Anwendungsliste][9]
9.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster am Azure Stack-Verwaltungsportal (https://adminportal.local.azurestack.external)) als Dienstadministrator an.
10.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus.
11. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
12. Kopieren Sie den **Schlüssel** und das **Geheimnis**, und fügen Sie sie in das Eingabefeld **Client-ID** bzw. das Eingabefeld **Clientgeheimnis** für BitBucket ein.
13. Klicken Sie auf **Speichern**.

## <a name="configure-onedrive"></a>Konfigurieren von OneDrive

Sie benötigen für diese Aufgabe ein mit einem OneDrive-Konto verknüpftes Microsoft-Konto.  Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

> [!NOTE]
> OneDrive for Business-Konten werden zurzeit nicht unterstützt.

1. Navigieren Sie zu https://apps.dev.microsoft.com/?referrer=https%3A%2F%2Fdev.onedrive.com%2Fapp-registration.htm, und melden Sie sich mit Ihrem Microsoft-Konto an.
2. Klicken Sie unter **Meine Anwendungen** auf **App hinzufügen**.
![OneDrive-Anwendungen][10]
3. Geben Sie für die neue Anwendungsregistrierung einen **Namen** wie **App Service in Azure Stack** ein, und klicken Sie dann auf **Anwendung erstellen**.
4. Der nächste Bildschirm enthält die Eigenschaften der neuen Anwendung. Speichern Sie die **Anwendungs-ID** in einen temporären Speicherort.
![OneDrive-Anwendungseigenschaften][11]
5. Klicken Sie unter **Anwendungsgeheimnisse** auf **Neues Kennwort generieren**. Notieren Sie sich das Kennwort unter **Neues Kennwort wurde generiert**. Dies ist Ihr Anwendungsgeheimnis, das nicht mehr wiederhergestellt werden kann, nachdem Sie auf **OK** geklickt haben.
6. Klicken Sie unter **Plattformen** auf **Plattform hinzufügen**, und wählen Sie dann **Web** aus.
7. Geben Sie den **Umleitungs-URI** ein. In einer Standardbereitstellung von Azure Stack weist der Umleitungs-URI das Format https://portal.local.azurestack.external/TokenAuthorize auf. Wenn die Ausführung in einer anderen Domäne erfolgt, ersetzen Sie „azurestack.local“ durch Ihren Domänennamen.
![OneDrive-Anwendung: Webplattform hinzufügen][12]
8. Fügen Sie **Microsoft Graph-Berechtigungen** - **Delegierte Berechtigungen** hinzu.
    - **Files.ReadWrite.AppFolder**
    - **User.Read**  
      ![OneDrive-Anwendung: Graph-Berechtigungen][13]
9. Klicken Sie auf **Speichern**.
10.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster am Azure Stack-Verwaltungsportal (https://adminportal.local.azurestack.external)) als Dienstadministrator an.
11.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus.
12. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
13. Kopieren Sie die **Anwendungs-ID** und das **Kennwort**, und fügen Sie sie in das Eingabefeld **Client-ID** bzw. das Eingabefeld **Clientgeheimnis** für OneDrive ein.
14. Klicken Sie auf **Speichern**.

## <a name="configure-dropbox"></a>Konfigurieren von Dropbox

> [!NOTE]
> Sie benötigen ein Dropbox-Konto, um diese Aufgabe abschließen zu können. Möglicherweise möchten Sie lieber ein Konto für Ihre Organisation anstelle eines persönlichen Kontos verwenden.

1. Navigieren Sie zu https://www.dropbox.com/developers/apps, und melden Sie sich mit den Anmeldeinformationen Ihres DropBox-Kontos an.
2. Klicken Sie auf **Create app**.

    ![Dropbox-Anwendungen][14]

3. Wählen Sie **Dropbox API** aus.
4. Legen Sie die Zugriffsebene auf **App Folder** (App-Ordner) fest.
5. Geben Sie einen **Namen** für Ihre Anwendung ein.
![Registrierung der Dropbox-Anwendung][15]
6. Klicken Sie auf **Create app** (App erstellen). Es wird eine Seite mit den Einstellungen für die App angezeigt, z.B. **App key** (App-Schlüssel) und **App secret** (App-Geheimnis).
7. Stellen Sie sicher, dass **App folder name** (Name des App-Ordners) auf **App Service in Azure Stack** festgelegt ist.
8. Legen Sie den **OAuth 2 Redirect URI** (OAuth 2-Umleitungs-URI) fest, und klicken Sie dann auf **Add** (Hinzufügen). In einer Standardbereitstellung von Azure Stack weist der Umleitungs-URI das Format https://portal.local.azurestack.external/TokenAuthorize auf. Wenn die Ausführung in einer anderen Domäne erfolgt, ersetzen Sie „azurestack.local“ durch Ihre Domäne.
![Konfiguration der Dropbox-Anwendung][16]
9.  Melden Sie sich im Browser auf einer neuen Registerkarte oder in einem neuen Fenster am Azure Stack-Verwaltungsportal (https://adminportal.local.azurestack.external)) als Dienstadministrator an.
10.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus.
11. Klicken Sie auf **Source control configuration** (Konfiguration der Quellcodeverwaltung).
12. Kopieren Sie die Werte von **Application Key** (Anwendungsschlüssel) und von **App secret** (App-Geheimnis), und fügen Sie sie in das Eingabefeld **Client-ID** bzw. das Eingabefeld **Clientgeheimnis** für Dropbox ein.
13. Klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

Benutzer können jetzt die Bereitstellungsquellen unter anderem für [Continuous Deployment](https://docs.microsoft.com/azure/app-service/deploy-continuous-deployment), [lokale Git-Bereitstellungen](https://docs.microsoft.com/azure/app-service/deploy-local-git) und die [Synchronisierung von Cloudordnern](https://docs.microsoft.com/azure/app-service/deploy-content-sync) verwenden.

<!--Image references-->
[1]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin.png
[2]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-source-control-configuration.png
[3]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-developer-applications.png
[4]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-populated.png
[5]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-github-register-a-new-oauth-application-complete.png
[6]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-roles-management-server-repair-all.png
[7]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-dashboard.png
[8]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer.png
[9]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-bitbucket-access-management-add-oauth-consumer-complete.png
[10]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-applications.png
[11]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-registration.png
[12]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-platform.png
[13]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Onedrive-application-graph-permissions.png
[14]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-applications.png
[15]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-registration.png
[16]: ./media/azure-stack-app-service-configure-deployment-sources/App-service-provider-admin-Dropbox-application-configuration.png
