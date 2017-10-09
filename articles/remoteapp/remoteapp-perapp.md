---
title: "aaaPublish program tooindividual användare i Azure RemoteApp-samling (förhandsgranskning) | Microsoft Docs"
description: "Lär dig hur du kan publicera appar tooindividual användare, i stället för beroende av grupper i Azure RemoteApp."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
ms.openlocfilehash: 87b435552ddbfc2c6d03aeb01d95a44985e71f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-tooindividual-users-in-an-azure-remoteapp-collection-preview"></a>Publicera program tooindividual användare i Azure RemoteApp-samling (förhandsgranskning)
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Den här artikeln förklarar hur toopublish program tooindividual användare i en Azure RemoteApp-samling. Detta är en ny funktion i Azure RemoteApp, för tillfället i privat förhandsvisning och finns bara tooselect Tidiga efterföljare i utvärderingssyfte.

Innebar av bara ett sätt att publicera program i Azure RemoteApp: hello administratören publicerade appar från hello avbildningen och de skulle vara synliga tooall användare i hello samling.

Ett vanligt scenario är tooinclude många program i en enda avbildning och distribuera en samling i ordning tooreduce hanteringskostnader. Ofta är inte alla program som är relevanta tooall användare – administratörer skulle föredra toopublish appar tooindividual användare så att de inte ser onödiga program i deras program-feed.

Det är nu möjligt att göra i Azure RemoteApp – för tillfället är det en begränsad funktion i förhandsgranskningen . Här är en kort sammanfattning av hello nya funktioner:

1. En samling kan anges till ett av två lägen:
   
   * hello ursprungliga samling-läge, där alla användare i en samling kan se publicerade alla program. Det här är standardläget för hello.
   * hello nya programläge, där användarna bara ser program som har tilldelats explicit toothem
2. Hello tillfället kan hello programläge endast aktiveras med hjälp av Azure RemoteApp PowerShell-cmdlets.
   
   * När Användartilldelning i samlingen hello-uppsättningen tooapplication läge inte kan hanteras via hello Azure-portalen. Användartilldelning måste toobe hanteras via PowerShell-cmdlets.
3. Användarna ser bara hello program publiceras direkt toothem. Men kan det fortfarande vara möjligt för en användare toolaunch hello andra program som är tillgängliga på hello avbildning genom att öppna dem direkt i hello-operativsystemet.
   
   * Den här funktionen innehåller inte en säker låsning av program. den begränsar bara synligheten i hello program-feed.
   * Om du behöver tooisolate användare från program behöver toouse separata samlingar för som.

## <a name="how-tooget-azure-remoteapp-powershell-cmdlets"></a>Hur tooget Azure RemoteApp PowerShell-cmdlets
tootry hello nya funktionen för förhandsgranskning, behöver du toouse Azure PowerShell-cmdlets. Det är för närvarande inte möjligt toouse hello Azure Management portal tooenable hello nya läget för programpublicering.

Kontrollera först att du har hello [Azure PowerShell-modulen](/powershell/azure/overview) installerad.

Starta hello PowerShell-konsolen i administratörsläge och kör hello följande cmdlet:

        Add-AzureAccount

Du uppmanas att ange användarnamn och lösenord för Azure. När du är inloggad kommer du att kunna toorun Azure RemoteApp-cmdlets mot dina Azure-prenumerationer.

## <a name="how-toocheck-which-mode-a-collection-is-in"></a>Hur toocheck vilket läge en samling är i
Kör följande cmdlet hello:

        Get-AzureRemoteAppCollection <collectionName>

![Kontrollera samlingsläget hello](./media/remoteapp-perapp/araacllelvel.png)

Hej AclLevel-egenskapen kan ha hello följande värden:

* Samling: hello ursprungliga publiceringsläget. Alla användare ser alla publicerade appar.
* Program: hello nya publiceringsläget. Användarna ser bara hello appar direkt publicerade toothem.

## <a name="how-tooswitch-tooapplication-publishing-mode"></a>Hur tooswitch tooapplication publiceringsläget
Kör följande cmdlet hello:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Programmets publiceringstillstånd bevaras: till en början ser alla användare alla hello ursprungliga publicerade appar.

## <a name="how-toolist-users-who-can-see-a-specific-application"></a>Hur toolist användare som kan se ett visst program
Kör följande cmdlet hello:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Detta visar alla användare som kan se hello program.

Obs: Du kan se hello programalias (kallas ”app-alias” i hello ovanstående syntax) genom att köra Get-AzureRemoteAppProgram – samlingsnamn <collectionName>.

## <a name="how-tooassign-an-application-tooa-user"></a>Hur tooassign en tooa programanvändare
Kör följande cmdlet hello:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

hello användaren ser nu programmet hello i hello Azure RemoteApp-klienten och kommer att kunna tooconnect tooit.

## <a name="how-tooremove-an-application-from-a-user"></a>Hur tooremove ett program från en användare
Kör följande cmdlet hello:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Skicka feedback
Vi uppskattar din feedback och dina förslag om den här funktionen för förhandsgranskning. Fyll i hello [undersökning](http://www.instant.ly/s/FDdrb) toolet oss om vad du tycker.

## <a name="havent-had-a-chance-tootry-hello-preview-feature"></a>Har inte haft ett chansen tootry hello förhandsgranskningsfunktion?
Om du inte har deltagit i förhandsgranskningen hello ännu, använder du denna [undersökning](http://www.instant.ly/s/AY83p) toorequest åtkomst.

