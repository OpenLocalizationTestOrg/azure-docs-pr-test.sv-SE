---
title: aaaCreate Traffic Manager-profilen i Azure | Microsoft Docs
description: "Den här artikeln beskriver hur toocreate Traffic Manager-profilen"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: kumud
ms.openlocfilehash: 5cd3d2903552c9a0427da41a73f6f38f6b0afc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-traffic-manager-profile"></a>Skapa en Traffic Manager-profil

Den här artikeln beskrivs hur en profil med **prioritet** routning kan skapas tooroute användare tootwo Azure Web Apps slutpunkter. Med hjälp av hello **prioritet** routning typen all trafik är routade toohello första slutpunkten medan hello andra sparas som en säkerhetskopia. Användare kan därför vara routade toohello andra slutpunkten om hello första slutpunkten blir ohälsosamt.

I den här artikeln är två tidigare skapade Azure Web App slutpunkter associerade toothis nyskapad Traffic Manager-profilen. Mer om hur toolearn toocreate Azure Web App slutpunkter finns hello [dokumentationssidan för Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/). Du kan lägga till någon slutpunkt som har ett DNS-namn och kan nås via hello offentliga internet och att vi använder Azure Web Apps slutpunkter som ett exempel.

### <a name="create-a-traffic-manager-profile"></a>Skapa en Traffic Manager-profil
1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com). Om du inte redan har ett konto, kan du registrera en [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/free/). 
2. På hello **hubb** -menyn klickar du på **ny** > **nätverk** > **se alla**, klickar du på **trafik Manager** profil tooopen hello **skapa Traffic Manager-profilen** bladet Klicka **skapa**.
3. På hello **skapa Traffic Manager-profilen** bladet slutförts enligt följande:
    1. Ge profilen ett namn i **Namn**. Det här namnet måste toobe unika inom hello trafficmanager.net zon och resulterar i hello DNS-namnet <name>, trafficmanager.net som används tooaccess Traffic Manager-profilen.
    2. I **routningsmetod**väljer hello **prioritet** routningsmetoden.
    3. I **prenumeration**, Välj hello-prenumeration som du vill att toocreate profilen under
    4. I **resursgruppen**, och skapa en ny resurs grupp tooplace profilen under.
    5. I **resursgruppsplats**, Välj hello plats hello resursgruppen. Den här inställningen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello Traffic Manager-profilen som ska distribueras globalt.
    6. Klicka på **Skapa**.
    7. När hello globala distribution av Traffic Manager-profilen är klar visas den i respektive resursgruppen som en hello resurser.

    ![Skapa en Traffic Manager-profil](./media/traffic-manager-create-profile/Create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Lägga till slutpunkter för Traffic Manager

1. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn som du skapade i föregående avsnitt och klickar på hello traffic manager-profil i hello hello resulterar det hello visas.
2. I hello **trafikhanterarprofil** bladet i hello **inställningar** klickar du på **slutpunkter**.
3. I hello **slutpunkter** bladet som visas, klickar du på **Lägg till**.
4. I hello **lägga till slutpunkten** bladet slutförts enligt följande:
    1. Klicka på **Azure-slutpunkt** för **Typ**.
    2. Ange en **namn** som du vill använda toorecognize den här slutpunkten.
    3. För **mål resurstypen**, klickar du på **Apptjänst**.
    4. För **mål resurs**, klickar du på **väljer du en apptjänst** tooshow hello lista över hello Web Apps under hello samma prenumeration. I hello **resurs** bladet som visas, Välj hello apptjänst som du vill tooadd som hello första slutpunkten.
    5. För **Prioritet**väljer du **1**. Detta resulterar i all trafik toothis endpoint om den är felfri.
    6. Behåll **Lägg till som inaktiverad** som avmarkerat.
    7. Klicka på **OK**
5.  Upprepa steg 3 och 4 för hello nästa Azure Web Apps slutpunkt. Se till att tooadd med dess **prioritet** värdet på **2**.
6.  När hello lägga till båda slutpunkterna är klar, visas de i hello **trafikhanterarprofil** bladet tillsammans med deras övervakning status som **Online**.

    ![Lägg till en Traffic Manager-slutpunkt](./media/traffic-manager-create-profile/add-traffic-manager-endpoint.png)

## <a name="use-hello-traffic-manager-profile"></a>Använd hello Traffic Manager-profilen
1.  I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn som du skapade i föregående avsnitt hello. I hello resultat som visas, klickar du på hello traffic manager-profil.
2. I hello **trafikhanterarprofil** bladet, klickar du på **översikt**.
3. Hej **trafikhanterarprofil** bladet visar hello DNS-namnet för nyskapade Traffic Manager-profilen. Detta kan användas av alla klienter (till exempel genom att gå tooit med en webbläsare) tooget dirigeras toohello rätt slutpunkten enligt hello routning. I det här fallet alla förfrågningar är routade toohello första slutpunkten och om Traffic Manager identifierar den vara ohälsosamt misslyckas hello trafik automatiskt över toohello nästa slutpunkt.

## <a name="delete-hello-traffic-manager-profile"></a>Ta bort hello trafikhanterarprofil
När det inte längre behövs tar du bort hello resursgrupp och hello Traffic Manager-profil som du har skapat. Välj toodo därför hello resursgruppen från hello **trafikhanterarprofil** bladet och klicka på **ta bort**.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [typer av routing](traffic-manager-routing-methods.md).
- Mer information om typer av slutpunkter [slutpunktstyper](traffic-manager-endpoint-types.md).
- Lär dig mer om [slutpunktsövervakning](traffic-manager-monitoring.md).



