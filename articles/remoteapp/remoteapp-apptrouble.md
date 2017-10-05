---
title: "Felsökning för Azure RemoteApp - programfel för att starta och anslutningen | Microsoft Docs"
description: "Lär dig hur du felsöker problem med att starta och ansluter till program i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e5cf7171-d1c2-4053-a38b-5af7821305e1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: fc9d538991adce7fc13e9654b9a7c6d113d03fde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a><span data-ttu-id="13460-103">Felsöka Azure RemoteApp - programfel för att starta och anslutning</span><span class="sxs-lookup"><span data-stu-id="13460-103">Troubleshoot Azure RemoteApp - Application launch and connection failures</span></span>
> [!IMPORTANT]
> <span data-ttu-id="13460-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="13460-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="13460-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="13460-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="13460-106">Program som finns i Azure RemoteApp kan inte starta för flera olika skäl.</span><span class="sxs-lookup"><span data-stu-id="13460-106">Applications hosted in Azure RemoteApp can fail to launch for a few different reasons.</span></span> <span data-ttu-id="13460-107">Den här artikeln beskrivs olika anledningar och felmeddelanden som användare kan visas vid försök att starta program.</span><span class="sxs-lookup"><span data-stu-id="13460-107">This article describes various reasons and error messages users might receive when trying to launch applications.</span></span> <span data-ttu-id="13460-108">Det är också pratar om anslutningsfel.</span><span class="sxs-lookup"><span data-stu-id="13460-108">It also talks about connection failures.</span></span> <span data-ttu-id="13460-109">(Men den här artikeln beskriver inte problem när du loggar in på Azure RemoteApp-klienten.)</span><span class="sxs-lookup"><span data-stu-id="13460-109">(But this article does not describe issues when signing into the Azure RemoteApp client.)</span></span>  

<span data-ttu-id="13460-110">Läs vidare för information om vanliga felmeddelanden på grund av fel för att starta och anslutning av app.</span><span class="sxs-lookup"><span data-stu-id="13460-110">Read on for information about common error messages due to app launch and connection failures.</span></span>

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a><span data-ttu-id="13460-111">Vi håller på att ställa in... Försök igen om 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="13460-111">We're getting you set up... Try again in 10 minutes.</span></span>
<span data-ttu-id="13460-112">Det här felet betyder Azure RemoteApp skala upp så att de uppfyller kapacitetsbehovet för dina användare.</span><span class="sxs-lookup"><span data-stu-id="13460-112">This error means Azure RemoteApp is scaling up to meet the capacity need of your users.</span></span> <span data-ttu-id="13460-113">I bakgrunden som mer Azure RemoteApp-VM-instanser skapas för att hantera kapacitetsbehov för dina användare.</span><span class="sxs-lookup"><span data-stu-id="13460-113">In the background more Azure RemoteApp VM instances are being created to handle the capacity needs of your users.</span></span> <span data-ttu-id="13460-114">Vanligtvis detta tar cirka fem minuter men kan ta upp till 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="13460-114">Typically this takes around five minutes but can take up to 10 minutes.</span></span> <span data-ttu-id="13460-115">Ibland är det inträffar inte tillräckligt snabbt och resurser krävs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="13460-115">Sometimes, this doesn't happen fast enough and resources are needed immediately.</span></span> <span data-ttu-id="13460-116">Till exempel en 9: 00 scenario där många användare behöver använda din app i Azure RemoteAppn på samma gång.</span><span class="sxs-lookup"><span data-stu-id="13460-116">For example a 9 AM scenario where many users need to use your app in Azure RemoteAppn at the same time.</span></span> <span data-ttu-id="13460-117">Om detta händer du vi aktivera **kapacitet läge** på serverdelen.</span><span class="sxs-lookup"><span data-stu-id="13460-117">If this happens to you we can enable **capacity mode** on the back end.</span></span> <span data-ttu-id="13460-118">Om du vill göra detta för att öppna ett supportärende i Azure.</span><span class="sxs-lookup"><span data-stu-id="13460-118">To do this open an Azure Support ticket.</span></span> <span data-ttu-id="13460-119">Vara säker på att inkludera ditt prenumerations-ID i begäran.</span><span class="sxs-lookup"><span data-stu-id="13460-119">Be certain to include your subscription ID in the request.</span></span>  

