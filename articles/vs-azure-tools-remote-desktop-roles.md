---
title: "aaaUsing Fjärrskrivbord med Azure-roller | Microsoft Docs"
description: "Med hjälp av fjärrskrivbord med Azure roller"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d35fd421cde8be9e3caa474db95974a54e528bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a><span data-ttu-id="b52a2-103">Med hjälp av fjärrskrivbord med Azure roller</span><span class="sxs-lookup"><span data-stu-id="b52a2-103">Using Remote Desktop with Azure Roles</span></span>
<span data-ttu-id="b52a2-104">Du kan komma åt Azure roller och virtuella datorer som hanteras av Azure med hjälp av hello Azure SDK och Fjärrskrivbordstjänster.</span><span class="sxs-lookup"><span data-stu-id="b52a2-104">By using hello Azure SDK and Remote Desktop Services, you can access Azure roles and virtual machines that are hosted by Azure.</span></span> <span data-ttu-id="b52a2-105">Du kan konfigurera Fjärrskrivbordstjänster från en Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b52a2-105">In Visual Studio, you can configure Remote Desktop Services from an Azure cloud service project.</span></span> <span data-ttu-id="b52a2-106">tooenable för Fjärrskrivbordstjänster, måste du skapa ett fungerande projekt som innehåller en eller flera roller och sedan publicera tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b52a2-106">tooenable Remote Desktop Services, you must create a working project that contains one or more roles and then publish it tooAzure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b52a2-107">Du ska använda en Azure roll för felsökning eller endast utveckling.</span><span class="sxs-lookup"><span data-stu-id="b52a2-107">You should access an Azure role for troubleshooting or development only.</span></span> <span data-ttu-id="b52a2-108">Hej syftet med varje virtuell dator är toorun en viss roll i ditt Azure-program inte toorun andra klientprogram.</span><span class="sxs-lookup"><span data-stu-id="b52a2-108">hello purpose of each virtual machine is toorun a specific role in your Azure application, not toorun other client applications.</span></span> <span data-ttu-id="b52a2-109">Om du vill toouse Azure toohost en virtuell dator som du kan använda för något ändamål, se att komma åt Azure virtuella datorer från Server Explorer.</span><span class="sxs-lookup"><span data-stu-id="b52a2-109">If you want toouse Azure toohost a virtual machine that you can use for any purpose, see Accessing Azure Virtual Machines from Server Explorer.</span></span>
> 
> 

