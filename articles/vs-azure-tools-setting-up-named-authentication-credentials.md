---
title: aaaSetting upp med namnet autentiseringsuppgifter | Microsoft Docs
description: "Lär dig hur tootooprovide autentiseringsuppgifter som Visual Studio kan använda tooauthenticate begäranden tooAzure toopublish ett program tooAzure från Visual Studio eller toomonitor en befintlig molntjänst... "
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 3cc147a2f7a3e766293ca282f56078cf24281346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-named-authentication-credentials"></a>Konfigurera namngivna autentiseringsuppgifter
toopublish ett program tooAzure från Visual Studio eller toomonitor en befintlig molntjänst, måste du ange autentiseringsuppgifter som Visual Studio kan använda tooauthenticate begär tooAzure. Det finns flera platser i Visual Studio, där du kan logga in tooprovide autentiseringsuppgifterna. Till exempel från hello Server Explorer, du kan öppna hello snabbmenyn för hello **Azure** nod och välj **ansluta tooMicrosoft Azure-prenumerationen... **. När du loggar in information om hello-prenumerationer som är kopplade till ditt Azure-konto finns i Visual Studio och det inte finns något mer du har toodo.

Azure-verktyg har också stöd för ett äldre sätt för att tillhandahålla autentiseringsuppgifter med hjälp av hello prenumeration fil (.publishsettings-fil). Det här avsnittet beskriver den här metoden stöds fortfarande i Azure SDK 2.2.

hello följande objekt krävs för autentisering tooAzure:

* Prenumerations-ID
* Ett giltigt X.509 v3-certifikat

> [!NOTE]
> hello-längden hello X.509 v3-certifikat måste vara minst 2 048 bitar. Azure avvisar alla certifikat som uppfyller inte det här kravet eller som inte är giltigt.
>
>

Visual Studio använder ditt prenumerations-ID tillsammans med hello certifikatdata som autentiseringsuppgifter. hello rätt autentiseringsuppgifter refereras i hello prenumeration fil (.publishsettings), som innehåller en offentlig nyckel för hello certifikatet. hello prenumeration fil kan innehålla referenser för mer än en prenumeration.

Du kan redigera hello prenumerationsinformation från hello **ny/redigera prenumeration** dialogrutan som beskrivs senare i det här avsnittet.

Om du vill att toocreate certifikat själv, kan du läsa toohello instruktionerna i [skapa och ladda upp ett Hanteringscertifikat för Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) och sedan ladda upp hello certifikat toohello manuellt [klassiska Azure-portalen ](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Autentiseringsuppgifterna som Visual Studio kräver toomanage dina molntjänster som inte är hello samma autentiseringsuppgifter som är nödvändiga tooauthenticate en begäran mot hello Azure storage-tjänster.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>Importera, skapa eller redigera autentiseringsuppgifter i Visual Studio
Du kan importera, ställa in eller ändra dina autentiseringsuppgifter i hello **ny prenumeration** dialogrutan som visas om du utför hello följande åtgärd:

* I **Server Explorer**öppnar hello snabbmenyn för hello **Azure** noden, Välj **hantera och Filter prenumerationer... **, Välj hello **certifikat** fliken och gör något av följande hello:

    * Välj **importera** tooopen hello Importera Microsoft Azure-prenumerationer dialogrutan där du kan hämta hello prenumerationer filen för hello för närvarande läsas in prenumerationen, bläddra tooits hämtningsplats och sedan importera den för användning i autentisering.
    * Välj **ny** tooopen hello prenumeration dialogrutan där du kan ställa in en ny prenumeration för autentisering.
    * Välj **redigera** (när du har valt prenumerationen aktiv) tooopen hello redigera dialogrutan för prenumerationer där du kan redigera en befintlig prenumeration för autentisering. 

hello följande procedur förutsätter att hello **ny prenumeration** dialogruta är öppen.

### <a name="tooset-up-authentication-credentials-in-visual-studio"></a>tooset in autentiseringsuppgifter i Visual Studio
1. I hello **välja ett befintligt certifikat** för autentisering lista, Välj ett certifikat.
2. Välj hello **kopiera hello fullständig sökväg** länk. hello sökvägen för hello-certifikat (.cer-fil) är kopierade toohello Urklipp.

   > [!IMPORTANT]
   > toopublish ditt Azure-program från Visual Studio måste du överföra det här certifikatet toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   >
   >
3. tooupload hello certifikat toohello Azure-portalen:

   1. Öppna hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   2. Om du uppmanas logga in toohello portal och gå sedan för**inställningar**, **Hanteringscertifikat**.
   3. Hello Management certifikat fönstret Välj **överför**.
   4. Välj din Azure-prenumeration, klistra in hello fullständig sökväg till hello .cer-fil som du just skapade och välj **överför**.
