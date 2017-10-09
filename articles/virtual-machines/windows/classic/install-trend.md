---
title: "aaaInstall Trend Micro djup Security på en virtuell dator | Microsoft Docs"
description: "Den här artikeln beskriver hur tooinstall och konfigurera Trend Micro säkerhet på en virtuell dator som skapats med hello klassiska distributionsmodellen i Azure."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: dc5492db07a37a2296df5da673183a14c6d5b1f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a><span data-ttu-id="20f36-103">Hur tooinstall och konfigurera Trend Micro djup säkerhet som en tjänst på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="20f36-103">How tooinstall and configure Trend Micro Deep Security as a Service on a Windows VM</span></span>
> [!IMPORTANT]
> <span data-ttu-id="20f36-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="20f36-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="20f36-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="20f36-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="20f36-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="20f36-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="20f36-107">Den här artikeln beskrivs hur du tooinstall och konfigurera Trend Micro djup säkerhet som en tjänst på en ny eller befintlig virtuell dator (VM) med Windows Server.</span><span class="sxs-lookup"><span data-stu-id="20f36-107">This article shows you how tooinstall and configure Trend Micro Deep Security as a Service on a new or existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="20f36-108">Djupgående säkerhet som en tjänst innehåller skydd mot skadlig kod, en brandvägg, ett intrång förebyggande system och integritet övervakning.</span><span class="sxs-lookup"><span data-stu-id="20f36-108">Deep Security as a Service includes anti-malware protection, a firewall, an intrusion prevention system, and integrity monitoring.</span></span>

<span data-ttu-id="20f36-109">hello-klienten installeras som ett tillägg för säkerhet via hello VM-agenten.</span><span class="sxs-lookup"><span data-stu-id="20f36-109">hello client is installed as a security extension via hello VM Agent.</span></span> <span data-ttu-id="20f36-110">På en ny virtuell dator installerar du hello djup säkerhet Agent som hello VM-agenten skapas automatiskt av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="20f36-110">On a new virtual machine, you install hello Deep Security Agent, as hello VM Agent is created automatically by hello Azure portal.</span></span>

<span data-ttu-id="20f36-111">En befintlig virtuell dator skapas med hello klassiska portalen, hello Azure CLI eller PowerShell kanske inte har en VM-agent.</span><span class="sxs-lookup"><span data-stu-id="20f36-111">An existing VM created using hello classic portal, hello Azure CLI, or PowerShell might not have a VM agent.</span></span> <span data-ttu-id="20f36-112">För en befintlig virtuell dator som inte har hello VM-agenten måste toodownload och installera den först.</span><span class="sxs-lookup"><span data-stu-id="20f36-112">For an existing virtual machine that doesn't have hello VM Agent, you need toodownload and install it first.</span></span> <span data-ttu-id="20f36-113">Den här artikeln täcker båda situationer.</span><span class="sxs-lookup"><span data-stu-id="20f36-113">This article covers both situations.</span></span>

