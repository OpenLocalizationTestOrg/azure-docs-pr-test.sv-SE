---
title: "aaaHow tooretain en konstant virtuella IP-adressen för en Azure-molntjänst | Microsoft Docs"
description: "Lär dig hur ändras inte tooensure som hello virtuella IP-adressen (VIP) för Azure-Molntjänsten."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4a58e2c6-7a79-4051-8a2c-99182ff8b881
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 9e27121797ffb61517b8d2c2661ec44ff7298968
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retain-a-constant-virtual-ip-address-for-an-azure-cloud-service"></a>Bevara ett konstant virtuella IP-adressen för en Azure-molntjänst
Du kanske måste tooensure hello virtuell IP-adress (VIP) för hello-tjänsten inte ändras när du uppdaterar en molnbaserad tjänst som finns i Azure. Många domän hanteringstjänster använda hello Domain Name System (DNS) för att registrera domännamn. DNS fungerar bara om hello VIP förblir hello samma. Du kan använda hello **Publiceringsguiden** i Azure-verktyg tooensure som hello VIP för Molntjänsten ändras inte när du uppdaterar den. Mer information om hur toouse DNS-domän management för molntjänster, se [konfigurera ett anpassat domännamn för en Azure-molntjänst](cloud-services/cloud-services-custom-domain-name.md).

## <a name="publish-a-cloud-service-without-changing-its-vip"></a>Publicera en tjänst i molnet utan att ändra dess VIP
hello VIP av molntjänster har allokerats när du först distribuera den tooAzure i en viss miljö, till exempel hello-produktionsmiljö. hello VIP ändringar endast om du tar bort hello distribution explicit eller hello distributionen är implicit bort hello distribution av uppdateringsprocessen. tooretain hello VIP måste du inte ta bort distributionen och måste du se till att Visual Studio inte ta bort distributionen automatiskt. 

Du kan ange inställningar för distribution i hello **Publiceringsguiden**, som stöder flera distributionsalternativ. Du kan ange en ny distribution eller uppdatera-distributioner som kan vara inkrementell eller samtidig. Båda typerna av distribution behålla hello VIP. Definitioner för dessa olika typer av distribution finns i [Publiceringsguiden för Azure-program](vs-azure-tools-publish-azure-application-wizard.md). Du kan dessutom styra hello tidigare distribution av en molntjänst tas bort om ett fel inträffar. Om du inte alternativet korrekt ändras hello VIP oväntat.

## <a name="update-a-cloud-service-without-changing-its-vip"></a>Uppdatera en tjänst i molnet utan att ändra dess VIP
1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio. 

2. I **Solution Explorer**, högerklicka på hello projektet. Markera på snabbmenyn hello **publicera**.

    ![Menyn Publish (Publicera)](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/solution-explorer-publish-menu.png)

3. I hello **publicera Azure-programmet** dialogrutan, Välj hello Azure-prenumeration toowhich som du vill toodeploy. Logga in om nödvändigt och välj **nästa**.

    ![Publicera Azure program inloggningssida](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-signin.png)

4. På hello **gemensamma inställningar för** kontrollerar du att hello namnet på hello cloud service toowhich du distribuerar, hello **miljö**, hello **versionskonfiguration**, och hello **Tjänstkonfiguration** är korrekta.

    ![Publicera gemensamma inställningar för Azure-program](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-common-settings.png)

5. På hello **avancerade inställningar** kontrollerar du att hello **distributionsetiketten** och hello **lagringskonto** är korrekta. Kontrollera att hello **ta bort distributionen på fel** kryssrutan är avmarkerad och kontrollera att hello **uppdatering av programdistribution** är markerad. Genom att avmarkera hello **ta bort distributionen på fel** kan du se till att din VIP försvinner inte om ett fel uppstår under distributionen. Genom att välja hello **uppdatering av programdistribution** kan du se till att distributionen tas inte bort och din VIP försvinner inte när du publicerar ditt program. 

    ![Publicera fliken Avancerade inställningar för Azure-program](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-advanced-settings.png)

6. toofurther anger du hur hello roller toobe uppdateras, Välj **inställningar** nästa för**uppdatering av programdistribution**. Välj antingen **stegvis uppdatering** eller **samtidig uppdatering**, och välj **OK**. Välj **stegvis uppdatering** tooupdate varje instans av ditt program efter varandra, så som hello programmet alltid är tillgängligt. Välj **samtidig uppdatering** tooupdate alla instanser av programmet under hello samtidigt. Samtidig uppdatering är snabbare, men tjänsten kanske inte är tillgänglig under hello uppdateringsprocessen. När du är klar väljer du **nästa**.

    ![Publicera Azure inställningar för programdistribution sida](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-deployment-update-settings.png)

7. I hello **publicera Azure-programmet** dialogrutan **nästa** tills hello **sammanfattning** visas. Verifiera inställningarna och välj sedan **publicera**.
   
    ![Publicera Azure program sammanfattningssida](./media/vs-azure-tools-cloud-service-retain-a-constant-virtual-ip-address/azure-publish-summary.png)

## <a name="next-steps"></a>Nästa steg
- [Med hjälp av hello Visual Studio Azure guiden Publicera program](vs-azure-tools-publish-azure-application-wizard.md)