## <a name="tooenable-and-use-remote-desktop-for-an-azure-role"></a><span data-ttu-id="b52a2-110">tooenable och använda Fjärrskrivbord för en Azure-roll</span><span class="sxs-lookup"><span data-stu-id="b52a2-110">tooenable and use Remote Desktop for an Azure Role</span></span>
1. <span data-ttu-id="b52a2-111">Öppna hello snabbmenyn för cloud service-projekt i Solution Explorer och välj sedan **publicera**.</span><span class="sxs-lookup"><span data-stu-id="b52a2-111">In Solution Explorer, open hello shortcut menu for your cloud service project, and then choose **Publish**.</span></span>
   
    <span data-ttu-id="b52a2-112">Hej **publicera Azure-programmet** guiden visas.</span><span class="sxs-lookup"><span data-stu-id="b52a2-112">hello **Publish Azure Application** wizard appears.</span></span>
   
    ![Publicera kommandot för en Cloud Service-projekt](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. <span data-ttu-id="b52a2-114">Längst ned hello **Publiceringsinställningar i Microsoft Azure** hello guiden, Välj hello **aktivera Fjärrskrivbord** för alla roller.</span><span class="sxs-lookup"><span data-stu-id="b52a2-114">At hello bottom of **Microsoft Azure Publish Settings** page of hello wizard, select hello **Enable Remote Desktop** for all roles check box.</span></span> 
   
    <span data-ttu-id="b52a2-115">Hej **konfiguration av fjärrskrivbord** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="b52a2-115">hello **Remote Desktop Configuration** dialog box appears.</span></span>
3. <span data-ttu-id="b52a2-116">Längst ned hello hello **konfiguration av fjärrskrivbord** dialogrutan Välj hello **fler alternativ** knappen.</span><span class="sxs-lookup"><span data-stu-id="b52a2-116">At hello bottom of hello **Remote Desktop Configuration** dialog box, choose hello **More Options** button.</span></span> 
   
    <span data-ttu-id="b52a2-117">Detta visar en nedrullningsbar listruta som låter dig skapa eller välj ett certifikat så att du kan kryptera autentiseringsuppgifter information när du ansluter via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="b52a2-117">This displays a dropdown list box that lets you create or choose a certificate so that you can encrypt credentials information when connecting via remote desktop.</span></span>
4. <span data-ttu-id="b52a2-118">Välj i listrutan hello  **&lt;skapa >**, eller välj en befintlig hello-listan.</span><span class="sxs-lookup"><span data-stu-id="b52a2-118">In hello dropdown list, choose **&lt;Create>**, or choose an existing one from hello list.</span></span> 
   
    <span data-ttu-id="b52a2-119">Hoppa över hello följande steg om du väljer ett befintligt certifikat.</span><span class="sxs-lookup"><span data-stu-id="b52a2-119">If you choose an existing certificate, skip hello following steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b52a2-120">hello-certifikat som du behöver för en anslutning till fjärrskrivbord skiljer sig från hello-certifikat som du använder för andra åtgärder i Azure.</span><span class="sxs-lookup"><span data-stu-id="b52a2-120">hello certificates that you need for a remote desktop connection are different from hello certificates that you use for other Azure operations.</span></span> <span data-ttu-id="b52a2-121">hello fjärråtkomstcertifikatet måste ha en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="b52a2-121">hello remote access certificate must have a private key.</span></span>
   > 
   > 
   
    <span data-ttu-id="b52a2-122">Hej **skapa certifikat** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="b52a2-122">hello **Create Certificate** dialog box appears.</span></span>
   
   1. <span data-ttu-id="b52a2-123">Ange ett eget namn för hello nya certifikatet och välj sedan hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="b52a2-123">Provide a friendly name for hello new certificate, and then choose hello **OK** button.</span></span> <span data-ttu-id="b52a2-124">hello nytt certifikat visas i listrutan hello..</span><span class="sxs-lookup"><span data-stu-id="b52a2-124">hello new certificate appears in hello dropdown list box.</span></span>
   2. <span data-ttu-id="b52a2-125">I hello **konfiguration av fjärrskrivbord** dialogrutan Ange ett användarnamn och ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="b52a2-125">In hello **Remote Desktop Configuration** dialog box, provide a user name and a password.</span></span>
      
       <span data-ttu-id="b52a2-126">Du kan inte använda ett befintligt konto.</span><span class="sxs-lookup"><span data-stu-id="b52a2-126">You can’t use an existing account.</span></span> <span data-ttu-id="b52a2-127">Ange inte administratören som hello användarnamn för hello nytt konto.</span><span class="sxs-lookup"><span data-stu-id="b52a2-127">Don’t specify Administrator as hello user name for hello new account.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="b52a2-128">Om hello lösenordet inte uppfyller kraven på komplexitet hello, visas en röd ikon nästa toohello lösenordet i textrutan.</span><span class="sxs-lookup"><span data-stu-id="b52a2-128">If hello password doesn’t meet hello complexity requirements, a red icon appears next toohello password text box.</span></span> <span data-ttu-id="b52a2-129">hello lösenord måste innehålla versaler, gemena bokstäver och siffror eller symboler.</span><span class="sxs-lookup"><span data-stu-id="b52a2-129">hello password must include capital letters, lowercase letters, and numbers or symbols.</span></span>
      > 
      > 
   3. <span data-ttu-id="b52a2-130">Välj ett datum på vilket konto som hello upphör att gälla och efter vilken anslutning till fjärrskrivbord kommer att blockeras.</span><span class="sxs-lookup"><span data-stu-id="b52a2-130">Choose a date on which hello account will expire and after which remote desktop connections will be blocked.</span></span>
   4. <span data-ttu-id="b52a2-131">När du har förutsatt att alla hello nödvändig information, Välj hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="b52a2-131">After you've provided all hello required information, choose hello **OK** button.</span></span>
      
       <span data-ttu-id="b52a2-132">Flera inställningar som gör att fjärråtkomsttjänsten läggs toohello .csdef- och .cscfg-filer.</span><span class="sxs-lookup"><span data-stu-id="b52a2-132">Several settings that enable Remote Access Services are added toohello .cscfg and .csdef files.</span></span>
5. <span data-ttu-id="b52a2-133">I hello **Publiceringsinställningar i Microsoft Azure** guiden, Välj hello **OK** knappen när du är klar toopublish Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="b52a2-133">In hello **Microsoft Azure Publish Settings** wizard, choose hello **OK** button when you’re ready toopublish your cloud service.</span></span>
   
    <span data-ttu-id="b52a2-134">Om du inte är redo toopublish väljer hello **Avbryt** knappen.</span><span class="sxs-lookup"><span data-stu-id="b52a2-134">If you're not ready toopublish, choose hello **Cancel** button.</span></span> <span data-ttu-id="b52a2-135">hello konfigurationsinställningarna sparas och du kan publicera din molntjänst senare.</span><span class="sxs-lookup"><span data-stu-id="b52a2-135">hello configuration settings are saved, and you can publish your cloud service later.</span></span>

## <a name="connect-tooan-azure-role-by-using-remote-desktop"></a><span data-ttu-id="b52a2-136">Ansluta tooan Azure roll med hjälp av fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="b52a2-136">Connect tooan Azure Role by using Remote Desktop</span></span>
<span data-ttu-id="b52a2-137">När du har publicerat din tjänst i molnet på Azure, kan du använda Server Explorer toolog till hello virtuella datorer som är värd för Azure.</span><span class="sxs-lookup"><span data-stu-id="b52a2-137">After you publish your cloud service on Azure, you can use Server Explorer toolog into hello virtual machines that Azure hosts.</span></span> 

1. <span data-ttu-id="b52a2-138">I Server Explorer expanderar du hello **Azure** nod, och expandera sedan hello nod för en tjänst i molnet och en av dess roller toodisplay en lista över instanser.</span><span class="sxs-lookup"><span data-stu-id="b52a2-138">In Server Explorer, expand hello **Azure** node, and then expand hello node for a cloud service and one of its roles toodisplay a list of instances.</span></span>
2. <span data-ttu-id="b52a2-139">Öppna hello snabbmenyn för en instans-nod och välj sedan **ansluta med hjälp av fjärrskrivbord**.</span><span class="sxs-lookup"><span data-stu-id="b52a2-139">Open hello shortcut menu for an instance node, and then choose **Connect Using Remote Desktop**.</span></span>
   
    ![Ansluter via fjärrskrivbord](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. <span data-ttu-id="b52a2-141">Ange hello användarnamn och lösenord som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="b52a2-141">Enter hello user name and password that you created previously.</span></span> <span data-ttu-id="b52a2-142">Du är nu inloggad i fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="b52a2-142">You are now logged into your remote session.</span></span>

