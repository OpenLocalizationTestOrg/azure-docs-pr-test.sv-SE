---
title: "aaaHow toocreate och distribuera en tjänst i molnet | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera en molnbaserad tjänst som använder metoden Snabbregistrering för hello i Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 0ea78ccc-5e7d-40f8-bdb6-478c0eb0e265
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 09f889f81ccee6e8a7657116183aa4100410214c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-deploy-a-cloud-service"></a>Hur tooCreate och distribuera en tjänst i molnet
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
> * [Klassisk Azure-portal](cloud-services-how-to-create-deploy.md)
> 
> 

hello klassiska Azure-portalen ger dig två sätt du toocreate och distribuera en tjänst i molnet: **Snabbregistrering** och **skapa anpassade**.

Det här avsnittet beskrivs hur toouse hello Snabbregistrering metoden toocreate en ny molntjänst och sedan använda **överför** tooupload och distribuera ett paket med cloud service i Azure. När du använder den här metoden gör tillgängliga praktiska länkar för att slutföra alla krav när du går hello klassiska Azure-portalen. Om du är klar toodeploy ditt moln tjänst när du har skapat den, kan du göra både på hello samma gång med hjälp av **skapa anpassade**.

> [!NOTE]
> Om du planerar toopublish Molntjänsten från Visual Studio Team Services VSTS () använda **Snabbregistrering**, och sedan konfigurera VSTS publicering från **Snabbstart** eller hello instrumentpanel.
> 
> 

## <a name="concepts"></a>Koncept
Tre komponenter som krävs i ordning toodeploy ett program som ett moln-tjänsten i Azure:

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

