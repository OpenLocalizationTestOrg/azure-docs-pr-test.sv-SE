---
title: aaaSetting upp med namnet autentiseringsuppgifter | Microsoft Docs
description: "Lär dig hur tootooprovide autentiseringsuppgifter som Visual Studio kan använda tooauthenticate begäranden tooAzure toopublish ett program tooAzure från Visual Studio eller toomonitor en befintlig molntjänst... "
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 3cc147a2f7a3e766293ca282f56078cf24281346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-named-authentication-credentials"></a><span data-ttu-id="4bc86-103">Konfigurera namngivna autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="4bc86-103">Setting Up Named Authentication Credentials</span></span>
<span data-ttu-id="4bc86-104">toopublish ett program tooAzure från Visual Studio eller toomonitor en befintlig molntjänst, måste du ange autentiseringsuppgifter som Visual Studio kan använda tooauthenticate begär tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4bc86-104">toopublish an application tooAzure from Visual Studio or toomonitor an existing cloud service, you must provide credentials that Visual Studio can use tooauthenticate requests tooAzure.</span></span> <span data-ttu-id="4bc86-105">Det finns flera platser i Visual Studio, där du kan logga in tooprovide autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="4bc86-105">There are several places in Visual Studio where you can sign in tooprovide these credentials.</span></span> <span data-ttu-id="4bc86-106">Till exempel från hello Server Explorer, du kan öppna hello snabbmenyn för hello **Azure** nod och välj **ansluta tooMicrosoft Azure-prenumerationen... **. När du loggar in information om hello-prenumerationer som är kopplade till ditt Azure-konto finns i Visual Studio och det inte finns något mer du har toodo.</span><span class="sxs-lookup"><span data-stu-id="4bc86-106">For example, from hello Server Explorer, you can open hello shortcut menu for hello **Azure** node and choose **Connect tooMicrosoft Azure Subscription...**. When you sign in, hello subscription information associated with your Azure account is available in Visual Studio, and there is nothing more you have toodo.</span></span>

<span data-ttu-id="4bc86-107">Azure-verktyg har också stöd för ett äldre sätt för att tillhandahålla autentiseringsuppgifter med hjälp av hello prenumeration fil (.publishsettings-fil).</span><span class="sxs-lookup"><span data-stu-id="4bc86-107">Azure Tools also supports an older way of providing credentials, using hello subscription file (.publishsettings file).</span></span> <span data-ttu-id="4bc86-108">Det här avsnittet beskriver den här metoden stöds fortfarande i Azure SDK 2.2.</span><span class="sxs-lookup"><span data-stu-id="4bc86-108">This topic describes this method, which is still supported in Azure SDK 2.2.</span></span>

<span data-ttu-id="4bc86-109">hello följande objekt krävs för autentisering tooAzure:</span><span class="sxs-lookup"><span data-stu-id="4bc86-109">hello following items are required for authentication tooAzure:</span></span>

* <span data-ttu-id="4bc86-110">Prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="4bc86-110">Your subscription ID</span></span>
* <span data-ttu-id="4bc86-111">Ett giltigt X.509 v3-certifikat</span><span class="sxs-lookup"><span data-stu-id="4bc86-111">A valid X.509 v3 certificate</span></span>

> [!NOTE]
> <span data-ttu-id="4bc86-112">hello-längden hello X.509 v3-certifikat måste vara minst 2 048 bitar.</span><span class="sxs-lookup"><span data-stu-id="4bc86-112">hello length of hello X.509 v3 certificate's key must be at least 2048 bits.</span></span> <span data-ttu-id="4bc86-113">Azure avvisar alla certifikat som uppfyller inte det här kravet eller som inte är giltigt.</span><span class="sxs-lookup"><span data-stu-id="4bc86-113">Azure will reject any certificate that doesn’t meet this requirement or that isn’t valid.</span></span>
>
>

<span data-ttu-id="4bc86-114">Visual Studio använder ditt prenumerations-ID tillsammans med hello certifikatdata som autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4bc86-114">Visual Studio uses your subscription ID together with hello certificate data as credentials.</span></span> <span data-ttu-id="4bc86-115">hello rätt autentiseringsuppgifter refereras i hello prenumeration fil (.publishsettings), som innehåller en offentlig nyckel för hello certifikatet.</span><span class="sxs-lookup"><span data-stu-id="4bc86-115">hello appropriate credentials are referenced in hello subscription file (.publishsettings file), which contains a public key for hello certificate.</span></span> <span data-ttu-id="4bc86-116">hello prenumeration fil kan innehålla referenser för mer än en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4bc86-116">hello subscription file can contain credentials for more than one subscription.</span></span>

<span data-ttu-id="4bc86-117">Du kan redigera hello prenumerationsinformation från hello **ny/redigera prenumeration** dialogrutan som beskrivs senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4bc86-117">You can edit hello subscription information from hello **New/Edit Subscription** dialog box, as explained later in this topic.</span></span>

