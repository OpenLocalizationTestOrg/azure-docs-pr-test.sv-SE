---
title: aaaManage Azure Analysis Services med PowerShell | Microsoft Docs
description: Azure Analysis Services-hantering med PowerShell.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: owend
ms.openlocfilehash: bc4250bf77b5a0d86c1049ee57493bcf2a1f0c1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Hantera Azure Analysis Services med PowerShell

Den här artikeln beskriver PowerShell cmdlets som används för tooperform Azure Analysis Services-servern och databasen hanteringsuppgifter. 

Hanteringsaktiviteter för Server, till exempel att skapa eller ta bort en server, pausa eller återuppta åtgärder eller ändra hello-servicenivå (nivån) använda cmdlets för Azure Resource Manager (AzureRM). Andra uppgifter för att hantera databaser som att lägga till eller ta bort medlemmar i rollen, bearbetning eller partitionering Använd cmdlets som ingår i hello samma SqlServer-modul som SQL Server Analysis Services.

## <a name="permissions"></a>Behörigheter
De flesta PowerShell uppgifter kräver att du har administratörsrättigheter på hello Analysis Services-server som du hanterar. Schemalagda aktiviteter för PowerShell är obevakad åtgärder. hello-kontot som kör hello Schemaläggaren måste ha administratörsrättigheter på hello Analysis Services-servern. 

För servern med hjälp av AzureRm cmdlets, ditt konto eller hello körs Schemaläggaren måste också tillhöra toohello ägarrollen för hello resurs i [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-what-is.md). 

## <a name="server-operations"></a>Åtgärder 
Azure Analysis Services-cmdlets som ingår i hello [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) komponent modulen. tooinstall AzureRM cmdlet-moduler, se [Azure Resource Manager cmdlets](/powershell/azure/overview) i hello PowerShell-galleriet.

|Cmdlet|Beskrivning| 
|------------|-----------------| 
|[Export-AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/export-azureanalysisservicesinstancelog)|Export logga toofile.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Hämtar information om en server-instans.|  
|[Ny AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Skapar en server-instans.|
|[Ta bort AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Tar bort en server-instans.|  
|[Pausa AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Pausar ett server-instansen.| 
|[Återuppta AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Återupptar en server-instans.|  
|[Ange AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Ändrar en server-instans.|   
|[Testa AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Testerna hello förekomsten av en server-instans.| 

## <a name="database-operations"></a>Databasåtgärder

Azure Analysis Services använder databasåtgärder hello samma [SqlServer](https://www.powershellgallery.com/packages/SqlServer) modulen som SQL Server Analysis Services. Dock stöds inte alla cmdlets för Azure Analysis Services. 

hello SqlServer modulen innehåller uppgiftsspecifika databasen cmdlet: ar som samt hello generella Invoke-ASCmd cmdlet som accepterar en tabellvy modellen Scripting språk (TMSL) fråga eller skript. hello har följande cmdletar i hello SqlServer modulen stöd för Azure Analysis Services.

  
|Cmdlet|Beskrivning|
|------------|-----------------| 
|[Lägg till RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Lägg till en medlem tooa databasroll.| 
|[Säkerhetskopiering ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Säkerhetskopiera databas för Analysis Services.|  
|[Ta bort RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Ta bort en medlem från en databasroll.|   
|[Anropa ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Köra ett skript för TMSL.|
|[Anropa ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Bearbeta en databas.|  
|[Anropa ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|Bearbeta en partition.| 
|[Anropa ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Bearbeta en tabell.|  
|[Merge-Partition](https://msdn.microsoft.com/library/hh479576.aspx)|Koppla en partition.|  
|[Återställ ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Återställa en Analysis Services-databas.| 
  

## <a name="related-information"></a>Relaterad information

* [Hämta SQL Server PowerShell-modulen](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Hämta SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   
* [SqlServer-modul i PowerShell-galleriet](https://www.powershellgallery.com/packages/SqlServer)    
* [Tabell modellen Programming för kompatibilitet nivå 1200 och högre](https://msdn.microsoft.com/library/mt712541.aspx)
