---
title: 'Tutorial: Azure Active Directory-Integration mit ScreenSteps | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ScreenSteps konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07abc79979bcb557b6513ff16472ff5c95d0e12f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56184515"
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Tutorial: Azure Active Directory-Integration mit ScreenSteps

In diesem Tutorial erfahren Sie, wie Sie ScreenSteps in Azure Active Directory (Azure AD) integrieren.

Die Integration von ScreenSteps in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ScreenSteps hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ScreenSteps anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit ScreenSteps konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ScreenSteps-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von ScreenSteps aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-screensteps-from-the-gallery"></a>Hinzufügen von ScreenSteps aus dem Katalog
Zum Konfigurieren der Integration von ScreenSteps in Azure AD müssen Sie ScreenSteps über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um ScreenSteps aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld **ScreenSteps** ein, wählen Sie im Ergebnisbereich **ScreenSteps** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![ScreenSteps in der Ergebnisliste](./media/screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ScreenSteps mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ScreenSteps als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ScreenSteps muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in ScreenSteps den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei ScreenSteps müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. **[Erstellen eines ScreenSteps-Testbenutzers](#create-a-screensteps-test-user)**, um eine Entsprechung von Britta Simon in ScreenSteps zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ScreenSteps-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei ScreenSteps die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ScreenSteps** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/screensteps-tutorial/tutorial_screensteps_samlbase.png)

1. Führen Sie im Abschnitt **Domäne und URLs für ScreenSteps** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für ScreenSteps](./media/screensteps-tutorial/tutorial_screensteps_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<tenantname>.ScreenSteps.com`.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie den Wert durch die tatsächliche Anmelde-URL. Dies wird später in diesem Tutorial beschrieben. 

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/screensteps-tutorial/tutorial_screensteps_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“ beim Konfigurieren des einmaligen Anmeldens](./media/screensteps-tutorial/tutorial_general_400.png)

1. Klicken Sie im Abschnitt **ScreenSteps-Konfiguration** auf **ScreenSteps konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![ScreenSteps-Konfiguration](./media/screensteps-tutorial/tutorial_screensteps_configure.png) 

1. Melden Sie sich in einem anderen Webbrowserfenster bei der ScreenSteps-Unternehmenswebsite als Administrator an.

1. Klicken Sie auf **Kontoeinstellungen**.

    ![Kontoverwaltung](./media/screensteps-tutorial/ic778523.png "Kontoverwaltung")

1. Klicken Sie auf **Einmaliges Anmelden**.

    ![Remoteauthentifizierung](./media/screensteps-tutorial/ic778524.png "Remoteauthentifizierung")

1. Klicken Sie auf **Create Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden erstellen).

    ![Remoteauthentifizierung](./media/screensteps-tutorial/ic778525.png "Remoteauthentifizierung")

1. Führen Sie im Abschnitt **Create Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden erstellen) die folgenden Schritte aus:

    ![Erstellen eines Authentifizierungsendpunkts](./media/screensteps-tutorial/ic778526.png "Erstellen eines Authentifizierungsendpunkts")
    
    a. Geben Sie in das Textfeld **Titel** einen Titel ein.
    
    b. Wählen Sie in der Liste **Modus** die Option **SAML** aus.
    
    c. Klicken Sie auf **Create**.

1. **Bearbeiten** Sie den neuen Endpunkt.

    ![Endpunkt bearbeiten](./media/screensteps-tutorial/ic778528.png "Endpunkt bearbeiten")

1. Führen Sie im Abschnitt **Edit Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden bearbeiten) die folgenden Schritte aus:

    ![Remote Authentication Endpoint](./media/screensteps-tutorial/ic778527.png "Remote Authentication Endpoint")

    a. Klicken Sie auf **Upload new SAML Certificate file** (Neue SAML-Zertifikatsdatei hochladen), und laden Sie dann das Zertifikat hoch, das Sie aus dem Azure-Portal heruntergeladen haben.
    
    b. Fügen Sie den Wert der **URL für den SAML-SSO-Dienst**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Remote Login URL** (Remoteanmelde-URL) ein.
    
    c. Fügen Sie den Wert der **Abmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Log out URL** (Abmelde-URL) ein.
    
    d. Wählen Sie eine **Gruppe** zum Zuweisen von Benutzern bei deren Bereitstellung aus.
    
    e. Klicken Sie auf **Aktualisieren**.

    f. Kopieren Sie die **SAML-Consumer-URL** in die Zwischenablage, und fügen Sie sie in das Textfeld **Anmelde-URL** im Abschnitt **Domänen und URLs für ScreenSteps** ein.
    
    g. Wechseln Sie zurück zu **Edit Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden bearbeiten).
    
    h. Klicken Sie auf die Schaltfläche **Make default for account** (Als Standard für Konto festlegen), um diesen Endpunkt für alle Benutzer zu verwenden, die sich bei ScreenSteps anmelden. Alternativ können Sie auf die Schaltfläche **Add to Site** (Zu Standort hinzufügen) klicken, um diesen Endpunkt für bestimmte Websites in **ScreenSteps** zu verwenden.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Dokumentation zu eingebettetem Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/screensteps-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/screensteps-tutorial/create_aaduser_02.png)

1. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/screensteps-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/screensteps-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-a-screensteps-test-user"></a>Erstellen eines ScreenSteps-Testbenutzers

In diesem Abschnitt erstellen Sie in ScreenSteps einen Benutzer mit dem Namen Britta Simon. Arbeiten Sie mit dem  [Clientsupportteam von ScreenSteps](https://www.screensteps.com/contact) zusammen, um die Benutzer in der ScreenSteps-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ScreenSteps gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon ScreenSteps zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **ScreenSteps** aus.

    ![ScreenSteps-Link in der Anwendungsliste](./media/screensteps-tutorial/tutorial_screensteps_app.png)  

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ScreenSteps“ klicken, sollten Sie automatisch bei Ihrer ScreenSteps-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/screensteps-tutorial/tutorial_general_01.png
[2]: ./media/screensteps-tutorial/tutorial_general_02.png
[3]: ./media/screensteps-tutorial/tutorial_general_03.png
[4]: ./media/screensteps-tutorial/tutorial_general_04.png

[100]: ./media/screensteps-tutorial/tutorial_general_100.png

[200]: ./media/screensteps-tutorial/tutorial_general_200.png
[201]: ./media/screensteps-tutorial/tutorial_general_201.png
[202]: ./media/screensteps-tutorial/tutorial_general_202.png
[203]: ./media/screensteps-tutorial/tutorial_general_203.png

