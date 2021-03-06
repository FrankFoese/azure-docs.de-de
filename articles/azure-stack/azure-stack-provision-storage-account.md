---
title: Speicherkonten in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Azure Stack-Speicherkonto erstellen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 1/18/2019
ms.author: mabrigg
ms.lastreviewed: 1/18/2019
ms.openlocfilehash: 692fba97a67a7dc9654e307c2b7628dcc89f3ba8
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768673"
---
# <a name="storage-accounts-in-azure-stack"></a>Speicherkonten in Azure Stack
Speicherkonten enthalten Blob-und Tabellenspeicherdienste und den eindeutigen Namespace für Ihre Speicherdatenobjekte. Standardmäßig sind die Daten in Ihrem Konto nur für Sie als Speicherkontobesitzer verfügbar.

1. Melden Sie sich auf dem Azure Stack-PoC-Computer unter `https://adminportal.local.azurestack.external` als [Administrator](azure-stack-connect-azure-stack.md) an, und klicken Sie auf **+ Eine Ressource erstellen** > **Daten und Speicher** > **Speicherkonto**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Namen für das Speicherkonto ein. Erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, und klicken Sie auf **Erstellen**, um das Speicherkonto zu erstellen.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Klicken sie zum Anzeigen des neuen Speicherkontos auf **Alle Ressourcen**, suchen Sie dann nach dem Speicherkonto, und klicken Sie auf seinen Namen.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Nächste Schritte
[Verwenden von Azure-Ressourcen-Manager-Vorlagen](user/azure-stack-arm-templates.md)

[Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md)

[Herunterladen des Azure Stack-Überprüfungshandbuchs für mit Azure konsistentem Speicher](https://aka.ms/azurestacktp1doc)
