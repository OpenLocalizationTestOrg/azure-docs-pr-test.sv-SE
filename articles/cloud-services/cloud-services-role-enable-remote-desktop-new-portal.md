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
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a><span data-ttu-id="62fb8-103">Aktivera anslutning till fjärrskrivbord för en roll i Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="62fb8-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="62fb8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="62fb8-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="62fb8-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="62fb8-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="62fb8-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="62fb8-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="62fb8-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="62fb8-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="62fb8-108">Fjärrskrivbord kan du tooaccess hello skrivbordet för en roll som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="62fb8-108">Remote Desktop enables you tooaccess hello desktop of a role running in Azure.</span></span> <span data-ttu-id="62fb8-109">Du kan använda en Fjärrskrivbord anslutning tootroubleshoot och diagnostisera problem med ditt program när den körs.</span><span class="sxs-lookup"><span data-stu-id="62fb8-109">You can use a Remote Desktop connection tootroubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="62fb8-110">Du kan aktivera anslutning till fjärrskrivbord i din roll under utvecklingen av inklusive hello fjärrskrivbord moduler i tjänstedefinitionsfilen eller välja tooenable fjärrskrivbord via hello Remote Desktop-tillägget.</span><span class="sxs-lookup"><span data-stu-id="62fb8-110">You can enable a Remote Desktop connection in your role during development by including hello Remote Desktop modules in your service definition or you can choose tooenable Remote Desktop through hello Remote Desktop Extension.</span></span> <span data-ttu-id="62fb8-111">hello är föredragen metod toouse hello Remote Desktop-tillägg som du kan aktivera fjärrskrivbord även efter hello programmet distribueras utan att behöva tooredeploy ditt program.</span><span class="sxs-lookup"><span data-stu-id="62fb8-111">hello preferred approach is toouse hello Remote Desktop extension as you can enable Remote Desktop even after hello application is deployed without having tooredeploy your application.</span></span>

## <a name="configure-remote-desktop-from-hello-azure-portal"></a><span data-ttu-id="62fb8-112">Konfigurera anslutning till fjärrskrivbord från hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="62fb8-112">Configure Remote Desktop from hello Azure portal</span></span>
<span data-ttu-id="62fb8-113">hello Azure-portalen använder hello Remote Desktop-tillägget metoden så att du kan aktivera fjärrskrivbord även efter hello programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="62fb8-113">hello Azure portal uses hello Remote Desktop Extension approach so you can enable Remote Desktop even after hello application is deployed.</span></span> <span data-ttu-id="62fb8-114">Hej **fjärrskrivbord** bladet för din molntjänst kan du tooenable fjärrskrivbord, ändra hello lokala administratörskontot används tooconnect toohello virtuella datorer, hello certifikat används för autentisering och ange hello utgångsdatum.</span><span class="sxs-lookup"><span data-stu-id="62fb8-114">hello **Remote Desktop** blade for your cloud service allows you tooenable Remote Desktop, change hello local Administrator account used tooconnect toohello virtual machines, hello certificate used in authentication and set hello expiration date.</span></span>

