---
title: Grundlegendes zur Nutzung von Azure-Reservierungen für Enterprise | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie die Azure-Reservierung für den Konzernbeitritt angewendet wird.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: banders
ms.openlocfilehash: 0f29544890fe10f4914de393a4b153cfe393a2ec
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54904300"
---
# <a name="understand-azure-reservation-usage-for-your-enterprise-enrollment"></a>Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt

Werten Sie Ihre Nutzung von Reservierungen aus, indem Sie die **ReservationId** von der [Reservierungsseite](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) und die Nutzungsdatei aus dem [EA-Portal](https://ea.azure.com) verwenden. Darüber hinaus können Sie die Reservierungsnutzung im [EA-Portal](https://ea.azure.com) im Abschnitt mit der Nutzungsübersicht anzeigen.

Wenn Sie die Reservierung in einem Abrechnungskontext mit nutzungsbasierter Bezahlung erworben haben, lesen Sie die Informationen unter [Grundlegendes zur Nutzung von Azure-Reservierungen für Ihr Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md).

## <a name="usage-for-reserved-virtual-machines-instances"></a>Nutzung von reservierten VM-Instanzen

In den folgenden Abschnitten wird davon ausgegangen, dass Sie eine Windows-VM des Typs „Standard_D1_v2“ in der Region „USA, Osten“ ausführen und Ihre Reservierungsinformationen mit den Angaben in der folgenden Tabelle übereinstimmen:

| Feld | Wert |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Menge |1|
|SKU | Standard_D1|
|Region | eastus |

Der Hardwarebestandteil des virtuellen Computers ist abgedeckt, da der bereitgestellte virtuelle Computer den Reservierungsattributen entspricht. Wenn Sie ermitteln möchten, welche Windows-Software nicht von der Reservierung abgedeckt wird, lesen Sie [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md).

### <a name="usage-in-csv-file-for-reserved-vm-instances"></a>Nutzung in der CSV-Datei für reservierte VM-Instanzen

Sie können die CSV-Enterprise-Nutzungsdatei aus dem Enterprise Portal herunterladen. Filtern Sie in der CSV-Datei nach **Zusätzliche Informationen**, und geben Sie Ihre **ReservationID** ein. Der folgende Screenshot zeigt die Felder, die im Zusammenhang mit der Reservierung stehen.

![Enterprise Agreement (EA) als CSV-Datei für Azure-Reservierungen](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. Die **ReservationId** im Feld **Zusätzliche Informationen** stellt die Reservierung dar, die auf die VM angewendet wurde.
2. **ConsumptionMeter** ist die Verbrauchseinheit-ID für die VM.
3. Die **Verbrauchseinheit-ID** ist die Reservierungsverbrauchseinheit, deren Kosten sich auf 0,00 USD belaufen. Die Kosten für die ausgeführte VM werden mit der reservierten VM-Instanz bezahlt.
4. „Standard_D1“ ist eine VM mit einer vCPU, und die VM wird ohne Azure-Hybridvorteil bereitgestellt. Diese Verbrauchseinheit deckt somit die zusätzlichen Kosten der Windows-Software ab. Die Verbrauchseinheit, die einer VM der D-Serie mit einem Kern entspricht, finden Sie unter [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md).  Wenn Sie Anspruch auf den Azure-Hybridvorteil haben, werden diese zusätzlichen Kosten nicht angewendet.

## <a name="usage-for-sql-database--cosmos-db-reserved-capacity-reservations"></a>Nutzung von Reservierungen von SQL-Datenbank- und Cosmos DB-Kapazitäten

In den folgenden Abschnitten wird der Nutzungsbericht anhand von Azure SQL-Datenbank beschrieben. Mit denselben Schritten können Sie auch die Nutzung für Azure Cosmos DB abrufen. 

Es wird davon ausgegangen, dass Sie eine SQL-Datenbank-Gen 4-Instanz in der Region „USA, Osten“ ausführen und Ihre Reservierungsinformationen mit den Angaben in der folgenden Tabelle übereinstimmen:

| Feld | Wert |
|---| --- |
|ReservationId |8244e673-83e9-45ad-b54b-3f5295d37cae|
|Menge |2|
|Produkt| SQL-Datenbank Gen 4 (2 Kerne)|
|Region | eastus |

### <a name="usage-in-csv-file"></a>Nutzung in einer CSV-Datei 

Filtern Sie nach **Zusätzliche Informationen**, und geben Sie Ihre **Reservierungs-ID** ein. Wählen Sie dann die entsprechende **Kategorie für Messung** aus: Azure SQL-Datenbank oder Azure Cosmos DB. Der folgende Screenshot zeigt die Felder, die im Zusammenhang mit der Reservierung stehen.

![Enterprise Agreement (EA) als CSV-Datei für reservierte SQL-Datenbank-Kapazitäten](./media/billing-understand-reserved-instance-usage-ea/billing-ea-sql-db-reserved-capacity-csv.png)

1. Die **ReservationId** im Feld **Zusätzliche Informationen** ist die Reservierung, die auf die SQL-Datenbank-Ressource angewendet wurde.
2. **ConsumptionMeter** ist die Verbrauchseinheit-ID für die SQL-Datenbank-Ressource.
3. Die **Verbrauchseinheit-ID** ist die Reservierungsverbrauchseinheit, für die keine Kosten anfallen. Bei SQL-Datenbank-Ressourcen, die für die Reservierung qualifiziert sind, wird diese Verbrauchseinheit-ID in der CSV-Datei angezeigt.

## <a name="usage-summary-page-in-enterprise-portal"></a>Seite mit Übersicht über die Nutzung im Enterprise Portal

Ihre Nutzung von Azure-Reservierungen wird darüber hinaus im Enterprise Portal im Abschnitt zur Nutzungsübersicht angezeigt: ![EA-Nutzungszusammenfassung (Enterprise Agreement)](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Die Hardwarekomponenten der VM werden nicht in Rechnung gestellt, da sie von der Reservierung abgedeckt sind. Bei einer SQL-Datenbank-Reservierung wird eine Zeile mit **Dienstnamen** als Nutzung von reservierten Azure SQL-Datenbank-Kapazitäten angezeigt.
2. In diesem Beispiel verfügen Sie nicht über den Azure-Hybridvorteil, weshalb Ihnen die Kosten für die Windows-Software, die mit der VM verwendet wurde, in Rechnung gestellt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

- [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
- [Vorauszahlen für virtuelle Computer mit Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vorauszahlen von SQL-Datenbank-Computeressourcen mit reservierter Azure SQL-Datenbank-Kapazität](../sql-database/sql-database-reserved-capacity.md) 
- [Verwalten von Azure-Reservierungen](billing-manage-reserved-vm-instance.md)
- [Grundlegendes zur Anwendung des Rabatts für Azure-Reservierungen auf virtuelle Computer](billing-understand-vm-reservation-charges.md)
- [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](billing-understand-reserved-instance-usage.md)
- [Nicht in Azure-Reservierungen enthaltene Windows-Softwarekosten](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