<span data-ttu-id="4bc86-118">Om du vill att toocreate certifikat själv, kan du läsa toohello instruktionerna i [skapa och ladda upp ett Hanteringscertifikat för Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) och sedan ladda upp hello certifikat toohello manuellt [klassiska Azure-portalen ](http://go.microsoft.com/fwlink/?LinkID=213885).</span><span class="sxs-lookup"><span data-stu-id="4bc86-118">If you want toocreate a certificate yourself, you can refer toohello instructions in [Create and Upload a Management Certificate for Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) and then manually upload hello certificate toohello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885).</span></span>

> [!NOTE]
> <span data-ttu-id="4bc86-119">Autentiseringsuppgifterna som Visual Studio kräver toomanage dina molntjänster som inte är hello samma autentiseringsuppgifter som är nödvändiga tooauthenticate en begäran mot hello Azure storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4bc86-119">These credentials that Visual Studio requires toomanage your cloud services aren’t hello same credentials that are required tooauthenticate a request against hello Azure storage services.</span></span>
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a><span data-ttu-id="4bc86-120">Importera, skapa eller redigera autentiseringsuppgifter i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bc86-120">Import, set up, or edit authentication credentials in Visual Studio</span></span>
<span data-ttu-id="4bc86-121">Du kan importera, ställa in eller ändra dina autentiseringsuppgifter i hello **ny prenumeration** dialogrutan som visas om du utför hello följande åtgärd:</span><span class="sxs-lookup"><span data-stu-id="4bc86-121">You can import, set up, or modify your authentication credentials in hello **New Subscription** dialog box, which appears if you perform hello following action:</span></span>

* <span data-ttu-id="4bc86-122">I **Server Explorer**öppnar hello snabbmenyn för hello **Azure** noden, Välj **hantera och Filter prenumerationer... **, Välj hello **certifikat** fliken och gör något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="4bc86-122">In **Server Explorer**, open hello shortcut menu for hello **Azure** node, choose **Manage and Filter Subscriptions...**, choose hello **Certificates** tab, and do one of hello following:</span></span>

    * <span data-ttu-id="4bc86-123">Välj **importera** tooopen hello Importera Microsoft Azure-prenumerationer dialogrutan där du kan hämta hello prenumerationer filen för hello för närvarande läsas in prenumerationen, bläddra tooits hämtningsplats och sedan importera den för användning i autentisering.</span><span class="sxs-lookup"><span data-stu-id="4bc86-123">Choose **Import** tooopen hello Import Microsoft Azure Subscriptions dialog where you can download hello  subscriptions file for hello currently loaded subscription, browse tooits download location, and then import it for use in authentication.</span></span>
    * <span data-ttu-id="4bc86-124">Välj **ny** tooopen hello prenumeration dialogrutan där du kan ställa in en ny prenumeration för autentisering.</span><span class="sxs-lookup"><span data-stu-id="4bc86-124">Choose **New** tooopen hello New Subscription dialog where you can set up a new subscription for use in authentication.</span></span>
    * <span data-ttu-id="4bc86-125">Välj **redigera** (när du har valt prenumerationen aktiv) tooopen hello redigera dialogrutan för prenumerationer där du kan redigera en befintlig prenumeration för autentisering.</span><span class="sxs-lookup"><span data-stu-id="4bc86-125">Choose **Edit** (after choosing your active subscription) tooopen hello Edit Subscription dialog where you can edit an existing subscription for use in authentication.</span></span> 

<span data-ttu-id="4bc86-126">hello följande procedur förutsätter att hello **ny prenumeration** dialogruta är öppen.</span><span class="sxs-lookup"><span data-stu-id="4bc86-126">hello following procedure assumes that hello **New Subscription** dialog box is open.</span></span>

### <a name="tooset-up-authentication-credentials-in-visual-studio"></a><span data-ttu-id="4bc86-127">tooset in autentiseringsuppgifter i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4bc86-127">tooset up authentication credentials in Visual Studio</span></span>
1. <span data-ttu-id="4bc86-128">I hello **välja ett befintligt certifikat** för autentisering lista, Välj ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="4bc86-128">In hello **Select an existing certificate** for authentication list, choose a certificate.</span></span>
2. <span data-ttu-id="4bc86-129">Välj hello **kopiera hello fullständig sökväg** länk.</span><span class="sxs-lookup"><span data-stu-id="4bc86-129">Choose hello **Copy hello full path** link.</span></span> <span data-ttu-id="4bc86-130">hello sökvägen för hello-certifikat (.cer-fil) är kopierade toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="4bc86-130">hello path for hello certificate (.cer file) is copied toohello Clipboard.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="4bc86-131">toopublish ditt Azure-program från Visual Studio måste du överföra det här certifikatet toohello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4bc86-131">toopublish your Azure application from Visual Studio, you must upload this certificate toohello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
   >
   >
3. <span data-ttu-id="4bc86-132">tooupload hello certifikat toohello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="4bc86-132">tooupload hello certificate toohello Azure portal:</span></span>

   1. <span data-ttu-id="4bc86-133">Öppna hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="4bc86-133">Open hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>
   2. <span data-ttu-id="4bc86-134">Om du uppmanas logga in toohello portal och gå sedan för**inställningar**, **Hanteringscertifikat**.</span><span class="sxs-lookup"><span data-stu-id="4bc86-134">If prompted, sign in toohello portal and then navigate too**Settings**, **Management Certificates**.</span></span>
   3. <span data-ttu-id="4bc86-135">Hello Management certifikat fönstret Välj **överför**.</span><span class="sxs-lookup"><span data-stu-id="4bc86-135">In hello Management certificates pane, choose **Upload**.</span></span>
   4. <span data-ttu-id="4bc86-136">Välj din Azure-prenumeration, klistra in hello fullständig sökväg till hello .cer-fil som du just skapade och välj **överför**.</span><span class="sxs-lookup"><span data-stu-id="4bc86-136">Select your Azure subscription, paste hello full path of hello .cer file that you just created, and choose **Upload**.</span></span>
