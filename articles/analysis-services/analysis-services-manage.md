---
title: aaaManage Azure Analysis Services | Microsoft Docs
description: "Lär dig hur toomanage en Analysis Services-server i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: b03bc440801a68162039e28cdb4f863da374014e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-analysis-services"></a>Hantera Analysis Services
När du har skapat en Analysis Services-server i Azure, kanske vissa administrations- och uppgifter som du behöver tooperform direkt eller senare på hello väg. Till exempel toohello uppdatera data, styra vem som kan komma åt hello modeller på servern eller övervaka ditt Servertillstånd bearbetas. Vissa hanteringsuppgifter kan endast utföras i Azure-portalen andra i SQL Server Management Studio (SSMS), och vissa aktiviteter kan göras antingen i.

## <a name="azure-portal"></a>Azure Portal
[Azure-portalen](http://portal.azure.com/) är där du kan skapa och ta bort servrar, övervaka serverresurser, ändra storlek och hantera vem som har åtkomst tooyour servrar.  Om du har några problem, kan du även skicka en supportförfrågan.

![Hämta servernamnet i Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Ansluter tooyour server i Azure är precis som ansluter tooa server-instans i din organisation. Du kan utföra många av hello samma uppgifter, till exempel processdata eller skapa ett skript för bearbetning, hantera roller och använda PowerShell från SSMS.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Hämta och installera SSMS
tooget alla hello senaste funktionerna och hello jämnaste upplevelse när de ansluter tooyour Azure Analysis Services-servern måste du använder hello senaste versionen av SSMS. 

[Hämta SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="tooconnect-with-ssms"></a>tooconnect med SSMS
 Kontrollera ditt användarnamn ingår i administratörsgruppen för hello analys när med hjälp av SSMS, innan du ansluter tooyour serverns hälsning första gången. Det finns fler toolearn [serveradministratörer](#server-administrators) senare i den här artikeln.

1. Innan du ansluter måste tooget hello servernamn. I **Azure-portalen** > server > **översikt** > **servernamn**, kopiera hello servernamn.
   
    ![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. I SSMS > **Object Explorer**, klickar du på **Anslut** > **Analysis Services**.
3. I hello **ansluta tooServer** klistra in i hello-servernamn, sedan i dialogrutan **autentisering**, väljer du något av följande typer av autentisering hello:
   
    **Windows-autentisering** toouse dina Windows-autentiseringsuppgifter domän\användarnamn och lösenord.

    **Active Directory-lösenordsautentisering** toouse ett organisationskonto. När ansluter från en icke-domän exempelvis anslutna datorn.

    **Active Directory Universal autentisering** toouse [icke-interaktiv eller Multi-Factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![Ansluta i SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Server-administratörer och användare
Det finns två typer av användare, administratörer och användare i Azure Analysis Services. Båda typerna av användare måste finnas i Azure Active Directory och måste anges med organisationens e-postadress eller UPN. Det finns fler toolearn [autentisering och användarbehörigheter](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Felsökning av anslutningsproblem
När du ansluter med hjälp av SSMS, om du stöter på problem kan behöva tooclear hello inloggningen cache. Inget är cachelagrade toodisc. tooclear hello cache, Stäng och starta om hello ansluta processen. 

## <a name="next-steps"></a>Nästa steg
Om du inte redan har distribuerat en tabellmodell tooyour ny server, nu är ett bra tillfälle. Det finns fler toolearn [distribuera tooAzure Analysis Services](analysis-services-deploy.md).

Om du har distribuerat en modell tooyour server, är du redo tooconnect tooit med hjälp av en klient eller en webbläsare. Det finns fler toolearn [hämta data från Azure Analysis Services-servern](analysis-services-connect.md).

