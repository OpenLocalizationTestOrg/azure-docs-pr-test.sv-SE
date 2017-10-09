---
title: aaaManage Azure Traffic Manager-profiler | Microsoft Docs
description: "Den här artikeln beskriver hur du skapar, inaktiverar, aktiverar och tar bort en Azure Traffic Manager-profil."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: kumud
ms.openlocfilehash: 0c6ab0c451581d039514a9de0b525b3937e45a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-traffic-manager-profile"></a>Hantera en Azure Traffic Manager-profil

Traffic Manager-profiler använder Routning av nätverkstrafik metoder toocontrol hello fördelning av trafik tooyour molntjänster eller webbplats slutpunkter. Den här artikeln förklarar hur toocreate och hantera profilerna.

## <a name="create-a-traffic-manager-profile"></a>Skapa en Traffic Manager-profil

Du kan skapa en Traffic Manager-profil med hjälp av hello Azure-portalen. När du har skapat din profil, kan du konfigurera slutpunkter, övervakning och andra inställningar i hello Azure-portalen. Traffic Manager stöder upp too200 slutpunkter per profil. De flesta användningsscenarier kräver dock endast ett litet antal slutpunkter.

### <a name="toocreate-a-traffic-manager-profile"></a>toocreate en trafikhanterarprofil

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com). Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/). 
2. På hello **hubb** -menyn klickar du på **ny** > **nätverk** > **se alla**, klickar du på **trafik Manager** profil tooopen hello **skapa Traffic Manager-profilen** bladet Klicka **skapa**.
3. På hello **skapa Traffic Manager-profilen** bladet slutförts enligt följande:
    1. Ge profilen ett namn i **Namn**. Det här namnet måste toobe unika inom hello trafficmanager.net zon och resulterar i hello DNS-namnet <name>, trafficmanager.net som används tooaccess Traffic Manager-profilen.
    2. I **routningsmetod**väljer hello **prioritet** routningsmetoden.
    3. I **prenumeration**, Välj hello-prenumeration som du vill att toocreate profilen under
    4. I **resursgruppen**, och skapa en ny resurs grupp tooplace profilen under.
    5. I **resursgruppsplats**, Välj hello plats hello resursgruppen. Den här inställningen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello Traffic Manager-profilen som ska distribueras globalt.
    6. Klicka på **Skapa**.
    7. När hello globala distribution av Traffic Manager-profilen är klar visas den i respektive resursgruppen som en hello resurser.

## <a name="disable-enable-or-delete-a-profile"></a>Inaktivera, aktivera eller ta bort en profil

Du kan inaktivera en befintlig profil så att trafik Manager inte refererar användaren begäranden toohello konfigurerade slutpunkter. När du inaktiverar en Traffic Manager-profil hello profil och hello information i hello profil förblir intakta och kan redigeras i hello Traffic Manager-gränssnittet.  Referenser återuppta när du återaktivera hello profil. När du skapar en Traffic Manager-profilen i hello Azure-portalen aktiveras den automatiskt. Om en bestämmer att en profil inte längre behövs, kan du ta bort den.

### <a name="toodisable-a-profile"></a>toodisable en profil

1. Om du använder ett anpassat domännamn, ändra hello CNAME-post på Internet-DNS-servern så att den inte längre pekar tooyour Traffic Manager-profilen.
2. Trafiken slutar att dirigeras toohello slutpunkterna via hello Traffic Manager-profilinställningarna.
3. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).
2. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn som du vill toomodify och klicka sedan på hello Traffic Manager-profilen i hello resulterar det hello visas.
3. I hello **trafikhanterarprofil** bladet klickar du på **översikt**, hello översikt-bladet klickar du på **inaktivera**, och sedan bekräfta toodisable hello Traffic Manager-profilen.

### <a name="tooenable-a-profile"></a>tooenable en profil

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).
2. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn som du vill toomodify och klicka sedan på hello Traffic Manager-profilen i hello resulterar det hello visas.
3. I hello **trafikhanterarprofil** bladet, klickar du på **översikt**, och klicka sedan på i hello översikt bladet **aktivera**.
5. Om du använder ett anpassat domännamn kan du skapa en CNAME-resursposten på din Internet-DNS-server toopoint toohello domännamnet för Traffic Manager-profilen.
6. Trafiken är riktat toohello slutpunkter igen.

### <a name="toodelete-a-profile"></a>toodelete en profil

1. Kontrollera att hello DNS-resursposten på Internet-DNS-servern inte längre använder en CNAME-resurspost som pekar toohello domännamnet för Traffic Manager-profilen.
2. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn som du vill toomodify och klicka sedan på hello Traffic Manager-profilen i hello resulterar det hello visas.
3. I hello **trafikhanterarprofil** bladet klickar du på **översikt**, hello översikt-bladet klickar du på **ta bort**, och sedan bekräfta toodelete hello Traffic Manager-profilen.

## <a name="next-steps"></a>Nästa steg

* [Lägg till en slutpunkt](traffic-manager-endpoints.md)
* [Konfigurera prioriterad routningsmetod](traffic-manager-configure-priority-routing-method.md)
* [Konfigurera geografisk routningsmetod](traffic-manager-configure-geographic-routing-method.md) 
* [Konfigurera viktad routningsmetod](traffic-manager-configure-weighted-routing-method.md)
* [Konfigurera routningsmetod för prestanda](traffic-manager-configure-performance-routing-method.md)
