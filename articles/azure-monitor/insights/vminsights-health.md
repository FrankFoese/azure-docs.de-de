---
title: Überwachen der Integrität von virtuellen Computern mit Azure Monitor für VMs (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Integrität der VM und des zugrundeliegenden Betriebssystems mithilfe von Azure Monitor for VMs beurteilen können.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2019
ms.author: magoedte
ms.openlocfilehash: 67217b63588946782d42b4287cf5f24e29ebe5bd
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55961260"
---
# <a name="understand-the-health-of-your-azure-virtual-machines-with-azure-monitor-for-vms-preview"></a>Überwachen der Integrität Ihrer virtuellen Azure-Computer mit Azure Monitor für VMs (Vorschauversion)
Azure beinhaltet mehrere Dienste, die einzeln eine bestimmte Rolle oder Aufgabe im Überwachungsbereich ausführen, aber ein tiefgreifender Blick auf die Integrität des auf Azure-VMs gehosteten Betriebssystems war bisher nicht verfügbar.  Zwar konnten Sie mithilfe von Log Analytics oder Azure Monitor verschiedene Bedingungen überwachen, diese waren aber nicht dafür ausgelegt, die Integrität von Kernkomponenten oder die Gesamtintegrität der VM zu modellieren und darzustellen.  Das Integritätsfeature von Azure Monitor for VMs überwacht proaktiv die Verfügbarkeit und Leistung des Windows- oder Linux-Gastbetriebssystems mit einem Modell, das Schlüsselkomponenten und ihre Beziehungen modelliert, und mit Kriterien, die angeben, wie die Integrität dieser Komponenten gemessen wird, und eine Warnung an Sie auslösen, wenn ein Zustand eingeschränkter Integrität erkannt wird.  

Die Anzeige des Integritätsgesamtstatus von Azure-VMs und des zugrundeliegenden Betriebssystems mit dem Azure Monitor for VMs kann aus zwei verschiedenen Perspektiven erfolgen, einmal direkt in einer VM oder übergreifend über alle in einer Ressourcengruppe enthaltenen VMs mithilfe von Azure Monitor.

Dieser Artikel soll Ihr Verständnis dafür schärfen, wie Sie erkannte Integritätsprobleme schnell bewerten, untersuchen und beheben.

Informationen zum Konfigurieren von Azure Monitor for VMs finden Sie unter [Enable Azure Monitor for VMs](vminsights-onboard.md) (Aktivieren von Azure Monitor for VMs).

>[!NOTE]
>Ab 11. Februar 2019 beginnen wir mit der Migration des aktuellen Integritätsmodells in der Integritätsfunktion „Azure Monitor für VMs“, das derzeit in der Integritätsdiagnose angezeigt wird, zu einer neuen Version des Integritätsmodells. Diese Aktualisierung verbessert die Leistung der Integritätsrollupverarbeitung und umfasst ein optimiertes Integritätsmodell, das in der Ansicht der Integritätsdiagnose angezeigt wird. 
>
>Mit dem neuen Integritätsmodell wird der Rollup von untergeordneten Integritätskriterien zu Integritätskriterien auf übergeordneter oder Entitätsebene schneller ausgeführt und so der Integritätszustand auf übergeordneter Ebene mit geringerer Latenz auf den gewünschten oder Zielzustand aktualisiert. Im Unterschied zur bisherigen, auf Registerkarten basierenden Methode zum Auswählen einer Kategorie in der Ansicht können Sie die Integritätskriterien unter den Kategorien **Leistung** und **Verfügbarkeit** filtern.
>
>Weitere Informationen zur Benutzeroberfläche der Integritätsdiagnose finden Sie im Abschnitt zur Integritätsdiagnose[in diesem Artikel](#health-diagnostics). 
>
>Diese Aktualisierung bringt folgende Verbesserungen mit sich: 
>
>- Integritätsrollupverarbeitung mit geringerer Latenz  
>- Schnellere Warnungen zu Änderungen des Integritätszustands 
>- Schnellere Aktualisierung des Integritätszustands in der aggregierten Ansicht eines virtuellen Computers für alle virtuellen Computer 
>
>Es gibt keine Regression der Funktionen, die aktuell mit der Integritätsfunktion von Azure Monitor für VMs bereitgestellt werden.

>Als Folge dieser Änderung sind zwei Benutzeroberflächen in der Integritätsdiagnose betroffen: Der Zustandsänderungsverlauf wird zurückgesetzt, und die vorherigen Zustandsänderungen für Integritätskriterien stehen in der Spalte „Zustandsänderung“ der Seite „Integritätsdiagnose“ nicht zur Überprüfung zur Verfügung. Wenn Sie die Verlaufsdaten eines unternehmenskritischen virtuellen Computers benötigen, können Sie zu Referenzzwecken einen Screenshot der Integritätskriteriendaten und der entsprechenden Zustandsänderungen erstellen. 

## <a name="monitoring-configuration-details"></a>Details der Überwachungskonfiguration
Dieser Abschnitt beschreibt im Umriss die definierten standardmäßigen Integritätskriterien für die Überwachung von Azure Windows- und Linux-VMs. Alle Integritätskriterien sind so vorkonfiguriert, dass sie beim Eintreten der Fehlerbedingung eine Warnung ausgeben. 

### <a name="windows-vms"></a>Virtuelle Windows-Computer

- Verfügbare Megabyte Arbeitsspeicher 
- Datenträger – mittlere Dauer pro Schreibvorgang (logischer Datenträger)
- Datenträger – mittlere Dauer pro Schreibvorgang (Datenträger)
- Logischer Datenträger – mittlere Dauer pro Lesevorgang
- Logischer Datenträger – mittlere Dauer pro Übertragung
- Datenträger – mittlere Dauer pro Lesevorgang
- Datenträger – mittlere Dauer pro Übertragung
- Aktuelle Länge der Datenträgerwarteschlange (Logischer Datenträger)
- Aktuelle Länge der Datenträgerwarteschlange (Datenträger)
- Datenträger – Leerlaufzeit (%)
- Fehler oder Beschädigung im Dateisystem
- Logischer Datenträger – wenig Speicherplatz (%)
- Logischer Datenträger – wenig Speicherplatz (MB)
- Logischer Datenträger – Leerlaufzeit (%)
- Speicherseiten pro Sekunde
- Genutzte Bandbreite (%) beim Lesen
- Genutzte Bandbreite (%) gesamt
- Genutzte Bandbreite (%) beim Schreiben
- Verwendeter Prozentsatz des zugesicherten Speichers
- Physischer Datenträger – Leerlaufzeit (%)
- Integrität des DHCP-Clientdiensts
- Integrität des DNS-Clientdiensts
- Integrität des RPC-Diensts
- Integrität des Serverdiensts
- CPU-Auslastung (%) gesamt
- Integrität des Windows-Ereignisprotokolldiensts
- Integrität des Windows-Firewalldiensts
- Integrität des Windows-Remoteverwaltungsdiensts

### <a name="linux-vms"></a>Virtuelle Linux-Computer
- Datenträger Durchschn. Datenträger s/übertragen 
- Datenträger Durchschn. Datenträger s/gelesen 
- Datenträger Durchschn. Datenträger s/geschrieben 
- Datenträgerintegrität
- Logischer Datenträger – verfügbarer Speicherplatz
- Logischer Datenträger – verfügbarer Speicherplatz (%)
- Logischer Datenträger – freie I-Knoten (%)
- Netzwerkadapterintegrität
- Prozessor – DPC-Zeit (%)
- Prozessor – Prozessorzeit (%)
- Prozessor – Prozessorzeit gesamt
- Prozessor – DPC-Zeit gesamt
- Betriebssystem – verfügbare Megabyte Arbeitsspeicher

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

## <a name="introduction-to-health-experience"></a>Einführung in die Integritätsoberfläche
Bevor wir in die Verwendung des Integritätsfeatures für eine einzelne VM oder eine Gruppe von VMs einsteigen, müssen wir Ihnen eine kurze Einführung geben, damit Sie verstehen, wie die Informationen dargestellt werden und wofür die Visualisierungen stehen.  

## <a name="view-health-directly-from-a-virtual-machine"></a>Direktes Anzeigen der Integrität in einer VM 
Um die Integrität einer Azure-VM anzuzeigen, wählen Sie im linken Bereich der VM **Insights (Vorschau)** aus. Auf der Insights-Seite der VM ist standardmäßig **Integrität** geöffnet und zeigt die Integritätsansicht der VM an.  

![Azure Monitor for VMs-Integritätsübersicht einer ausgewählten Azure-VM](./media/vminsights-health/vminsights-directvm-health.png)

Auf der Registerkarte **Integrität** zeigt die Tabelle unter dem Abschnitt **Integrität der Gast-VM** den aktuellen Integritätsstatus Ihrer VM und die Gesamtzahl der VM-Integritätswarnungen an, die von einer Komponente mit eingeschränkter Integrität ausgelöst wurden. Weitere Informationen zur Benachrichtigungsumgebung finden Sie im Abschnitt „Warnungen“.  

Die für eine VM definierten Integritätszustände sind in der folgenden Tabelle beschrieben: 

|Symbol |Integritätsstatus |Bedeutung |
|-----|-------------|------------|
| |Healthy |Der Integritätszustand lautet „Fehlerfrei“, wenn er innerhalb des Bereichs der definierten Integritätsbedingungen liegt, für den virtuellen Computer keine Probleme erkannt werden und dieser wie erwartet ausgeführt wird. Im Fall eines übergeordneten Rollupmonitors wird ein Rollup der Integrität durchgeführt, die dann den Best-Case- oder Worst-Case-Status des untergeordneten Elements wiedergibt.|
| |Kritisch |Der Integritätszustand lautet „Kritisch“, wenn er nicht innerhalb des Bereichs der definierten Integritätsbedingung liegt und ein oder mehrere kritische Probleme erkannt wurden, die behoben werden müssen, um die normale Funktionsweise wiederherzustellen. Im Fall eines übergeordneten Rollupmonitors wird ein Rollup der Integrität durchgeführt, die dann den Best-Case- oder Worst-Case-Status des untergeordneten Elements wiedergibt.|
| |Warnung |Der Integritätszustand lautet „Warnung“, wenn er zwischen zwei Schwellenwerten für die definierte Integritätsbedingung liegt, wobei ein Wert den Zustand *Warnung* und der andere Wert den Zustand *kritisch* angibt (drei Schwellenwerte für den Integritätszustand können konfiguriert werden), oder wenn ein nicht kritisches Problem erkannt wird, das zu kritischen Problemen führen kann, wenn es nicht behoben wird. Im Fall eines übergeordneten Rollupmonitors erhält das übergeordnete Element den Zustand *Warnung*, wenn mindestens eines der untergeordneten Elemente den Zustand „Warnung“ aufweist. Wenn ein untergeordnetes Element vorhanden ist, das den Status *Kritisch* und zugleich ein weiteres untergeordnetes Element den Status *Warnung* aufweist, weist das übergeordnete Rollup den Integritätsstatus *Kritisch* aus.|
| |Unknown |Der Integritätszustand lautet *Unbekannt*, wenn er aus verschiedenen Gründen nicht berechnet werden kann, etwa wegen fehlender Möglichkeit zum Erfassen von Daten, fehlender Dienstinitialisierung usw. Dieser Integritätszustand ist nicht konfigurierbar.| 

Durch Auswählen von **Integritätsdiagnose anzeigen** wird eine Seite geöffnet, auf der alle Komponenten der VM mit den ihnen zugeordneten Integritätskriterien, Statusänderungen und sonstigen wichtigen Problemen dargestellt sind, die durch Überwachung der mit der VM zusammenhängenden Komponenten gefunden wurden. Weitere Informationen finden Sie unter [Integritätsdiagnose](#health-diagnostics). 

Die Tabelle unter dem Abschnitt **Integrität der Komponente** zeigt einen Integritätsrollupstatus der wichtigsten Leistungskategorien, die anhand von Integritätskriterien für die betreffenden Bereiche überwacht werden, im Einzelnen **CPU**, **Arbeitsspeicher**, **Datenträger** und **Netzwerk**.  Durch Auswählen einer der Komponenten wird eine Seite geöffnet, auf der alle Integritätsaspekte für die einzelnen Integritätskriterien der betreffenden Komponente mit dem jeweiligen Integritätsstatus aufgelistet sind.  

Beim Zugriff auf die Integrität aus einer Azure-VM, die das Betriebssystem Windows ausführt, wird der Integritätsstatus der fünf wichtigsten Windows-Kerndienste unter **Integrität der Kerndienste** angezeigt.  Durch Auswählen eines der Dienste wird eine Seite geöffnet, auf der die Integritätskriterien zur Überwachung der betreffenden Komponente und ihr Integritätsstatus aufgelistet werden.  Ein Klick auf den Namen eines Integritätskriteriums öffnet dessen Eigenschaftenseite, auf der Sie die Konfigurationsdetails überprüfen können, darunter auch, ob für das Integritätskriterium eine entsprechende Azure Monitor-Warnung definiert ist. Weitere Informationen finden Sie unter [Integritätsdiagnose](#health-diagnostics) im Abschnitt zur Verwendung von Integritätskriterien.  

## <a name="aggregate-virtual-machine-perspective"></a>Aggregieren der VM-Perspektive
Um die Integritätssammlung aller Ihrer virtuellen Computer in einer Ressourcengruppe anzuzeigen, wählen Sie in der Navigationsliste im Portal **Azure Monitor** und dann **Virtual Machines (Vorschau)** aus.  

![VM Insights-Überwachungsansicht in Azure Monitor](./media/vminsights-health/vminsights-aggregate-health.png)

Wählen Sie in den Dropdownlisten **Abonnement** und **Ressourcengruppe** die passende Ressourcengruppe aus, die die zur Gruppe gehörenden VMs enthält, um ihren gemeldeten Integritätszustand anzuzeigen.  Ihre Auswahl gilt nur für das Integritätsfeature und nicht für die Leistung oder die Karte.

Die Registerkarte **Integrität** stellt die folgenden Informationen dar:

* Wie viele VMs sich in einem kritischen oder fehlerhaften Zustand im Vergleich zur Anzahl der VMs in fehlerfreiem Zustand oder zu denjenigen befinden, die keine Daten übermitteln (was als unbekannter Zustand bezeichnet wird)
* Welche und wie viele VMs – nach Betriebssystem dargestellt – einen fehlerhaften Zustand melden
* Wie viele VMs aufgrund eines bei einem Prozessor, einem Datenträger, beim Arbeitsspeicher oder bei einem Netzwerkadapter erkannten Problems fehlerhaft sind, nach Integritätsstatus kategorisiert  
* Wie viele VMs aufgrund eines bei einem Kerndienst des Betriebssystems erkannten Problems fehlerhaft sind, nach Integritätsstatus kategorisiert

Hier können Sie die kritischsten der erkannten Probleme anhand der Integritätskriterien, nach denen die VMs proaktiv überwacht werden, schnell identifizieren und die Details der VM-Integritätswarnungen zusammen mit zugeordneten Wissensartikeln durcharbeiten, die Sie bei der Diagnose und Korrektur der Probleme unterstützen.  Klicken Sie auf einen der Schweregrade, um die Seite [Alle Warnungen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) gefiltert nach diesem Schweregrad zu öffnen.

Die Liste **VM-Verteilung nach Betriebssystem** zeigt die VMs nach Windows-Edition oder Linux-Distribution zusammen mit den jeweiligen Versionen an. In jeder Betriebssystemkategorie sind die VMs basierend auf der Integrität weiter aufgeschlüsselt. 

![Distributionsperspektive von virtuellen Maschinen in VM Insights](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

Sie können auf ein beliebiges Spaltenelement klicken – **VM-Anzahl**, **Kritisch**, **Warnung**, **Fehlerfrei** oder **Unbekannt** –, um auf der Seite **Virtual Machines** eine Liste der gefilterten Ergebnisse passend zur ausgewählten Spalte anzuzeigen. Wenn Sie beispielsweise alle VMs überprüfen möchten, die **Red Hat Enterprise Linux, Version 7.5** ausführen, klicken Sie auf den Wert von **VM-Anzahl** für dieses Betriebssystem. Dann wird die folgende Seite geöffnet, auf der die VMs, die diesem Filterkriterium entsprechen, mit ihrem aktuell bekannten Integritätszustand aufgelistet sind.  

![Beispielrollup von Red Hat Linux-VMs](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)
 
Wenn Sie auf der Seite **Virtual Machines** in der Spalte **VM-Name** den Namen einer VM auswählen, werden Sie zur Seite der VM-Instanz geführt, die weitere Details zu den Warnungen und erkannten Problemen bei Integritätskriterien enthält, die die ausgewählte VM betreffen.  Hier können Sie die Details zum Integritätsstatus filtern, indem Sie auf das Symbol **Integritätsstatus** in der oberen linken Ecke der Seite klicken, um zu sehen, welche Komponenten fehlerhaft sind. Alternativ können Sie die Warnungen zur VM-Integrität anzeigen, die von einer fehlerhaften Komponente ausgelöst wurden, kategorisiert nach Schweregrad der Warnung.    

In der VM-Listenansicht wird durch Klicken auf den Namen einer VM die Seite **Integrität** der ausgewählten VM geöffnet, auf ähnliche Weise wie durch direktes Auswählen von **Insights (Vorschau)** aus der VM.

![VM Insights einer ausgewählten Azure Virtual Machine](./media/vminsights-health/vminsights-directvm-health.png)

Hier ist ein Rollup des **Integritätsstatus** für die VM mit **Warnungen** nach Schweregrad kategorisiert dargestellt, die die Warnungen zur VM-Integrität darstellen, die beim Wechsel des Integritätsstatus von fehlerfrei zu fehlerhaft für ein bestimmtes Integritätskriterium ausgelöst werden.  Durch Auswählen von **VMs in kritischem Zustand** wird eine Seite mit einer Liste einer oder mehrerer VMs geöffnet, die sich in einem kritischen Integritätsstatus befinden.  Durch Klicken auf den Integritätsstatus für eine der VMs in der Liste wird die Ansicht **Integritätsdiagnose** der betreffenden VM angezeigt.  Hier können Sie herausfinden, welche Integritätskriterien ein Problem mit dem Integritätsstatus widerspiegeln. Wenn die Seite **Integritätsdiagnose** geöffnet wird, zeigt sie alle Komponenten der VM und die ihnen zugeordneten Integritätskriterien mit dem aktuellen Integritätsstatus an.  Im Abschnitt [Integritätsdiagnose](#health-diagnostics) finden Sie weitere Details.  

Wenn Sie **Alle Integritätskriterien anzeigen** auswählen, wird eine Seite geöffnet, die eine Liste aller für dieses Feature verfügbaren Integritätskriterien anzeigt.  Die Informationen können auf der Grundlage der folgenden Optionen weiter gefiltert werden:

* **Typ** – Es gibt drei Typen von Integritätskriterien für die Bewertung von Zuständen und für den Rollup des Gesamtintegritätsstatus der überwachten VM.  
    a. **Einheit** – misst einen Aspekt des virtuellen Computers. Dieser Typ von Integritätskriterium kann beispielsweise einen Leistungsindikator überprüfen, um die Leistung der Komponente zu bestimmen, ein Skript auszuführen, um eine synthetische Transaktion durchzuführen oder nach einem Ereignis Ausschau zu halten, das auf einen Fehler hinweist.  Standardmäßig ist der Filter auf „Einheit“ festgelegt.  
    b. **Abhängigkeit** – Stellt einen Integritätsrollup zwischen verschiedenen Entitäten bereit. Durch diese Integritätskriterien ist es möglich, dass die Integrität einer Entität von der Integrität einer anderen Art von Entität abhängt, auf die sie für erfolgreichen Betrieb angewiesen ist.  
    c. **Aggregat** – stellt einen kombinierten Integritätsstatus ähnlicher Integritätskriterien bereit. Die Integritätskriterien Einheit und Abhängigkeit sind normalerweise unter einem zusammengefassten Integritätskriterium konfiguriert. Nicht nur wird eine bessere allgemeine Organisation der vielen verschiedenen für eine Entität angezielten Integritätskriterien ermöglicht, ein zusammengefasstes Integritätskriterium stellt auch einen eindeutigen Integritätsstatus für verschiedene Kategorien der Entitäten zur Verfügung.

* **Kategorie** – Typ von Integritätskriterien, der zum Gruppieren von Kriterien ähnlichen Typs für die Berichterstellung verwendet wird.  Dabei handelt es sich entweder um **Verfügbarkeit** oder um **Leistung**.

Sie können weiter ins Detail einsteigen, um zu sehen, welche Instanzen fehlerhaft sind, indem Sie auf einen Wert in der Spalte **Fehlerhafte Komponente** klicken.  Auf der Seite sind die Komponenten, die sich in einem kritischen Integritätsstatus befinden, in einer Tabelle aufgelistet.    

## <a name="health-diagnostics"></a>Integritätsdiagnose
Auf der Seite **Integritätsdiagnose** können Sie das Integritätsmodell eines virtuellen Computers anzeigen, in dem alle Komponenten des virtuellen Computers, die zugeordneten Integritätskriterien, Zustandsänderungen und weitere wichtige Probleme aufgeführt sind, die bei der Überwachung der mit dem virtuellen Computer verbundenen Komponenten ermittelt wurden.

![Beispiel der Seite „Integritätsdiagnose“ für eine VM](./media/vminsights-health/health-diagnostics-page-01.png)

Sie können die Integritätsdiagnose auf eine der folgenden Weisen starten.

* Nach dem Rollupintegritätsstatus für alle VMs aus der VM-Aggregatperspektive in Azure Monitor.  Klicken Sie auf der Seite **Integrität** im Abschnitt **Integrität der Gast-VM** auf das Symbol für den Integritätsstatus **Kritisch**, **Warnung**, **Fehlerfrei** oder **Unbekannt**, und führen Sie einen Drilldown zu der Seite aus, auf der alle VMs aufgelistet sind, die der Filterkategorie entsprechen.  Durch Klicken auf den Wert in der Spalte **Integritätsstatus** wird die Integritätsdiagnose mit dem Bereich der betreffenden VM geöffnet.      

* Nach dem Betriebssystem aus der VM-Aggregatperspektive in Azure Monitor. Unter **VM-Distribution** bewirkt das Auswählen eines der Spaltenwerte das Öffnen der Seite **Virtual Machines** und die Rückgabe einer Liste in der Tabelle, die der Filterkategorie entspricht.  Durch Klicken auf den Wert in der Spalte **Integritätszustand** wird die Integritätsdiagnose für den ausgewählten virtuellen Computer geöffnet.    
 
* Aus der Gast-VM auf der Registerkarte **Integrität** im Azure Monitor for VMs durch Auswählen von **Integritätsdiagnose anzeigen** 

In Integritätsdiagnosen sind die Integritätsinformationen nach den folgenden Kategorien geordnet: 

* Verfügbarkeit
* Leistung
 
Alle für eine bestimmte Komponente (z.B. logischer Datenträger oder CPU) definierten Integritätskriterien werden angezeigt. Zudem ist die Kategorie der Überwachung daneben in der Spalte **Integritätskriterien** zu sehen.  

Integritätskriterien werden durch einen dieser vier Zustände definiert: *Kritisch*, *Warnung*, *Fehlerfrei* und *Unbekannt*. Die ersten drei sind konfigurierbar, d.h., Sie können die Schwellenwerte der Überwachung mit der [Workloadüberwachungs-API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update) ändern. *Unbekannt* kann nicht konfiguriert werden und ist für bestimmte Szenarien reserviert.  

Die Seite „Integritätsdiagnose“ weist drei Hauptabschnitte auf:

* Komponentenmodell 
* Integritätskriterien
* Zustandsänderungen 

![Abschnitte auf der Seite „Integritätsdiagnose“](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Komponentenmodell
Die Spalte ganz links auf der Seite „Integritätsdiagnose“ ist das Komponentenmodell. In dieser Spalte werden alle dem virtuellen Computer zugeordneten Komponenten zusammen mit dem jeweiligen aktuellen Integritätszustand angezeigt. 

Im folgenden Beispiel sind die erkannten Instanzen Datenträger, logischer Datenträger, Prozessor, Arbeitsspeicher und Betriebssystem. Mehrere Instanzen dieser Komponenten werden ermittelt und in dieser Spalte angezeigt. In der folgenden Abbildung werden beispielsweise die zwei Instanzen von logischen Datenträgern – C: und D: – des virtuellen Computers angezeigt, die beide den Zustand „Fehlerfrei“ aufweisen.  

![Beispiel für die Darstellung eines Komponentenmodells in der Integritätsdiagnose](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Integritätskriterien
Die mittlere Spalte auf der Seite „Integritätsdiagnose“ ist die Spalte **Integritätskriterien**. Das für die VM definierte Integritätsmodell wird als Hierarchiebaum angezeigt. Das Integritätsmodell für einen virtuellen Computer besteht aus Einheits- und Aggregatintegritätskriterien.  

![Beispiel für die Darstellung von Integritätskriterien in der Integritätsdiagnose](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Ein Integritätskriterium misst die Integrität der überwachten Instanz anhand bestimmter Kriterien, bei denen es sich z.B. um einen Schwellenwert oder den Zustand einer Entität handeln kann. Ein Integritätskriterium weist wie weiter oben beschrieben entweder zwei oder drei Schwellenwerte für den Integritätszustand auf. Das Integritätskriterium kann sich jederzeit nur in einem seiner möglichen Zustände befinden. 

Die Gesamtintegrität eines Ziels wird durch die Integrität der einzelnen im Integritätsmodell definierten Integritätskriterien bestimmt. Dabei handelt es sich um eine Kombination aus Integritätskriterien, die direkt auf das Ziel gerichtet sind, und Integritätskriterien, die auf Komponenten gerichtet sind, für die mittels eines Aggregatintegritätskriteriums ein Rollup zum Ziel ausgeführt wird. Diese Hierarchie ist im Abschnitt **Integritätskriterien** der Seite Integritätsdiagnose veranschaulicht. Die Richtlinie für den Integritätsrollup ist Bestandteil der Konfiguration der Aggregatintegritätskriterien (Standardwert ist auf *Worst-of* festgelegt). Sie finden eine Liste der standardmäßig festgelegten Integritätskriterien, die im Rahmen dieses Features ausgeführt werden, im Abschnitt [Details der Überwachungskonfiguration](#monitoring-configuration-details).  

Für Integritätskriterien vom Typ **Einheit** kann die Konfiguration durch Klicken auf den Link mit den Auslassungspunkten ganz rechts geändert und durch Auswählen von **Details anzeigen** der Konfigurationsbereich geöffnet werden. 

![Beispiel für das Konfigurieren eines Integritätskriteriums](./media/vminsights-health/health-diagnostics-vm-example-02.png)

Durch Verwendung des Beispiels **Mittlere Dauer pro Schreibvorgang** im Konfigurationsbereich für das ausgewählte Integritätskriterium kann der zugehörige Schwellenwert mit einem anderen numerischen Wert konfiguriert werden. Es handelt sich um eine Überwachung mit zwei Zuständen, d.h., es ist nur eine Änderung von „Fehlerfrei“ in „Warnung“ möglich. Andere Integritätskriterien können drei Zustände aufweisen; bei diesen können Sie den Wert für den Warnungsschwellenwert und den Schwellenwert für den kritischen Zustand konfigurieren.  

>[!NOTE]
>Das Anwenden einer Konfigurationsänderung auf Integritätskriterien wirkt sich auf alle überwachten Instanzen aus.  Wenn Sie beispielsweise **Datenträger –1 D:** auswählen und den Schwellenwert **Mittlere Dauer pro Schreibvorgang** ändern, gilt die Änderung nicht nur für diese Instanz, sondern für alle auf dem virtuellen Computer erkannten und überwachten Instanzen von Datenträgern.
>

![Beispiel für das Konfigurieren eines Integritätskriteriums für einen Einheitenmonitor](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

Wenn Sie mehr zum Integritätsindikator erfahren möchten, können Sie in den enthaltenen Artikeln Probleme, Ursachen und Lösungen ermitteln. Klicken Sie auf der Seite auf den Link **Informationen anzeigen**. In Ihrem Browser wird eine neue Registerkarte mit dem entsprechenden Wissensartikel geöffnet. Sie können sich [hier](https://docs.microsoft.com/azure/monitoring/infrastructure-health/) jederzeit alle Wissensartikel zu Integritätskriterien ansehen, die im Integritätsfeature von Azure Monitor for VMs enthalten sind.
  
### <a name="state-changes"></a>Zustandsänderungen
Ganz rechts auf der Seite „Integritätsdiagnose“ wird die Spalte **Zustandsänderungen** angezeigt. Sie listet alle Zustandsänderungen auf, die den im Abschnitt **Integritätskriterien** ausgewählten Integritätskriterien zugeordnet sind, oder die Zustandsänderungen einer VM, wenn in der Spalte **Komponentenmodell** oder **Integritätskriterien** der Tabelle eine VM ausgewählt ist. 

![Beispiel für die Darstellung von Zustandsänderungen in der Integritätsdiagnose](./media/vminsights-health/health-diagnostics-page-statechanges.png)

Dieser Abschnitt besteht aus dem Status der Integritätskriterien und den entsprechenden Zeitpunkten, wobei der letzte Zustand oben angezeigt wird.   

### <a name="association-of-component-model-health-criteria-and-state-change-columns"></a>Zuordnung der Spalten „Komponentenmodell“, „Integritätskriterien“ und „Zustandsänderung“ 
Die drei Spalten sind miteinander verknüpft. Wenn Sie eine erkannte Instanz im Abschnitt **Komponentenmodell** auswählen, wird der Abschnitt **Integritätskriterien** nach dieser Komponentenansicht gefiltert. Entsprechend wird auch der Abschnitt **Zustandsänderung** basierend auf dem ausgewählten Integritätskriterium aktualisiert. 

![Beispiel zum Auswählen einer überwachten Instanz mit Ergebnissen](./media/vminsights-health/health-diagnostics-vm-example-01.png)

Wenn Sie im Beispiel oben **Datenträger – 1 D:** auswählen, wird die Struktur der Integritätskriterien nach **Datenträger – 1 D:** gefiltert. In der Spalte **Zustandsänderung** wird die Zustandsänderung basierend auf der Verfügbarkeit von **Datenträger – 1 D:** angezeigt. 

Um den aktualisierten Integritätszustand anzuzeigen, können Sie die Seite „Integritätsdiagnose“ aktualisieren, indem Sie auf den Link **Aktualisieren** klicken.  Wenn basierend auf dem vordefinierten Abrufintervall eine Aktualisierung des Integritätsstatus des Integritätskriteriums erfolgt, können Sie mit dieser Aufgabe Wartezeiten vermeiden und den aktuellen Integritätsstatus anzeigen.  Der **Zustand der Integritätskriterien** ist ein Filter, über den Sie den Bereich der Ergebnisse basierend auf dem ausgewählten Integritätszustand – *Fehlerfrei*, *Warnung*, *Kritisch*, *Unbekannt* und *Alle* – festlegen können.  Die Zeitangabe **Letzte Aktualisierung** oben rechts stellt den Zeitpunkt der letzten Aktualisierung der Seite „Integritätsdiagnose“ dar.  

## <a name="alerts"></a>Alerts
Das Integritätsfeature von Azure Monitor for VMs ist in [Azure-Warnungen](../../azure-monitor/platform/alerts-overview.md) integriert und löst eine Warnung aus, wenn die vordefinierten Integritätskriterien von „Fehlerfrei“ in einen fehlerhaften Zustand wechseln und der Umstand erkannt wird. Warnungen werden nach dem Schweregrad kategorisiert – Schweregrad 0 bis 4, wobei 0 den höchsten Schweregrad darstellt.  

Die Gesamtzahl der VM-Integritätswarnungen, nach Schweregrad kategorisiert, steht auf dem Dashboard **Integrität** im Bereich **Warnungen** zur Verfügung. Wenn Sie entweder die Gesamtzahl der Warnungen oder die einem Schweregrad entsprechende Anzahl auswählen, wird die Seite **Warnungen** geöffnet und listet alle Warnungen auf, die Ihrer Auswahl entsprechen.  Wenn Sie beispielsweise die Zeile auswählen, die **Schweregrad 1** entspricht, sehen Sie eine Ansicht ähnlich der folgenden:

![Beispiel für alle Warnungen mit Schweregrad 1](./media/vminsights-health/vminsights-sev1-alerts-01.png)

Die Warnungen auf der Seite **Warnungen** weisen nicht nur einen auf Ihre Auswahl eingeschränkten Bereich auf, sondern sind darüber hinaus nach **Ressourcentyp** gefiltert und zeigen nur Integritätswarnungen an, die von der VM-Ressource ausgelöst wurden.  Dies spiegelt sich in der Liste der Warnungen in der Spalte **Zielressource** wider, wo die Azure-VM angezeigt wird, für die die Warnung beim Erreichen der Fehlerbedingung für das einzelne Integritätskriterium ausgegeben wurde.  

Warnungen von anderen Ressourcentypen oder Diensten sollen nicht in dieser Ansicht enthalten sein, also z. B. keine Protokollwarnungen, die auf Log Analytics-Abfragen basieren, oder Metrikwarnungen, die Sie normalerweise auf der Standardseite [Alle Warnungen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) im Azure Monitor anzeigen. 

Zum Filtern dieser Ansicht können Sie Werte in den Dropdownmenüs am oberen Rand der Seite auswählen.

|Column |BESCHREIBUNG | 
|-------|------------| 
|Abonnement |Wählen Sie ein Azure-Abonnement aus. Nur Warnungen im ausgewählten Abonnement sind in der Ansicht enthalten. | 
|Ressourcengruppe |Wählen Sie eine einzelne Ressourcengruppe aus. Nur Warnungen mit Zielen in der ausgewählten Ressourcengruppe sind in der Ansicht enthalten. | 
|Ressourcentyp |Wählen Sie mindestens einen Ressourcentyp aus. Standardmäßig sind in dieser Ansicht nur Warnungen mit dem Ziel **Virtuelle Computer** ausgewählt und enthalten. Diese Spalte ist nur verfügbar, nachdem eine Ressourcengruppe angegeben wurde. | 
|Ressource |Wählen Sie eine Ressource aus. Nur Warnungen mit dieser Ressource als Ziel sind in der Ansicht enthalten. Diese Spalte ist nur verfügbar, nachdem ein Ressourcentyp angegeben wurde. | 
|Severity |Wählen Sie einen Warnungsschweregrad oder *Alle* aus, um Warnungen aller Schweregrade einzuschließen. | 
|Überwachungsbedingung |Wählen Sie eine Überwachungsbedingung aus, um Warnungen danach zu filtern, ob sie vom System *Ausgelöst* oder vom System *Gelöst* wurden, falls die Bedingung nicht mehr aktiv ist. Oder wählen Sie *Alle* aus, um Warnungen in allen Zuständen einzuschließen. | 
|Warnungsstatus |Wählen Sie einen Warnungsstatus aus, *Neu*, *Bestätigt*, *Geschlossen*, oder wählen Sie *Alle* aus, um Warnungen mit jedem Status einzuschließen. | 
|Überwachungsdienst |Wählen Sie einen Dienst oder *Alle* aus, um alle Dienste einzuschließen. Für dieses Feature werden nur Warnungen von *VM Insights* unterstützt.| 
|Zeitbereich| Nur Warnungen, die innerhalb des ausgewählten Zeitfensters ausgelöst wurden, sind in der Ansicht enthalten. Unterstützte Werte sind die letzte Stunde, die letzten 24 Stunden, die letzten 7 Tage und die letzten 30 Tage. | 

Die Seite **Warnungsdetail** wird angezeigt, wenn Sie eine Warnung auswählen, stellt Details zur Warnung zur Verfügung und erlaubt Ihnen, ihren Status zu ändern. Weitere Informationen zum Verwalten von Warnungen finden Sie unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](../../azure-monitor/platform/alerts-metric.md).  

>[!NOTE]
>Derzeit ist es nicht möglich, über das Portal neue Warnungen basierend auf Integritätskriterien zu erstellen oder vorhandene Integritätswarnungsregeln in Azure Monitor zu ändern.  
>

![Bereich „Warnungsdetails“ für eine ausgewählte Warnung](./media/vminsights-health/alert-details-pane-01.png)

Der Warnungszustand kann außerdem für eine oder mehrere Warnungen geändert werden, indem Sie in der oberen linken Ecke der Seite **Alle Warnungen** den Befehl **Status ändern** auswählen. Im Bereich **Warnungsstatus ändern** wählen Sie einen der Status aus, fügen eine Beschreibung der Änderung im Feld **Kommentar** hinzu und klicken dann auf **OK**, um Ihre Änderungen zu committen. Während die Informationen überprüft und die Änderungen übernommen werden, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.  

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Erkennen von Engpässen und Gesamtauslastung im Hinblick auf die Leistung Ihrer VM finden Sie unter [View Azure VM Performance](vminsights-performance.md) (Anzeigen der Leistung von Azure-VMs). Informationen zu erkannten Anwendungsabhängigkeiten finden Sie unter [View Azure Monitor for VMs Map](vminsights-maps.md) (Anzeigen der Zuordnung von Azure Monitor for VMs). 
