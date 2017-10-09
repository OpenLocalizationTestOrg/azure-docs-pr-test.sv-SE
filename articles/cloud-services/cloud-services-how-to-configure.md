---
title: "aaaHow tooconfigure en tjänst i molnet (klassiska portal) | Microsoft Docs"
description: "Lär dig hur tooconfigure cloud services i Azure. Lär dig tjänstkonfigurationen för tooupdate hello molnet och konfigurera fjärråtkomst toorole instanser."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 1ea2320f97f667153f7984e4d61d373a6344cf6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-cloud-services"></a>Hur tooConfigure Cloud Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-configure-portal.md)
> * [Klassisk Azure-portal](cloud-services-how-to-configure.md)
> 
> 

Du kan konfigurera hello de vanligaste inställningarna för en tjänst i molnet i hello klassiska Azure-portalen. Eller, om du vill tooupdate konfigurationsfilerna direkt, hämta en service configuration file tooupdate och överför hello uppdateras filen och uppdatera hello Molntjänsten hello konfigurationsändringar. Oavsett hur push hello konfigurationsuppdateringar-installeras tooall rollinstanser.

hello klassiska Azure-portalen kan du också för[aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)

Azure kan bara kontrollera 99,95% tjänsttillgänglighet under hello konfigurationsuppdateringar om du har minst två rollinstanser för varje roll. Som gör att en virtuell dator tooprocess klientbegäranden medan hello andra uppdateras. Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Ändra en tjänst i molnet
1. I hello [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **molntjänster**hello namnet på hello-Molntjänsten och klicka sedan på **konfigurera**.
   
    ![Konfigurationssidan](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    På hello **konfigurera** kan du konfigurera övervakning, uppdatering rollinställningar och välj hello gästoperativsystemet och familj för rollinstanser. 
2. I **övervakning**, ange hello övervakning nivå tooVerbose eller Minimal och konfigurera hello diagnostik anslutningssträngar som krävs för övervakning av utförlig.
3. Du kan uppdatera hello följande inställningar för tjänsten roller (grupperade efter roll):
   
    * **Inställningar för** ändra hello värden av olika konfigurationsinställningar som anges i hello *ConfigurationSettings* element i hello tjänstekonfigurationsfilen (.cscfg).

    * **Certifikat**  
        Ändra hello tumavtrycket för certifikatet som används i SSL-kryptering för en roll. toochange ett certifikat, måste du först överföra hello nytt certifikat (på hello **certifikat** sidan). Uppdatera hello tumavtrycket i hello certifikat strängen som visas i hello rollinställningar.
4. I **operativsystemet**, kan du ändra hello operativsystemsfamilj eller version för rollinstanser, eller välj **automatisk** tooenable automatiska uppdateringar av hello aktuella operativsystemets version. hello operativsysteminställningar gäller tooweb och arbetsroller, men påverkar inte virtuella datorer.
   
    Hello senaste version av operativsystemet är installerat på alla rollinstanser under distributionen och hello operativsystem uppdateras automatiskt som standard. 
   
    Om du behöver för din cloud service toorun på en annan operativsystemversion på grund av kompatibilitetskraven i koden, väljer du en operativsystemsfamilj och version. När du väljer en specifik operativsystemsversion har automatisk operativsystemuppdateringar för hello Molntjänsten avbrutits. Du behöver tooensure hello operativsystem ta emot uppdateringar.
   
    Om du har löst alla problem med programkompatibilitet som dina appar har med hello senaste version av operativsystemet, kan du aktivera automatisk operativsystemuppdateringar genom att ange hello Operativsystemversionen för**automatisk**. 
   
    ![OS-inställningar](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. toosave konfigurationsinställningarna, och push-installera dem toohello rollinstanser, klickar du på **spara**. (Klicka på **Ignorera** toocancel hello ändringar.) **Spara** och **Ignorera** läggs toohello kommandofältet när du ändrar en inställning.

## <a name="update-a-cloud-service-configuration-file"></a>Uppdatera en cloud service-konfigurationsfil
1. Hämta en cloud service konfigurationsfilen (.cscfg) med hello aktuella konfiguration. På hello **konfigurera** för hello Molntjänsten, klickar du på **hämta**. Klicka på **spara**, eller klicka på **Spara som** toosave hello-filen.
2. När du har uppdaterat hello-tjänstkonfigurationsfil ladda upp och tillämpa hello configuration uppdateringar:
   
   1. På hello **konfigurera** klickar du på **överför**.
      
       ![Överför konfiguration](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. I **konfigurationsfilen**, använda **Bläddra** tooselect hello uppdateras .cscfg-filen.
   3. Om din molntjänst innehåller de roller som har en enda instans, Välj hello **tillämpa konfigurationen även om en eller flera roller innehåller en enda instans** kryssrutan tooenable hello configuration uppdateringar för hello roller tooproceed.
      
       Om du inte definierar minst två instanser av varje roll, Azure kan inte garantera till minst 99,95% tillgänglighet för din molntjänst under service configuration uppdateringar. Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).
   4. Klicka på **OK** (markering). 

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md).
* Konfigurera en [domännamn](cloud-services-custom-domain-name.md).
* [Hantera din molntjänst](cloud-services-how-to-manage.md).
* [Aktivera anslutning till fjärrskrivbord för en roll i Azure-molntjänster](cloud-services-role-enable-remote-desktop.md)
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate.md).

