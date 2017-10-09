---
title: "Felsökning av RemoteApp - aaaAzure programfel för att starta och anslutningen | Microsoft Docs"
description: "Lär dig hur tootroubleshoot problem med start- och ansluter tooapplications i Azure RemoteApp."
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
ms.openlocfilehash: e51d480c9d3fa1f2076f95b63c7a8cd2f6956a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a><span data-ttu-id="fd433-103">Felsöka Azure RemoteApp - programfel för att starta och anslutning</span><span class="sxs-lookup"><span data-stu-id="fd433-103">Troubleshoot Azure RemoteApp - Application launch and connection failures</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fd433-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="fd433-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="fd433-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="fd433-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="fd433-106">Finns i Azure RemoteApp-programmen inte toolaunch för flera olika skäl.</span><span class="sxs-lookup"><span data-stu-id="fd433-106">Applications hosted in Azure RemoteApp can fail toolaunch for a few different reasons.</span></span> <span data-ttu-id="fd433-107">Den här artikeln beskrivs olika anledningar och felmeddelanden som användarna kan få när försök toolaunch program.</span><span class="sxs-lookup"><span data-stu-id="fd433-107">This article describes various reasons and error messages users might receive when trying toolaunch applications.</span></span> <span data-ttu-id="fd433-108">Det är också pratar om anslutningsfel.</span><span class="sxs-lookup"><span data-stu-id="fd433-108">It also talks about connection failures.</span></span> <span data-ttu-id="fd433-109">(Men den här artikeln beskriver inte problem när du loggar in hello Azure RemoteApp-klienten.)</span><span class="sxs-lookup"><span data-stu-id="fd433-109">(But this article does not describe issues when signing into hello Azure RemoteApp client.)</span></span>  

<span data-ttu-id="fd433-110">Läs vidare för information om vanliga felmeddelanden på grund av tooapp start- och fel.</span><span class="sxs-lookup"><span data-stu-id="fd433-110">Read on for information about common error messages due tooapp launch and connection failures.</span></span>

## <a name="were-getting-you-set-up-try-again-in-10-minutes"></a><span data-ttu-id="fd433-111">Vi håller på att ställa in... Försök igen om 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="fd433-111">We're getting you set up... Try again in 10 minutes.</span></span>
<span data-ttu-id="fd433-112">Det här felet betyder Azure RemoteApp skala upp toomeet hello kapacitetsbehovet för dina användare.</span><span class="sxs-lookup"><span data-stu-id="fd433-112">This error means Azure RemoteApp is scaling up toomeet hello capacity need of your users.</span></span> <span data-ttu-id="fd433-113">I bakgrunden hello skapas mer Azure RemoteApp-VM-instanser toohandle hello kapacitetsbehov för dina användare.</span><span class="sxs-lookup"><span data-stu-id="fd433-113">In hello background more Azure RemoteApp VM instances are being created toohandle hello capacity needs of your users.</span></span> <span data-ttu-id="fd433-114">Vanligtvis detta tar cirka fem minuter men kan ta upp too10 minuter.</span><span class="sxs-lookup"><span data-stu-id="fd433-114">Typically this takes around five minutes but can take up too10 minutes.</span></span> <span data-ttu-id="fd433-115">Ibland är det inträffar inte tillräckligt snabbt och resurser krävs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="fd433-115">Sometimes, this doesn't happen fast enough and resources are needed immediately.</span></span> <span data-ttu-id="fd433-116">Till exempel 9: 00 scenario där många användare behöver toouse din app i Azure RemoteAppn på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="fd433-116">For example a 9 AM scenario where many users need toouse your app in Azure RemoteAppn at hello same time.</span></span> <span data-ttu-id="fd433-117">Om detta händer tooyou vi aktivera **kapacitet läge** på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="fd433-117">If this happens tooyou we can enable **capacity mode** on hello back end.</span></span> <span data-ttu-id="fd433-118">toodo detta öppna ett Azure supportärende.</span><span class="sxs-lookup"><span data-stu-id="fd433-118">toodo this open an Azure Support ticket.</span></span> <span data-ttu-id="fd433-119">Att vissa tooinclude ditt prenumerations-ID i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="fd433-119">Be certain tooinclude your subscription ID in hello request.</span></span>  