1. <span data-ttu-id="62fb8-115">Klicka på **molntjänster**hello namnet på hello-Molntjänsten och klicka sedan på **fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="62fb8-115">Click **Cloud Services**, click hello name of hello cloud service, and then click **Remote Desktop**.</span></span>

    ![Cloud services fjärrskrivbord](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. <span data-ttu-id="62fb8-117">Välj om du vill tooenable Remote Desktop för en enskild roll eller för alla roller och sedan ändra hello värdet för hello switcher för**aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="62fb8-117">Choose whether you want tooenable Remote Desktop for an individual role or for all roles, then change hello value of hello switcher too**Enabled**.</span></span>

3. <span data-ttu-id="62fb8-118">Fyll i hello som krävs för användarnamn, lösenord, upphör att gälla och certifikat.</span><span class="sxs-lookup"><span data-stu-id="62fb8-118">Fill in hello required fields for user name, password, expiry, and certificate.</span></span>

    ![Cloud services fjärrskrivbord](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > <span data-ttu-id="62fb8-120">Alla rollinstanser kommer att startas om när du först aktivera Fjärrskrivbord och klicka på OK (markering).</span><span class="sxs-lookup"><span data-stu-id="62fb8-120">All role instances will be restarted when you first enable Remote Desktop and click OK (checkmark).</span></span> <span data-ttu-id="62fb8-121">tooprevent en omstart, hello certifikatlösenord används tooencrypt hello måste installeras på hello roll.</span><span class="sxs-lookup"><span data-stu-id="62fb8-121">tooprevent a reboot, hello certificate used tooencrypt hello password must be installed on hello role.</span></span> <span data-ttu-id="62fb8-122">tooprevent en omstart [överföra ett certifikat för hello Molntjänsten](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) och returnerar sedan toothis dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62fb8-122">tooprevent a restart, [upload a certificate for hello cloud service](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) and then return toothis dialog.</span></span>
   >
   >
3. <span data-ttu-id="62fb8-123">I **roller**väljer hello rollen tooupdate eller välj **alla** för alla roller.</span><span class="sxs-lookup"><span data-stu-id="62fb8-123">In **Roles**, select hello role you want tooupdate or select **All** for all roles.</span></span>

4. <span data-ttu-id="62fb8-124">När du är klar configuration-uppdateringar, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="62fb8-124">When you finish your configuration updates, click **Save**.</span></span> <span data-ttu-id="62fb8-125">Det tar en stund innan dina rollinstanser är klar tooreceive anslutningar.</span><span class="sxs-lookup"><span data-stu-id="62fb8-125">It will take a few moments before your role instances are ready tooreceive connections.</span></span>

## <a name="remote-into-role-instances"></a><span data-ttu-id="62fb8-126">Remote i rollinstanser</span><span class="sxs-lookup"><span data-stu-id="62fb8-126">Remote into role instances</span></span>
<span data-ttu-id="62fb8-127">När fjärrskrivbord har aktiverats på hello roller, kan du upprätta en anslutning direkt från hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="62fb8-127">Once Remote Desktop is enabled on hello roles, you can initiate a connection directly from hello Azure Portal:</span></span>

1. <span data-ttu-id="62fb8-128">Klicka på **instanser** tooopen hello **instanser** bladet.</span><span class="sxs-lookup"><span data-stu-id="62fb8-128">Click **Instances** tooopen hello **Instances** blade.</span></span>
2. <span data-ttu-id="62fb8-129">Välj en rollinstans som har konfigurerats för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="62fb8-129">Select a role instance that has Remote Desktop configured.</span></span>
3. <span data-ttu-id="62fb8-130">Klicka på **Anslut** toodownload en RDP-filen för hälsningspaket rollinstansen.</span><span class="sxs-lookup"><span data-stu-id="62fb8-130">Click **Connect** toodownload an RDP file for hello role instance.</span></span>

    ![Cloud services fjärrskrivbord](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. <span data-ttu-id="62fb8-132">Klicka på **öppna** och sedan **Anslut** toostart hello fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="62fb8-132">Click **Open** and then **Connect** toostart hello Remote Desktop connection.</span></span>

>[!NOTE]
> <span data-ttu-id="62fb8-133">Om din molntjänst placerad bakom en NSG, måste du kanske toocreate regler som tillåter trafik på portarna **3389** och **20000**.</span><span class="sxs-lookup"><span data-stu-id="62fb8-133">If your cloud service is sitting behind an NSG, you may need toocreate rules that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="62fb8-134">Fjärrskrivbord använder port **3389**.</span><span class="sxs-lookup"><span data-stu-id="62fb8-134">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="62fb8-135">Molnet tjänstinstanser belastningsutjämnas, så du inte kan styra vilken instans tooconnect till direkt.</span><span class="sxs-lookup"><span data-stu-id="62fb8-135">Cloud Service instances are load balanced, so you can't directly control which instance tooconnect to.</span></span>  <span data-ttu-id="62fb8-136">Hej *RemoteForwarder* och *RemoteAccess* agenter hantera RDP-trafik och Tillåt hello klienten toosend en RDP-cookie och ange en enskild instans tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="62fb8-136">hello *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow hello client toosend an RDP cookie and specify an individual instance tooconnect to.</span></span>  <span data-ttu-id="62fb8-137">Hej *RemoteForwarder* och *RemoteAccess* agenter kräver att port **20000*** öppnas, där blockeras om du har en NSG.</span><span class="sxs-lookup"><span data-stu-id="62fb8-137">hello *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62fb8-138">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="62fb8-138">Additional resources</span></span>

<span data-ttu-id="62fb8-139">[Hur tooConfigure molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="62fb8-139">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