<span data-ttu-id="20f36-114">Om du har någon aktuell prenumeration från Trend Micro för en lokal lösning kan du använda den toohelp skydda din virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="20f36-114">If you have a current subscription from Trend Micro for an on-premises solution, you can use it toohelp protect your Azure virtual machines.</span></span> <span data-ttu-id="20f36-115">Om du inte är en kund ännu kan registrera du dig för en utvärderingsprenumeration.</span><span class="sxs-lookup"><span data-stu-id="20f36-115">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="20f36-116">Mer information om den här lösningen finns hello Trend Micro blogginlägget [Microsoft Azure VM-tillägget för djup säkerhetsgrupper](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span><span class="sxs-lookup"><span data-stu-id="20f36-116">For more information about this solution, see hello Trend Micro blog post [Microsoft Azure VM Agent Extension For Deep Security](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span></span>

## <a name="install-hello-deep-security-agent-on-a-new-vm"></a><span data-ttu-id="20f36-117">Installera hello djup säkerhet Agent på en ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="20f36-117">Install hello Deep Security Agent on a new VM</span></span>

<span data-ttu-id="20f36-118">Hej [Azure-portalen](http://portal.azure.com) kan du installera tillägg för hello Trend Micro säkerhet när du använder en bild från hello **Marketplace** toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="20f36-118">hello [Azure portal](http://portal.azure.com) lets you install hello Trend Micro security extension when you use an image from hello **Marketplace** toocreate hello virtual machine.</span></span> <span data-ttu-id="20f36-119">Om du skapar en virtuell dator är med hjälp av hello portal ett enkelt sätt tooadd skydd från Trend Micro.</span><span class="sxs-lookup"><span data-stu-id="20f36-119">If you're creating a single virtual machine, using hello portal is an easy way tooadd protection from Trend Micro.</span></span>

<span data-ttu-id="20f36-120">Med hjälp av en transaktion från hello **Marketplace** öppnas en guide som hjälper dig att konfigurera hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="20f36-120">Using an entry from hello **Marketplace** opens a wizard that helps you set up hello virtual machine.</span></span> <span data-ttu-id="20f36-121">Du använder hello **inställningar** bladet, hello tredje panelen hello guiden tooinstall hello Trend Micro säkerhet tillägg.</span><span class="sxs-lookup"><span data-stu-id="20f36-121">You use hello **Settings** blade, hello third panel of hello wizard, tooinstall hello Trend Micro security extension.</span></span>  <span data-ttu-id="20f36-122">Allmänna instruktioner finns i [skapa en virtuell dator som kör Windows i hello Azure-portalen](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="20f36-122">For general instructions, see [Create a virtual machine running Windows in hello Azure portal](tutorial.md).</span></span>

<span data-ttu-id="20f36-123">När du får toohello **inställningar** bladet hello guiden hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="20f36-123">When you get toohello **Settings** blade of hello wizard, do hello following steps:</span></span>

1. <span data-ttu-id="20f36-124">Klicka på **tillägg**, klicka på **lägga till tillägget** i hello nästa ruta.</span><span class="sxs-lookup"><span data-stu-id="20f36-124">Click **Extensions**, then click **Add extension** in hello next pane.</span></span>

   ![Börja lägga till hello-tillägg][1]

2. <span data-ttu-id="20f36-126">Välj **djup säkerhet Agent** i hello **ny resurs** fönstret.</span><span class="sxs-lookup"><span data-stu-id="20f36-126">Select **Deep Security Agent** in hello **New resource** pane.</span></span> <span data-ttu-id="20f36-127">I hello djup säkerhet Agent klickar **skapa**.</span><span class="sxs-lookup"><span data-stu-id="20f36-127">In hello Deep Security Agent pane, click **Create**.</span></span>

   ![Identifiera Agent djup säkerhet][2]

3. <span data-ttu-id="20f36-129">Ange hello **klient-ID** och **klient aktivering lösenord** för hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="20f36-129">Enter hello **Tenant Identifier** and **Tenant Activation Password** for hello extension.</span></span> <span data-ttu-id="20f36-130">Alternativt kan du ange en **princip säkerhetsidentifieraren**.</span><span class="sxs-lookup"><span data-stu-id="20f36-130">Optionally, you can enter a **Security Policy Identifier**.</span></span> <span data-ttu-id="20f36-131">Klicka på **OK** tooadd hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="20f36-131">Then, click **OK** tooadd hello client.</span></span>

   ![Ange information om webbprogramtillägg][3]

## <a name="install-hello-deep-security-agent-on-an-existing-vm"></a><span data-ttu-id="20f36-133">Installera hello djup säkerhet Agent på en befintlig virtuell dator</span><span class="sxs-lookup"><span data-stu-id="20f36-133">Install hello Deep Security Agent on an existing VM</span></span>
<span data-ttu-id="20f36-134">tooinstall hello-agenten på en befintlig virtuell dator, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="20f36-134">tooinstall hello agent on an existing VM, you need hello following items:</span></span>

* <span data-ttu-id="20f36-135">hello Azure PowerShell-modulen, version 0.8.2 eller senare, installeras på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="20f36-135">hello Azure PowerShell module, version 0.8.2 or newer, installed on your local computer.</span></span> <span data-ttu-id="20f36-136">Du kan kontrollera hello-versionen av Azure PowerShell som du har installerat med hjälp av hello **Get-Module azure | format-table version** kommando.</span><span class="sxs-lookup"><span data-stu-id="20f36-136">You can check hello version of Azure PowerShell that you have installed by using hello **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="20f36-137">Instruktioner och en länk toohello senaste versionen finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="20f36-137">For instructions and a link toohello latest version, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="20f36-138">Logga in tooyour Azure-prenumeration med `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="20f36-138">Log in tooyour Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="20f36-139">hello VM-agenten är installerad på hello virtuella måldatorn.</span><span class="sxs-lookup"><span data-stu-id="20f36-139">hello VM Agent installed on hello target virtual machine.</span></span>

<span data-ttu-id="20f36-140">Kontrollera först att hello VM-agenten redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="20f36-140">First, verify that hello VM Agent is already installed.</span></span> <span data-ttu-id="20f36-141">Fyll i hello molntjänstnamnet och namn på virtuell dator och kör sedan följande kommandon i en administratörsnivå Azure PowerShell-kommandotolk hello.</span><span class="sxs-lookup"><span data-stu-id="20f36-141">Fill in hello cloud service name and virtual machine name, and then run hello following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="20f36-142">Ersätt allt inom hello citattecken, inklusive Hej < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="20f36-142">Replace everything within hello quotes, including hello < and > characters.</span></span>

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

<span data-ttu-id="20f36-143">Om du inte vet hello Molntjänsten och namn på virtuell dator, kör **Get-AzureVM** toodisplay att hello information för alla virtuella datorer i din aktuella prenumeration.</span><span class="sxs-lookup"><span data-stu-id="20f36-143">If you don't know hello cloud service and virtual machine name, run **Get-AzureVM** toodisplay that information for all hello virtual machines in your current subscription.</span></span>

<span data-ttu-id="20f36-144">Om hello **write-host** kommandot returnerar **SANT**, hello VM-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="20f36-144">If hello **write-host** command returns **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="20f36-145">Om den returnerar **FALSKT**, se hello instruktioner och en länk toohello hämta i hello Azure blogginlägget [Agent för VM-tillägg - del 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span><span class="sxs-lookup"><span data-stu-id="20f36-145">If it returns **False**, see hello instructions and a link toohello download in hello Azure blog post [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span></span>

<span data-ttu-id="20f36-146">Om hello VM-agenten är installerad kan du köra dessa kommandon.</span><span class="sxs-lookup"><span data-stu-id="20f36-146">If hello VM Agent is installed, run these commands.</span></span>

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="20f36-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20f36-147">Next steps</span></span>
<span data-ttu-id="20f36-148">Det tar några minuter för hello agent toostart körs när den är installerad.</span><span class="sxs-lookup"><span data-stu-id="20f36-148">It takes a few minutes for hello agent toostart running when it is installed.</span></span> <span data-ttu-id="20f36-149">Efter det måste tooactivate djup säkerhet på hello virtuella datorn så att den kan hanteras av en djup Security Manager.</span><span class="sxs-lookup"><span data-stu-id="20f36-149">After that, you need tooactivate Deep Security on hello virtual machine so it can be managed by a Deep Security Manager.</span></span> <span data-ttu-id="20f36-150">Se hello följande artiklar för mer information:</span><span class="sxs-lookup"><span data-stu-id="20f36-150">See hello following articles for additional instructions:</span></span>

* <span data-ttu-id="20f36-151">Trends artikel om den här lösningen [Instant-On Cloud Security för Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span><span class="sxs-lookup"><span data-stu-id="20f36-151">Trend's article about this solution, [Instant-On Cloud Security for Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span></span>
* <span data-ttu-id="20f36-152">En [exempel på Windows PowerShell-skript](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="20f36-152">A [sample Windows PowerShell script](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtual machine</span></span>
* <span data-ttu-id="20f36-153">[Instruktioner](http://go.microsoft.com/fwlink/?LinkId=404099) hello-exempel</span><span class="sxs-lookup"><span data-stu-id="20f36-153">[Instructions](http://go.microsoft.com/fwlink/?LinkId=404099) for hello sample</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20f36-154">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="20f36-154">Additional resources</span></span>
<span data-ttu-id="20f36-155">[Hur toolog på tooa virtuell dator som kör Windows Server]</span><span class="sxs-lookup"><span data-stu-id="20f36-155">[How toolog on tooa virtual machine running Windows Server]</span></span>

<span data-ttu-id="20f36-156">[Azure VM-tillägg och funktioner]</span><span class="sxs-lookup"><span data-stu-id="20f36-156">[Azure VM Extensions and features]</span></span>

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Hur toolog på tooa virtuell dator som kör Windows Server]:connect-logon.md
[Azure VM-tillägg och funktioner]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
