---
title: "aaaHow tooconfigure en tjänst i molnet (portal) | Microsoft Docs"
description: "Lär dig hur tooconfigure cloud services i Azure. Lär dig tjänstkonfigurationen för tooupdate hello molnet och konfigurera fjärråtkomst toorole instanser. Dessa exempel används hello Azure-portalen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: 969a08558473e8c79153192942bfda587eb5ada5
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

Du kan konfigurera hello de vanligaste inställningarna för en tjänst i molnet i hello Azure-portalen. Eller, om du vill tooupdate konfigurationsfilerna direkt, hämta en service configuration file tooupdate och överför hello uppdateras filen och uppdatera hello Molntjänsten hello konfigurationsändringar. Oavsett hur push hello konfigurationsuppdateringar-installeras tooall rollinstanser.

Du kan också hantera hello instanser av dina molntjänstroller eller fjärrskrivbord i dem.

Azure kan bara kontrollera 99,95% tjänsttillgänglighet under hello konfigurationsuppdateringar om du har minst två rollinstanser för varje roll. Som gör att en virtuell dator tooprocess klientbegäranden medan hello andra uppdateras. Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Ändra en tjänst i molnet
När du öppnar hello [Azure-portalen](https://portal.azure.com/), navigera tooyour Molntjänsten. Härifrån kan hantera du många aspekter av den.

![Inställningssidan](./media/cloud-services-how-to-configure-portal/cloud-service.png)

Hej **inställningar** eller **alla inställningar** länkar öppnas hello **inställningar** bladet där du kan ändra hello **egenskaper**, ändra hello **Configuration**, hantera hello **certifikat**, installationsprogrammet **Varna regler**, och hantera hello **användare** som har åtkomst toothis Molntjänsten.

![Azure cloud service inställningsbladet](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Hantera gäst-OS-version

Som standard uppdaterar Azure regelbundet gäst OS toohello senaste stöds bilden i hello OS-familj som du har angett i tjänstkonfigurationen (.cscfg), till exempel Windows Server 2016.

Om du behöver tootarget en viss OS-version, du kan ange i hello **Configuration** bladet.

![Ange OS-version](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> Om du väljer en viss OS-version inaktiverar automatisk OS uppdateringar och gör korrigering ditt ansvar. Du måste se till att dina rollinstanser tar emot uppdateringar eller du kan visa dina program toosecurity säkerhetsproblem.

## <a name="monitoring"></a>Övervakning
Du kan lägga till aviseringar tooyour Molntjänsten. Klicka på **inställningar** > **avisering regler** > **Lägg till avisering**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Härifrån kan konfigurera du en avisering. Med hello **mått** listrutan, kan du konfigurera en avisering om hello följande typer av data.

* Disk-lästa
* Skrivning till disk
* Nätverk i
* Nätverk ut
* CPU-procent

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Konfigurera övervakning från en mått panel
Istället för att använda **inställningar** > **Varningsregler**, kan du klicka på någon av hello mått paneler i hello **övervakning** avsnitt i hello **moln tjänsten** bladet.

![Molntjänsten övervakning](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Härifrån kan anpassa hello diagram används med hello panelen eller lägga till en varningsregel.

## <a name="reboot-reimage-or-remote-desktop"></a>Omstart eller avbildningsåterställning fjärrskrivbord
Du kan inte konfigurera Fjärrskrivbord med hello just nu **Azure-portalen**. Men du kan konfigurera det via hello [klassiska Azure-portalen](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), eller via [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).

Klicka först på hello cloud service-instans.

![Cloud Service-instans](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Från hello bladet som öppnar du initiera en fjärrskrivbordsanslutning, starta om hello instans, eller via fjärranslutning avbildningsåterställning (starta med en ny avbildning) hello via fjärranslutning.

![Cloud Service-instans knappar](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>Konfigurera om din .cscfg
Du kanske måste tooreconfigure Molntjänsten via hello [service config (cscfg)](cloud-services-model-and-package.md#cscfg) fil. Du måste först toodownload din .cscfg-filen, ändra den och sedan ladda upp den.

1. Klicka på hello **inställningar** ikonen eller hello **alla inställningar** länka tooopen in hello **inställningar** bladet.

    ![Inställningssidan](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Klicka på hello **Configuration** objekt.

    ![Konfiguration av bladet](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Klicka på hello **hämta** knappen.

    ![Ladda ned](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. När du har uppdaterat hello-tjänstkonfigurationsfil ladda upp och tillämpa hello configuration uppdateringar:

    ![Ladda upp](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. Välj hello .cscfg-filen och klicka på **OK**.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).
* Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).
* [Hantera din molntjänst](cloud-services-how-to-manage-portal.md).
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).