* Om du vill toodeploy en molnbaserad tjänst som använder Secure Sockets Layer (SSL) för kryptering av data, [konfigurera ditt program](cloud-services-configure-ssl-certificate.md#step-2-modify-the-service-definition-and-configuration-files) för SSL.
* Om du vill tooconfigure Fjärrskrivbord anslutningar toorole instanser [konfigurerar hello roller](cloud-services-role-enable-remote-desktop.md) för fjärrskrivbord.
* Om du vill tooconfigure utförlig övervakning för Molntjänsten, kan du aktivera Azure-diagnostik för hello-Molntjänsten. *Minimal övervakning* (hello standardinställningar för övervakning nivå) använder prestandaräknare som samlats in från hello värdoperativsystem för rollinstanser (virtuella datorer). ”Utförlig övervakning * samlar in ytterligare mått baserat på prestandadata i hello rollen instanser tooenable närmare analys av problem som uppstår under bearbetningen av programmet. toofind reda på hur Azure-diagnostik för tooenable finns [aktiverar diagnostik i Azure](cloud-services-dotnet-diagnostics.md).

toocreate en molntjänst med distributioner av webbroller eller arbetsroller, måste du [skapa hello servicepaket](cloud-services-model-and-package.md#servicepackagecspkg).

## <a name="before-you-begin"></a>Innan du börjar
* Om du inte har installerat hello Azure SDK, klickar du på **installera Azure SDK** tooopen hello [Azure hämtar sidan](https://azure.microsoft.com/downloads/), och sedan hämta hello SDK för hello språk som du föredrar toodevelop din kod. (Du har en möjlighet toodo detta senare.)
* Om alla rollinstanser kräver ett certifikat, skapa hello certifikat. Molntjänster kräver en .pfx-fil med en privat nyckel. Du kan [överför hello certifikat tooAzure](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) som du skapar och distribuerar hello-Molntjänsten.
* Om du planerar toodeploy hello cloud service tooan tillhörighetsgrupp skapa hello tillhörighetsgrupp. Du kan använda en tillhörighetsgrupp toodeploy Molntjänsten och andra Azure-tjänster toohello samma plats i en region. Du kan skapa hello tillhörighetsgrupp i hello **nätverk** område i hello klassiska Azure-portalen på hello **Tillhörighetsgrupper** sidan.

## <a name="how-to-create-a-cloud-service-using-quick-create"></a>Så här: skapa en molntjänst med Snabbregistrering
1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **ny**>**Compute**>**Molntjänsten** > **Snabbregistrering**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_QuickCreate.png)
2. I **URL**, ange en underdomän namn toouse i hello offentlig URL för åtkomst till Molntjänsten för produktionsdistributioner. hello URL-formatet för Produktionsdistribution: http://*myURL*. cloudapp.net.
3. I **Region eller Affinitetsgrupp**, Välj hello geografisk region eller affinitetsgrupp toodeploy hello molntjänst i. Välj en tillhörighetsgrupp om du vill toodeploy din cloud service toohello samma plats som andra Azure-tjänster inom en region.
4. Klicka på **Create Cloud Service** (Skapa molntjänst).
   
    ![CloudServices_Region](./media/cloud-services-how-to-create-deploy/CloudServices_Regionlist.png)
   
    Du kan övervaka hello status för hello processen i hello meddelandeområdet längst ned hello hello-fönstret.
   
    Hej **molntjänster** område öppnas med hello ny molntjänst visas. När hello status ändras tooCreated, har för att skapa moln slutförts.
   
    ![CloudServices_CloudServicesPage](./media/cloud-services-how-to-create-deploy/CloudServices_CloudServicesPage.png)

## <a name="how-to-upload-a-certificate-for-a-cloud-service"></a>Så här: ladda upp ett certifikat för en tjänst i molnet
1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **molntjänster**hello namnet på hello-Molntjänsten och klicka sedan på **certifikat**.
   
    ![CloudServices_QuickCreate](./media/cloud-services-how-to-create-deploy/CloudServices_EmptyDashboard.png)
2. Klicka på antingen **överföra ett certifikat** eller **överför**.
3. I **filen**, använda **Bläddra** tooselect hello certifikatet (.pfx-filen).
4. I **lösenord**, ange hello privata nyckeln för hello certifikatet.
5. Klicka på **OK** (markering).
   
    ![CloudServices_AddaCertificate](./media/cloud-services-how-to-create-deploy/CloudServices_AddaCertificate.png)
   
    Du kan titta på hello fortskrider hello uppladdning hello meddelandeområdet visas nedan. När hello överföringen har slutförts hello certifikat till toohello tabell. Klicka på OK tooclose hello-meddelande i hello meddelandet.
   
    ![CloudServices_CertificateProgress](./media/cloud-services-how-to-create-deploy/CloudServices_CertificateProgress.png)

## <a name="how-to-deploy-a-cloud-service"></a>Så här: distribuera en tjänst i molnet
1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **molntjänster**hello namnet på hello-Molntjänsten och klicka sedan på **instrumentpanelen**.
2. Klicka på antingen **ladda upp en ny Produktionsdistribution** eller **överför**.
3. I **distributionsetiketten**, ange ett namn för hello ny distribution – till exempel MyCloudServicev4.
4. I **paketet**, använda **Bläddra** tooselect hello service paketet (.cspkg)-fil toouse.
5. I **Configuration**, använda **Bläddra** tooselect hello service konfigurera toouse fil (.cscfg).
6. Om hello Molntjänsten innehåller några roller med endast en instans, väljer hello **distribuera även om en eller flera roller innehåller en enda instans** kryssrutan tooenable hello distribution tooproceed.
   
    Azure kan bara garantera 99,95% åtkomst toohello Molntjänsten vid underhåll och service uppdateringar om varje roll har minst två instanser. Om det behövs kan du lägga till ytterligare rollinstanser på hello **skala** sidan när du har distribuerat hello-Molntjänsten. Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).
7. Klicka på **OK** (markering) toobegin hello cloud service-distributionen.
   
    ![CloudServices_UploadaPackage](./media/cloud-services-how-to-create-deploy/CloudServices_UploadaPackage.png)
   
    Du kan övervaka hello distribution i meddelandeområdet hello hello status. Klicka på OK toohide hello-meddelande.
   
    ![CloudServices_UploadProgress](./media/cloud-services-how-to-create-deploy/CloudServices_UploadProgress.png)

## <a name="verify-your-deployment-completed-successfully"></a>Kontrollera distributionen har slutförts
1. Klicka på **instrumentpanelen**.
   
    hello status ska visa att hello-tjänsten är **kör**.
2. Under **snabböversikten**, klicka på hello plats URL tooopen Molntjänsten i en webbläsare.
   
    ![CloudServices_QuickGlance](./media/cloud-services-how-to-create-deploy/CloudServices_QuickGlance.png)


## <a name="next-steps"></a>Nästa steg
* [Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure.md).
* Konfigurera en [domännamn](cloud-services-custom-domain-name.md).
* [Hantera din molntjänst](cloud-services-how-to-manage.md).
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate.md).

