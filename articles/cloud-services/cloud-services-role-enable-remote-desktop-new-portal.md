---
title: "aaaEnable anslutning till fjärrskrivbord för en roll i Azure Cloud Services | Microsoft Docs"
description: "Hur tooconfigure din azure cloud service programmet tooallow anslutningar till fjärrskrivbord"
services: cloud-services
documentationcenter: 
author: mmccrory
manager: timlt
editor: 
ms.assetid: 73ea1d64-1529-4d72-b58e-f6c10499e6bb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: mmccrory
ms.openlocfilehash: 55d7043df571c2e88b04aa9ef01dc8ae1d6784f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Aktivera anslutning till fjärrskrivbord för en roll i Azure-molntjänster
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klassisk Azure-portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

Fjärrskrivbord kan du tooaccess hello skrivbordet för en roll som körs i Azure. Du kan använda en Fjärrskrivbord anslutning tootroubleshoot och diagnostisera problem med ditt program när den körs.

Du kan aktivera anslutning till fjärrskrivbord i din roll under utvecklingen av inklusive hello fjärrskrivbord moduler i tjänstedefinitionsfilen eller välja tooenable fjärrskrivbord via hello Remote Desktop-tillägget. hello är föredragen metod toouse hello Remote Desktop-tillägg som du kan aktivera fjärrskrivbord även efter hello programmet distribueras utan att behöva tooredeploy ditt program.

## <a name="configure-remote-desktop-from-hello-azure-portal"></a>Konfigurera anslutning till fjärrskrivbord från hello Azure-portalen
hello Azure-portalen använder hello Remote Desktop-tillägget metoden så att du kan aktivera fjärrskrivbord även efter hello programmet distribueras. Hej **fjärrskrivbord** bladet för din molntjänst kan du tooenable fjärrskrivbord, ändra hello lokala administratörskontot används tooconnect toohello virtuella datorer, hello certifikat används för autentisering och ange hello utgångsdatum.

1. Klicka på **molntjänster**hello namnet på hello-Molntjänsten och klicka sedan på **fjärrskrivbord**.

    ![Cloud services fjärrskrivbord](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Välj om du vill tooenable Remote Desktop för en enskild roll eller för alla roller och sedan ändra hello värdet för hello switcher för**aktiverad**.

3. Fyll i hello som krävs för användarnamn, lösenord, upphör att gälla och certifikat.

    ![Cloud services fjärrskrivbord](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Alla rollinstanser kommer att startas om när du först aktivera Fjärrskrivbord och klicka på OK (markering). tooprevent en omstart, hello certifikatlösenord används tooencrypt hello måste installeras på hello roll. tooprevent en omstart [överföra ett certifikat för hello Molntjänsten](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) och returnerar sedan toothis dialogrutan.
   >
   >
3. I **roller**väljer hello rollen tooupdate eller välj **alla** för alla roller.

4. När du är klar configuration-uppdateringar, klickar du på **spara**. Det tar en stund innan dina rollinstanser är klar tooreceive anslutningar.

## <a name="remote-into-role-instances"></a>Remote i rollinstanser
När fjärrskrivbord har aktiverats på hello roller, kan du upprätta en anslutning direkt från hello Azure-portalen:

1. Klicka på **instanser** tooopen hello **instanser** bladet.
2. Välj en rollinstans som har konfigurerats för fjärrskrivbord.
3. Klicka på **Anslut** toodownload en RDP-filen för hälsningspaket rollinstansen.

    ![Cloud services fjärrskrivbord](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Klicka på **öppna** och sedan **Anslut** toostart hello fjärrskrivbordsanslutning.

>[!NOTE]
> Om din molntjänst placerad bakom en NSG, måste du kanske toocreate regler som tillåter trafik på portarna **3389** och **20000**.  Fjärrskrivbord använder port **3389**.  Molnet tjänstinstanser belastningsutjämnas, så du inte kan styra vilken instans tooconnect till direkt.  Hej *RemoteForwarder* och *RemoteAccess* agenter hantera RDP-trafik och Tillåt hello klienten toosend en RDP-cookie och ange en enskild instans tooconnect till.  Hej *RemoteForwarder* och *RemoteAccess* agenter kräver att port **20000*** öppnas, där blockeras om du har en NSG.

## <a name="additional-resources"></a>Ytterligare resurser

[Hur tooConfigure molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)
