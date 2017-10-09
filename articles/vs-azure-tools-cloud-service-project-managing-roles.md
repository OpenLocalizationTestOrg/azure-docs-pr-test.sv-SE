---
title: aaaManaging roller i Azure cloud services med Visual Studio | Microsoft Docs
description: "Lär dig hur tooadd och ta bort roller i Azure cloud services med Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 131edc534d1110ba3d25cd00a3a24b643576875c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a><span data-ttu-id="e3ed6-103">Hantera roller i Azure-molntjänster med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3ed6-103">Managing roles in Azure cloud services with Visual Studio</span></span>
<span data-ttu-id="e3ed6-104">När du har skapat din Azure-molntjänst kan du lägga till nya roller tooit eller ta bort befintliga roller från den.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-104">After you have created your Azure cloud service, you can add new roles tooit or remove existing roles from it.</span></span> <span data-ttu-id="e3ed6-105">Du kan också importera ett befintligt projekt och konvertera det tooa roll.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-105">You can also import an existing project and convert it tooa role.</span></span> <span data-ttu-id="e3ed6-106">Du kan till exempel importera ASP.NET-webbprogram och ange det som en webbroll.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-106">For example, you can import an ASP.NET web application and designate it as a web role.</span></span>

## <a name="adding-a-role-tooan-azure-cloud-service"></a><span data-ttu-id="e3ed6-107">Lägga till en roll tooan Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="e3ed6-107">Adding a role tooan Azure cloud service</span></span>
<span data-ttu-id="e3ed6-108">hello följande steg leder dig genom att lägga till ett webb eller arbetare rollen tooan Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-108">hello following steps guide you through adding a web or worker role tooan Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="e3ed6-109">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-109">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="e3ed6-110">I **Solution Explorer**, expandera hello projektnoden</span><span class="sxs-lookup"><span data-stu-id="e3ed6-110">In **Solution Explorer**, expand hello project node</span></span>

1. <span data-ttu-id="e3ed6-111">Högerklicka på hello **roller** nod toodisplay hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-111">Right-click hello **Roles** node toodisplay hello context menu.</span></span> <span data-ttu-id="e3ed6-112">Hello snabbmenyn, Välj **Lägg till**, Välj en befintlig webbroll eller worker-rollen från hello aktuella lösning eller skapa ett projekt för webb- eller worker-rollen.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-112">From hello context menu, select **Add**, then select an existing web role or worker role from hello current solution, or create a web or worker role project.</span></span> <span data-ttu-id="e3ed6-113">Du kan också Välj en lämplig, till exempel ett ASP.NET web application-projekt och associera det med ett rollprojekt.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-113">You can also select an appropriate project, such as an ASP.NET web application project, and associate it with a role project.</span></span>

    ![Menyn Alternativ tooadd en roll tooan Azure cloud service-projekt](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a><span data-ttu-id="e3ed6-115">Ta bort en roll från en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="e3ed6-115">Removing a role from an Azure cloud service</span></span>
<span data-ttu-id="e3ed6-116">hello hur följande steg du tar bort en webbplats eller arbetsprocess roll från ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-116">hello following steps guide you through removing a web or worker role from an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="e3ed6-117">Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-117">Create or open an Azure cloud service project in Visual Studio.</span></span>

1. <span data-ttu-id="e3ed6-118">I **Solution Explorer**, expandera hello projektnoden</span><span class="sxs-lookup"><span data-stu-id="e3ed6-118">In **Solution Explorer**, expand hello project node</span></span>

1. <span data-ttu-id="e3ed6-119">Expandera hello **roller** nod.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-119">Expand hello **Roles** node.</span></span>

1. <span data-ttu-id="e3ed6-120">Högerklicka på hello nod du tooremove och, hello snabbmenyn väljer **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-120">Right-click hello node you want tooremove, and, from hello context menu, select **Remove**.</span></span> 

    ![Menyn Alternativ tooadd roll-tooan Azure-molntjänst](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-tooan-azure-cloud-service-project"></a><span data-ttu-id="e3ed6-122">Readding en roll tooan Azure cloud service-projekt</span><span class="sxs-lookup"><span data-stu-id="e3ed6-122">Readding a role tooan Azure cloud service project</span></span>
<span data-ttu-id="e3ed6-123">Om du tar bort en roll från ditt molntjänstprojekt men vid ett senare tillfälle tooadd hello tillbaka rollen toohello projekt, endast hello rollen deklaration och grundläggande attribut, till exempel slutpunkter och diagnostik information har lagts till.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-123">If you remove a role from your cloud service project but later decide tooadd hello role back toohello project, only hello role declaration and basic attributes, such as endpoints and diagnostics information, are added.</span></span> <span data-ttu-id="e3ed6-124">Inga ytterligare resurser eller referenser läggs toohello `ServiceDefinition.csdef` fil eller toohello `ServiceConfiguration.cscfg` fil.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-124">No additional resources or references are added toohello `ServiceDefinition.csdef` file or toohello `ServiceConfiguration.cscfg` file.</span></span> <span data-ttu-id="e3ed6-125">Om du vill tooadd informationen måste toomanually lägga tillbaka det i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-125">If you want tooadd this information, you need toomanually add it back into these files.</span></span>

<span data-ttu-id="e3ed6-126">Till exempel kan du ta bort en webbtjänstrollen och du senare tooadd rollen tillbaka till din lösning.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-126">For example, you might remove a web service role and later you decide tooadd this role back into your solution.</span></span> <span data-ttu-id="e3ed6-127">Om du gör det uppstår ett fel.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-127">If you do this, an error occurs.</span></span> <span data-ttu-id="e3ed6-128">tooprevent det här felet ska du har tooadd hello `<LocalResources>` element som visas i följande hello XML tillbaka till hello `ServiceDefinition.csdef` fil.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-128">tooprevent this error, you have tooadd hello `<LocalResources>` element shown in hello following XML back into hello `ServiceDefinition.csdef` file.</span></span> <span data-ttu-id="e3ed6-129">Använd hello namnet på hello webbtjänstrollen som du har lagt till hello projekt som en del av hello namnattributet för hello  **<LocalStorage>**  element.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-129">Use hello name of hello web service role that you added back into hello project as part of hello name attribute for hello **<LocalStorage>** element.</span></span> <span data-ttu-id="e3ed6-130">I det här exemplet hello hello webbtjänstrollen heter **WCFServiceWebRole1**.</span><span class="sxs-lookup"><span data-stu-id="e3ed6-130">In this example, hello name of hello web service role is **WCFServiceWebRole1**.</span></span>

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a><span data-ttu-id="e3ed6-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3ed6-131">Next steps</span></span>
- [<span data-ttu-id="e3ed6-132">Konfigurera hello roller för en Azure-molntjänst med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3ed6-132">Configure hello Roles for an Azure cloud service with Visual Studio</span></span>](vs-azure-tools-configure-roles-for-cloud-service.md)