![Vi att konfigurera](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-tooyour-applications-please-re-launch-your-application"></a><span data-ttu-id="fd433-121">Det gick inte att återansluta automatiskt tooyour program, starta om programmet</span><span class="sxs-lookup"><span data-stu-id="fd433-121">Could not auto-reconnect tooyour applications, please re-launch your application</span></span>
<span data-ttu-id="fd433-122">Det här meddelandet visas ofta om du använder Azure RemoteApp och placera din PC toosleep som är längre än 4 timmar och sedan aktiverades av din dator och hello Azure RemoteApp-klienten försök tooauto återansluta och tidsgränsen har överskridits.</span><span class="sxs-lookup"><span data-stu-id="fd433-122">This error message is often seen if you were using Azure RemoteApp and then put your PC toosleep longer than 4 hours and then woke your PC up and hello Azure RemoteApp client attempt tooauto reconnect and timeout was exceeded.</span></span>  <span data-ttu-id="fd433-123">Instruera användarna toonavigate tillbaka toohello programmet och försök tooopen från hello Azure RemoteApp-klienten.</span><span class="sxs-lookup"><span data-stu-id="fd433-123">Instruct users toonavigate back toohello application and attempt tooopen it from hello Azure RemoteApp client.</span></span>

![Det gick inte att återansluta automatiskt tooyour program](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-hello-temp-profile"></a><span data-ttu-id="fd433-125">Problem med hello tillfällig profil</span><span class="sxs-lookup"><span data-stu-id="fd433-125">Problems with hello temp profile</span></span>
<span data-ttu-id="fd433-126">Felet uppstår när användarprofilen (användarprofil-Disk) inte kunde toomount och hello användare tar emot en tillfälliga profil.</span><span class="sxs-lookup"><span data-stu-id="fd433-126">This error occurs when your user profile (User Profile Disk) failed toomount and hello user received a temporary profile.</span></span>  <span data-ttu-id="fd433-127">Administratörer bör gå toohello samling i hello Azure-portalen och gå sedan toohello **sessioner** fliken och försök för**logga ut** hello användare.</span><span class="sxs-lookup"><span data-stu-id="fd433-127">Administrators should navigate toohello collection in hello Azure portal and then go toohello **Sessions** tab and attempt too**Log Off** hello user.</span></span> <span data-ttu-id="fd433-128">Detta kommer att tvinga en fullständig logga ut från hello användarsession - sedan har hello användaren försök toolaunch en app igen.</span><span class="sxs-lookup"><span data-stu-id="fd433-128">This will force a full log off of hello user session - then have hello user attempt toolaunch an app again.</span></span> <span data-ttu-id="fd433-129">Om kontakta Azure-supporten om problemet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="fd433-129">If that fails contact Azure support.</span></span>

## <a name="azure-remoteapp-has-stopped-working"></a><span data-ttu-id="fd433-130">Azure RemoteApp har slutat att fungera</span><span class="sxs-lookup"><span data-stu-id="fd433-130">Azure RemoteApp has stopped working</span></span>
<span data-ttu-id="fd433-131">Det här felmeddelandet innebär hello Azure RemoteApp-klienten har ett problem och måste toobe startas om.</span><span class="sxs-lookup"><span data-stu-id="fd433-131">This error message means hello Azure RemoteApp client is having an issue and needs toobe restarted.</span></span> <span data-ttu-id="fd433-132">Instruera användarna tooclose: Välj **Stäng program** och starta sedan om hello Azure RemoteApp-klienten.</span><span class="sxs-lookup"><span data-stu-id="fd433-132">Instruct users tooclose: select **Close program** and then launch hello Azure RemoteApp client again.</span></span>  <span data-ttu-id="fd433-133">Om hello problemet kvarstår öppen och supportärende i Azure.</span><span class="sxs-lookup"><span data-stu-id="fd433-133">If hello issue continues open and Azure Support ticket.</span></span>

![Azure RemoteApp har slutat att fungera](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-hello-connection-or-contact-your-system-administrator"></a><span data-ttu-id="fd433-135">Ett fel uppstod när fjärrskrivbordsanslutningen försökte få åtkomst till den här resursen.</span><span class="sxs-lookup"><span data-stu-id="fd433-135">An error occurred while Remote Desktop Connection was accessing this resource.</span></span> <span data-ttu-id="fd433-136">Försök hello anslutning eller kontakta din systemadministratör</span><span class="sxs-lookup"><span data-stu-id="fd433-136">Retry hello connection or contact your system administrator</span></span>
<span data-ttu-id="fd433-137">Detta är ett allmänt felmeddelande - kontakta Azure-supporten så kan vi undersöka.</span><span class="sxs-lookup"><span data-stu-id="fd433-137">This is a generic error message - contact Azure support so we can investigate.</span></span> 

![Allmänt Azure RemoteApp-meddelande](./media/remoteapp-apptrouble/ra-apptrouble4.png) 

