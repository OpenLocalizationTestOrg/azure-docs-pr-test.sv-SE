---
title: "aaaHow toocreate och distribuera en tjänst i molnet | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera en tjänst i molnet med hjälp av hello Azure-portalen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 56ea2f14-34a2-4ed9-857c-82be4c9d0579
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: dc8b81a594f3514e662c49c9a46a33da8a551f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a>Hur toocreate och distribuera en tjänst i molnet
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [Klassisk Azure-portal](cloud-services-how-to-create-deploy.md)
>
>

hello Azure-portalen ger dig två sätt du toocreate och distribuera en tjänst i molnet: *Snabbregistrering* och *skapa anpassade*.

Den här artikeln förklarar hur toouse hello Snabbregistrering metoden toocreate en ny molntjänst och sedan använda **överför** tooupload och distribuera ett paket med cloud service i Azure. När du använder den här metoden gör tillgängliga praktiska länkar för att slutföra alla krav när du går hello Azure-portalen. Om du är klar toodeploy ditt moln tjänst när du har skapat den, kan du göra både på hello samtidigt med skapa anpassade.

> [!NOTE]
> Om du planerar toopublish Molntjänsten från Visual Studio Team Services VSTS (), använda Snabbregistrering och sedan konfigurera VSTS publicering hello Azure Quickstart eller hello instrumentpanel. Mer information finns i [kontinuerlig leverans tooAzure med hjälp av Visual Studio Team Services][TFSTutorialForCloudService], eller se hjälp för hello **Snabbstart** sidan.
>
>

## <a name="concepts"></a>Koncept
Nödvändiga toodeploy ett program som en tjänst i molnet i Azure består av tre komponenter:

* **Tjänstdefinitionen**  
  hello molnet tjänstdefinitionsfilen (.csdef) definierar hello modell, inklusive hello antal roller.
* **Tjänstkonfiguration**  
  hello molnet tjänstekonfigurationsfilen (.cscfg) innehåller konfigurationsinställningar för hello Molntjänsten och enskilda roller, inklusive hello antalet rollinstanser.
* **Tjänstpaket**  
  hello service-paketet (.cspkg) innehåller hello programkod och konfigurationer och hello tjänstdefinitionsfilen.

Du kan lära dig mer om de här och hur toocreate ett paket [här](cloud-services-model-and-package.md).

## <a name="prepare-your-app"></a>Förbereda din app
Innan du kan distribuera en tjänst i molnet, måste du skapa hello cloud service-paketet (.cspkg) från din programkod och ett moln tjänstekonfigurationsfilen (.cscfg). hello Azure SDK innehåller verktyg för att förbereda filerna obligatorisk distribution. Du kan installera hello SDK från hello [Azure hämtar](https://azure.microsoft.com/downloads/) hello språk som du föredrar toodevelop sidan programkoden.

Tre kräver cloud tjänsten särskilda konfigurationer innan du exporterar en tjänstpaket:

* Om du vill toodeploy en molnbaserad tjänst som använder Secure Sockets Layer (SSL) för kryptering av data, [konfigurera ditt program](cloud-services-configure-ssl-certificate-portal.md#modify) för SSL.
* Om du vill tooconfigure Fjärrskrivbord anslutningar toorole instanser [konfigurerar hello roller](cloud-services-role-enable-remote-desktop-new-portal.md) för fjärrskrivbord.
* Om du vill tooconfigure utförlig övervakning för Molntjänsten, kan du aktivera Azure-diagnostik för hello-Molntjänsten. *Minimal övervakning* (hello standardinställningar för övervakning nivå) använder prestandaräknare som samlats in från hello värdoperativsystem för rollinstanser (virtuella datorer). *Utförlig övervakning* samlar in ytterligare mått baserat på prestandadata i hello rollen instanser tooenable närmare analys av problem som uppstår under bearbetningen av programmet. toofind reda på hur Azure-diagnostik för tooenable finns [aktiverar diagnostik i Azure](cloud-services-dotnet-diagnostics.md).

toocreate en molntjänst med distributioner av webbroller eller arbetsroller, måste du [skapa hello servicepaket](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Innan du börjar
* Om du inte har installerat hello Azure SDK, klickar du på **installera Azure SDK** tooopen hello [Azure hämtar sidan](https://azure.microsoft.com/downloads/), och sedan hämta hello SDK för hello språk som du föredrar toodevelop din kod. (Du har en möjlighet toodo detta senare.)
* Om alla rollinstanser kräver ett certifikat, skapa hello certifikat. Molntjänster kräver en .pfx-fil med en privat nyckel. Du kan överföra hello certifikat tooAzure som du skapar och distribuerar hello-Molntjänsten.

## <a name="create-and-deploy"></a>Skapa och distribuera
1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **New > Compute**, och bläddrar sedan nedåt tooand Klicka **Molntjänsten**.

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/create-cloud-service.png)
3. I nya hello **Molntjänsten** bladet, ange ett värde för hello **DNS-namnet**.
4. Skapa en ny **resursgruppen** eller välj en befintlig.
5. Välj en **Plats**.
6. Klicka på **paketet**. Då öppnas hello **överföra ett paket** bladet. Fyll i hello krävs. Om någon av dina roller innehåller en enda instans, kontrollerar du **distribuera även om en eller flera roller innehåller en enda instans** är markerad.
7. Se till att **starta distribution** är markerad.
8. Klicka på **OK** som stänger hello **överföra ett paket** bladet.
9. Om du inte har några certifikat tooadd klickar du på **skapa**.

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/select-package.png)

## <a name="upload-a-certificate"></a>Överföra ett certifikat
Om din distributionspaketet [konfigurerats toouse certifikat](cloud-services-configure-ssl-certificate-portal.md#modify), kan du överföra hello certifikat nu.

1. Välj **certifikat**, och på hello **lägga till certifikat** bladet välj hello SSL-certifikat PFX-fil och ange sedan hello **lösenord** för hello certifikat
2. Klicka på **bifoga certifikat**, och klicka sedan på **OK** på hello **lägga till certifikat** bladet.
3. Klicka på **skapa** på hello **Molntjänsten** bladet. När hello distribution har nått hello **klar** status, kan du fortsätta toohello nästa steg.

    ![Publicera din molntjänst](media/cloud-services-how-to-create-deploy-portal/attach-cert.png)

## <a name="verify-your-deployment-completed-successfully"></a>Kontrollera distributionen har slutförts
1. Klicka på hello cloud service-instans.

    hello status ska visa att hello-tjänsten är **kör**.
2. Under **Essentials**, klicka på hello **Webbadress** tooopen ditt moln-tjänsten i en webbläsare.

    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy-portal/running.png)

[TFSTutorialForCloudService]: http://go.microsoft.com/fwlink/?LinkID=251796

## <a name="next-steps"></a>Nästa steg
* [Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).
* Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).
* [Hantera din molntjänst](cloud-services-how-to-manage-portal.md).
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).
