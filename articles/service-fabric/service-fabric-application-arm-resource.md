---
title: Bereitstellen und Aktualisieren von Anwendungen und Diensten mit Azure Resource Manager | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anwendungen und Dienste mithilfe einer Azure Resource Manager-Vorlage in einem Service Fabric-Cluster bereitstellen.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: dekapur
ms.openlocfilehash: feb9d0a01cbba75fc9868f5a603d494c5c09ae2e
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49386296"
---
# <a name="manage-applications-and-services-as-azure-resource-manager-resources"></a>Verwalten von Anwendungen und Diensten als Azure Resource Manager-Ressourcen

Sie können Anwendungen und Dienste mithilfe von Azure Resource Manager in Ihrem Service Fabric-Cluster bereitstellen. Das bedeutet, dass Sie Anwendungen nicht mehr über PowerShell oder die CLI bereitstellen und verwalten müssen, nachdem Sie gewartet haben, bis der Cluster bereit ist. Sie können stattdessen Anwendungen und Dienste nun in JSON ausdrücken und sie in derselben Resource Manager-Vorlage wie Ihren Cluster bereitstellen. Der Vorgang der Anwendungsregistrierung und -bereitstellung erfolgt in nur einem Schritt.

Diese Vorgehensweise empfiehlt sich für die Bereitstellung von Setup-, Governance- oder Clusterverwaltungsanwendungen, die Sie in Ihrem Cluster benötigen. Dies schließt die [Anwendung für die Patchorchestrierung](service-fabric-patch-orchestration-application.md), Watchdogs oder andere Anwendungen ein, die in Ihrem Cluster ausgeführt werden müssen, ehe andere Anwendungen und Dienste bereitgestellt werden. 

Falls möglich, verwalten Sie Ihre Anwendungen als Ressourcen-Manager-Ressourcen, um Folgendes zu verbessern:
* Überwachungsprotokoll: Der Ressourcen-Manager überprüft jeden einzelnen Vorgang und führt ein detailliertes *Aktivitätsprotokoll*, das Ihnen helfen kann, Änderungen an diesen Anwendungen und Ihrem Cluster nachzuvollziehen.
* Rollenbasierte Zugriffssteuerung (RBAC): Die Verwaltung des Zugriffs auf Cluster sowie auf Anwendungen, die im Cluster bereitgestellt sind, kann über dieselbe Resource Manager-Vorlage erfolgen.
* Azure Resource Manager (über Azure-Portal) fungiert als Zentrale für die Verwaltung Ihres Clusters und kritischer Anwendungsbereitstellungen.

