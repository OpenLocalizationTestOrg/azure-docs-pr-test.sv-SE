---
title: Hantera Azure Analysis Services med PowerShell | Microsoft Docs
description: Azure Analysis Services-hantering med PowerShell.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 52c74feacb8cf2e7005f6b284d7b55078449dc79
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/02/2017
---
# <a name="manage-azure-analysis-services-with-powershell"></a>Hantera Azure Analysis Services med PowerShell

Den här artikeln beskriver PowerShell-cmdlets som används för att utföra Azure Analysis Services-servern och databasen hanteringsuppgifter. 

Hanteringsaktiviteter för Server, till exempel att skapa eller ta bort en server, pausa eller återuppta åtgärder eller ändra service-nivån (nivån) använda cmdlets för Azure Resource Manager (AzureRM). Andra uppgifter för hantering av databaser, till exempel att lägga till eller ta bort medlemmar i rollen bearbetning eller partitionering använda cmdletar som ingår i samma modul för SqlServer som SQL Server Analysis Services.

## <a name="permissions"></a>Behörigheter
De flesta PowerShell uppgifter kräver att du har administratörsrättigheter på Analysis Services-servern som du hanterar. Schemalagda aktiviteter för PowerShell är obevakad åtgärder. Det konto som kör Schemaläggaren måste ha administratörsrättigheter på Analysis Services-servern. 

För servern med hjälp av AzureRm cmdlets, ditt konto eller det konto som kör Schemaläggaren måste också tillhöra ägarrollen för resursen i [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-what-is.md). 

## <a name="server-operations"></a>Åtgärder 
Azure Analysis Services-cmdlets som ingår i den [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) komponent modulen. Om du vill installera AzureRM cmdlet-moduler [Azure Resource Manager cmdlets](/powershell/azure/overview) i PowerShell-galleriet.

|Cmdlet|Beskrivning| 
|------------|-----------------| 
|[Lägg till AzureAnalysisServicesAccount](/powershell/module/azurerm.analysisservices/add-azureanalysisservicesaccount)|Lägger till en autentiserad konto som används för Azure Analysis Services serverbegäranden cmdlet.| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Hämtar information om en server-instans.|  
|[Ny AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Skapar en server-instans.|   
|[Ta bort AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Tar bort en server-instans.|  
|[Starta om AzureAnalysisServicesInstance](/powershell/module/azurerm.analysisservices/restart-azureanalysisservicesinstance)|Startar om en instans av Analysis Services-server i miljön är inloggad för tillfället. angetts i kommandot Lägg till AzureAnalysisServicesAccount.|  
|[Återuppta AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Återupptar en server-instans.|  
|[Pausa AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Pausar ett server-instansen.| 
|[Ange AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Ändrar en server-instans.|   
|[Testa AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Testar förekomsten av en server-instans.| 

## <a name="database-operations"></a>Databasåtgärder

Åtgärder i Azure Analysis Services-databasen använder samma [SqlServer](https://www.powershellgallery.com/packages/SqlServer) modulen som SQL Server Analysis Services. Dock stöds inte alla cmdlets för Azure Analysis Services. 

SqlServer-modulen innehåller cmdlet: ar uppgiftsspecifika databasen samt allmänna Invoke-ASCmd cmdlet som accepterar en tabellvy modellen Scripting språk (TMSL) fråga eller skript. Följande cmdlets i modulen SqlServer har stöd för Azure Analysis Services.

  
|Cmdlet|Beskrivning|
|------------|-----------------| 
|[Lägg till RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Lägga till en medlem i en databasroll.| 
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