![Vi att konfigurera](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a><span data-ttu-id="13460-121">Det gick inte att återansluta automatiskt till programmen, starta om programmet</span><span class="sxs-lookup"><span data-stu-id="13460-121">Could not auto-reconnect to your applications, please re-launch your application</span></span>
<span data-ttu-id="13460-122">Det här meddelandet visas ofta om du använder Azure RemoteApp och sedan försätta datorn i viloläge längre än 4 timmar och sedan aktiverades av din dator och Azure RemoteApp-klienten försöker automatiskt återansluter och tidsgränsen har överskridits.</span><span class="sxs-lookup"><span data-stu-id="13460-122">This error message is often seen if you were using Azure RemoteApp and then put your PC to sleep longer than 4 hours and then woke your PC up and the Azure RemoteApp client attempt to auto reconnect and timeout was exceeded.</span></span>  <span data-ttu-id="13460-123">Instruera användarna att gå tillbaka till programmet och försök att öppna från Azure RemoteApp-klienten.</span><span class="sxs-lookup"><span data-stu-id="13460-123">Instruct users to navigate back to the application and attempt to open it from the Azure RemoteApp client.</span></span>

![Det gick inte att återansluta automatiskt till ditt program](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a><span data-ttu-id="13460-125">Problem med tillfällig profil</span><span class="sxs-lookup"><span data-stu-id="13460-125">Problems with the temp profile</span></span>
<span data-ttu-id="13460-126">Det här felet uppstår om användarprofilen (användarprofil-Disk) inte gick att montera och användaren får en tillfälliga profil.</span><span class="sxs-lookup"><span data-stu-id="13460-126">This error occurs when your user profile (User Profile Disk) failed to mount and the user received a temporary profile.</span></span>  <span data-ttu-id="13460-127">Administratörer bör navigera till samlingen i Azure-portalen och gå sedan till den **sessioner** fliken och försök att **logga ut** användaren.</span><span class="sxs-lookup"><span data-stu-id="13460-127">Administrators should navigate to the collection in the Azure portal and then go to the **Sessions** tab and attempt to **Log Off** the user.</span></span> <span data-ttu-id="13460-128">Detta kommer att tvinga en fullständig logga ut från sessionen - sedan användaren försöker starta en app igen.</span><span class="sxs-lookup"><span data-stu-id="13460-128">This will force a full log off of the user session - then have the user attempt to launch an app again.</span></span> <span data-ttu-id="13460-129">Om kontakta Azure-supporten om problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="13460-129">If that fails contact Azure support.</span></span>

## <a name="azure-remoteapp-has-stopped-working"></a><span data-ttu-id="13460-130">Azure RemoteApp har slutat att fungera</span><span class="sxs-lookup"><span data-stu-id="13460-130">Azure RemoteApp has stopped working</span></span>
<span data-ttu-id="13460-131">Det här felmeddelandet innebär Azure RemoteApp-klienten har ett problem och måste startas om.</span><span class="sxs-lookup"><span data-stu-id="13460-131">This error message means the Azure RemoteApp client is having an issue and needs to be restarted.</span></span> <span data-ttu-id="13460-132">Instruera användarna att stänga: Välj **Stäng program** och starta sedan om Azure RemoteApp-klienten.</span><span class="sxs-lookup"><span data-stu-id="13460-132">Instruct users to close: select **Close program** and then launch the Azure RemoteApp client again.</span></span>  <span data-ttu-id="13460-133">Om problemet kvarstår öppen och supportärende i Azure.</span><span class="sxs-lookup"><span data-stu-id="13460-133">If the issue continues open and Azure Support ticket.</span></span>

![Azure RemoteApp har slutat att fungera](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a><span data-ttu-id="13460-135">Ett fel uppstod när fjärrskrivbordsanslutningen försökte få åtkomst till den här resursen.</span><span class="sxs-lookup"><span data-stu-id="13460-135">An error occurred while Remote Desktop Connection was accessing this resource.</span></span> <span data-ttu-id="13460-136">Försöka ansluta igen eller kontakta din systemadministratör</span><span class="sxs-lookup"><span data-stu-id="13460-136">Retry the connection or contact your system administrator</span></span>
<span data-ttu-id="13460-137">Detta är ett allmänt felmeddelande - kontakta Azure-supporten så kan vi undersöka.</span><span class="sxs-lookup"><span data-stu-id="13460-137">This is a generic error message - contact Azure support so we can investigate.</span></span> 

![Allmänt Azure RemoteApp-meddelande](./media/remoteapp-apptrouble/ra-apptrouble4.png) 