Der folgende Codeausschnitt zeigt die verschiedenen Arten von Ressourcen, die mittels einer Vorlage verwaltet werden können:

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```


## <a name="add-a-new-application-to-your-resource-manager-template"></a>Hinzufügen einer neuen Anwendung zu Ihrer Resource Manager-Vorlage

1. Bereiten Sie die Resource Manager-Vorlage Ihres Clusters auf die Bereitstellung vor. Weitere Informationen hierzu Sie unter [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).
2. Überlegen Sie sich einige der Anwendungen, die Sie im Cluster bereitstellen möchten. Gibt es welche, die stets ausgeführt werden und für die andere Anwendungen ggf. Abhängigkeiten aufweisen? Planen Sie die Bereitstellung von Governance- oder Setup-Anwendungen für den Cluster? Diese Arten von Anwendungen werden, wie oben beschrieben, am besten mithilfe einer Resource Manager-Vorlage verwaltet. 
3. Sobald Sie herausgefunden haben, welche Anwendungen auf diese Weise bereitgestellt werden sollen, müssen die Anwendungen gepackt, gezippt und in einer Dateifreigabe bereitgestellt werden. Die Freigabe muss über einen REST-Endpunkt zugänglich sein, den Azure Resource Manager während der Bereitstellung verwenden kann.
4. Beschreiben Sie in Ihrer Resource Manager-Vorlage unter Ihrer Clusterdeklaration die Eigenschaften der einzelnen Anwendungen. Zu diesen Eigenschaften gehören die Anzahl von Replikaten und Instanzen sowie Abhängigkeiten zwischen Ressourcen (anderen Anwendungen oder Diensten). Eine umfassende Liste der Eigenschaften finden Sie unter [REST-API – Swagger-Spezifikation](https://aka.ms/sfrpswaggerspec). Beachten Sie, dass dies nicht die Anwendungs- oder Dienstmanifeste ersetzt, sondern vielmehr einige der darin enthaltenen Elemente als Teil der Resource Manager-Vorlage des Clusters beschreibt. Hier ist eine Beispielvorlage, die die Bereitstellung des zustandslosen Diensts *Service1* und des zustandsbehafteten Diensts *Service2* als Teil von *Application1* enthält:

  ```json
  {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "clusterName": {
        "type": "string",
        "defaultValue": "Cluster",
        "metadata": {
          "description": "Name of your cluster - Between 3 and 23 characters. Letters and numbers only."
        }
      },
      "applicationTypeName": {
        "type": "string",
        "defaultValue": "ApplicationType",
        "metadata": {
          "description": "The application type name."
        }
      },
      "applicationTypeVersion": {
        "type": "string",
        "defaultValue": "1",
        "metadata": {
          "description": "The application type version."
        }
      },
      "appPackageUrl": {
        "type": "string",
        "metadata": {
          "description": "The URL to the application package sfpkg file."
        }
      },
      "applicationName": {
        "type": "string",
        "defaultValue": "Application1",
        "metadata": {
          "description": "The name of the application resource."
        }
      },
      "serviceName": {
        "type": "string",
        "defaultValue": "Application1~Service1",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName": {
        "type": "string",
        "defaultValue": "Service1Type",
        "metadata": {
          "description": "The name of the service type."
        }
      },
      "serviceName2": {
        "type": "string",
        "defaultValue": "Application1~Service2",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName2": {
        "type": "string",
        "defaultValue": "Service2Type",
        "metadata": {
          "description": "The name of the service type."
        }
      }
    },
    "variables": {
      "clusterLocation": "[resourcegroup().location]"
    },
    "resources": [
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [],
        "properties": {
          "provisioningState": "Default"
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "appPackageUrl": "[parameters('appPackageUrl')]"
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "typeName": "[parameters('applicationTypeName')]",
          "typeVersion": "[parameters('applicationTypeVersion')]",
          "parameters": {},
          "upgradePolicy": {
            "upgradeReplicaSetCheckTimeout": "01:00:00.0",
            "forceRestart": "false",
            "rollingUpgradeMonitoringPolicy": {
              "healthCheckWaitDuration": "00:02:00.0",
              "healthCheckStableDuration": "00:05:00.0",
              "healthCheckRetryTimeout": "00:10:00.0",
              "upgradeTimeout": "01:00:00.0",
              "upgradeDomainTimeout": "00:20:00.0"
            },
            "applicationHealthPolicy": {
              "considerWarningAsError": "false",
              "maxPercentUnhealthyDeployedApplications": "50",
              "defaultServiceTypeHealthPolicy": {
                "maxPercentUnhealthyServices": "50",
                "maxPercentUnhealthyPartitionsPerService": "50",
                "maxPercentUnhealthyReplicasPerPartition": "50"
              }
            }
          }
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateless",
          "serviceTypeName": "[parameters('serviceTypeName')]",
          "instanceCount": "-1",
          "partitionDescription": {
            "partitionScheme": "Singleton"
          },
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": []
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName2'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateful",
          "serviceTypeName": "[parameters('serviceTypeName2')]",
          "targetReplicaSetSize": "3",
          "minReplicaSetSize": "2",
          "replicaRestartWaitDuration": "00:01:00.0",
          "quorumLossWaitDuration": "00:02:00.0",
          "standByReplicaKeepDuration": "00:00:30.0",
          "partitionDescription": {
            "partitionScheme": "UniformInt64Range",
            "count": "5",
            "lowKey": "1",
            "highKey": "5"
          },
          "hasPersistedState": "true",
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": [],
          "defaultMoveCost": "Low"
        }
      }
    ]
  }
  ```

  > [!NOTE] 
  > *apiVersion* muss auf `"2017-07-01-preview"` festgelegt werden. Diese Vorlage kann auch unabhängig vom Cluster bereitgestellt werden, sofern der Cluster bereits bereitgestellt wurde.

5. Bereitstellen 

## <a name="manage-an-existing-application-via-resource-manager"></a>Verwalten einer vorhandenen Anwendung über Ressourcen-Manager

Angenommen, Ihr Cluster ist bereits eingerichtet und einige Anwendungen, die Sie als Resource Manager-Ressourcen verwalten möchten, sind bereits im Cluster bereitgestellt. Sie können, statt die Anwendungen zu entfernen und neu bereitzustellen, einen PUT-Aufruf mit denselben APIs verwenden, um die Anwendungen als Resource Manager-Ressourcen zu bestätigen. 

> [!NOTE]
> Damit ein Clusterupgrade fehlerhafte Apps ignorieren kann, kann der Kunde „maxpercentunhealthyapplications: 100“ im Abschnitt „upgradeDescription/healthPolicy“ festlegen. Ausführliche Beschreibungen für alle Einstellungen finden Sie in der [Dokumentation zur REST-API-Clusterupgraderichtlinie von Service Fabric](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterupgradepolicy).

## <a name="next-steps"></a>Nächste Schritte

* Verwenden der [Service Fabric CLI](service-fabric-cli.md) oder von [PowerShell](service-fabric-deploy-remove-applications.md) zum Bereitstellen anderer Anwendungen in Ihrem Cluster. 
* [Upgrade von Service Fabric-Clustern](service-fabric-cluster-upgrade.md)

