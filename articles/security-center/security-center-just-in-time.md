---
title: "aaaJust i tid virtuella datorn åtkomst till i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet vägleder dig igenom hur precis i tid VM åtkomst i Azure Security Center hjälper dig att styra åtkomst till tooyour Azure virtuella datorer."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: terrylan
ms.openlocfilehash: e6b58dd2c686cb227392b294e85914df5a546016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a><span data-ttu-id="dc35d-103">Hantera virtuella åtkomst med hjälp av precis i tid</span><span class="sxs-lookup"><span data-stu-id="dc35d-103">Manage virtual machine access using just in time</span></span>

<span data-ttu-id="dc35d-104">Precis i tid virtuell dator (VM) vara åtkomst används toolock ned inkommande trafik tooyour virtuella Azure-datorer att minska exponeringen tooattacks och ger enkel åtkomst tooconnect tooVMs vid behov.</span><span class="sxs-lookup"><span data-stu-id="dc35d-104">Just in time virtual machine (VM) access can be used toolock down inbound traffic tooyour Azure VMs, reducing exposure tooattacks while providing easy access tooconnect tooVMs when needed.</span></span>

> [!NOTE]
> <span data-ttu-id="dc35d-105">hello just-in tiden funktionen är i förhandsvisning och finns på hello standardnivån av Security Center.</span><span class="sxs-lookup"><span data-stu-id="dc35d-105">hello just in time feature is in preview and available on hello Standard tier of Security Center.</span></span>  <span data-ttu-id="dc35d-106">Se [priser](security-center-pricing.md) toolearn mer om Security Center prisnivåer.</span><span class="sxs-lookup"><span data-stu-id="dc35d-106">See [Pricing](security-center-pricing.md) toolearn more about Security Center's pricing tiers.</span></span>
>
>

## <a name="attack-scenario"></a><span data-ttu-id="dc35d-107">Angrepp</span><span class="sxs-lookup"><span data-stu-id="dc35d-107">Attack scenario</span></span>

<span data-ttu-id="dc35d-108">Brute force-attacker ofta målportar för hantering som ett sätt toogain åtkomst tooa VM.</span><span class="sxs-lookup"><span data-stu-id="dc35d-108">Brute force attacks commonly target management ports as a means toogain access tooa VM.</span></span> <span data-ttu-id="dc35d-109">Om det lyckas, kan en angripare ta kontroll över hello VM och upprätta en fot i din miljö.</span><span class="sxs-lookup"><span data-stu-id="dc35d-109">If successful, an attacker can take control over hello VM and establish a foothold into your environment.</span></span>

<span data-ttu-id="dc35d-110">Enkelriktade tooreduce exponering tooa brute-force attack är toolimit hello tid att öppna en port.</span><span class="sxs-lookup"><span data-stu-id="dc35d-110">One way tooreduce exposure tooa brute force attack is toolimit hello amount of time that a port is open.</span></span> <span data-ttu-id="dc35d-111">Hanteringsportar gör inte behöver toobe öppna hela tiden.</span><span class="sxs-lookup"><span data-stu-id="dc35d-111">Management ports do not need toobe open at all times.</span></span> <span data-ttu-id="dc35d-112">De behöver bara toobe öppna medan du är ansluten toohello VM, till exempel tooperform management eller underhållsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dc35d-112">They only need toobe open while you are connected toohello VM, for example tooperform management or maintenance tasks.</span></span> <span data-ttu-id="dc35d-113">När precis i tid är aktiverat, Security Center använder [Nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) (NSG) regler som begränsar åtkomst toomanagement portar så att de inte kan riktas av angripare.</span><span class="sxs-lookup"><span data-stu-id="dc35d-113">When just in time is enabled, Security Center uses [Network Security Group](../virtual-network/virtual-networks-nsg.md) (NSG) rules, which restrict access toomanagement ports so they cannot be targeted by attackers.</span></span>

![Precis i tid scenario][1]

## <a name="how-does-just-in-time-access-work"></a><span data-ttu-id="dc35d-115">Hur just-in-time-åtkomst fungerar?</span><span class="sxs-lookup"><span data-stu-id="dc35d-115">How does just in time access work?</span></span>

<span data-ttu-id="dc35d-116">När precis i tid är aktiverat, låser Security Center ned inkommande trafik tooyour virtuella Azure-datorer genom att skapa en regel för NSG.</span><span class="sxs-lookup"><span data-stu-id="dc35d-116">When just in time is enabled, Security Center locks down inbound traffic tooyour Azure VMs by creating an NSG rule.</span></span> <span data-ttu-id="dc35d-117">Du väljer hello portar på hello VM toowhich inkommande trafik ska låsas.</span><span class="sxs-lookup"><span data-stu-id="dc35d-117">You select hello ports on hello VM toowhich inbound traffic will be locked down.</span></span> <span data-ttu-id="dc35d-118">De här portarna styrs av hello precis i Tidslösning.</span><span class="sxs-lookup"><span data-stu-id="dc35d-118">These ports are controlled by hello just in time solution.</span></span>

<span data-ttu-id="dc35d-119">När en användare begär åtkomst tooa VM, Security Center kontrollerar hello användaren har [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) behörigheter som ger skrivbehörighet för hello Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="dc35d-119">When a user requests access tooa VM, Security Center checks that hello user has [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) permissions that provide write access for hello Azure resource.</span></span> <span data-ttu-id="dc35d-120">Om de har skrivbehörighet, hello begäran har godkänts och Security Center konfigurerar automatiskt hello tooallow Nätverkssäkerhetsgrupper (NSG: er) för inkommande trafik toohello hanteringsportar för hello mängden tid du angett.</span><span class="sxs-lookup"><span data-stu-id="dc35d-120">If they have write permissions, hello request is approved and Security Center automatically configures hello Network Security Groups (NSGs) tooallow inbound traffic toohello management ports for hello amount of time you specified.</span></span> <span data-ttu-id="dc35d-121">När hello tid har gått ut, återställer Security Center hello NSG: er tootheir tidigare tillstånd.</span><span class="sxs-lookup"><span data-stu-id="dc35d-121">After hello time has expired, Security Center restores hello NSGs tootheir previous states.</span></span>

> [!NOTE]
> <span data-ttu-id="dc35d-122">Security Center just-in-time-åtkomst för VM stöder för närvarande endast virtuella datorer som distribueras via Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="dc35d-122">Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="dc35d-123">Mer information om hello klassiska och Resource Manager distributionsmodellerna finns toolearn [Azure Resource Manager och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="dc35d-123">toolearn more about hello classic and Resource Manager deployment models see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>
>
>

## <a name="using-just-in-time-access"></a><span data-ttu-id="dc35d-124">Med hjälp av just-in-time-åtkomst</span><span class="sxs-lookup"><span data-stu-id="dc35d-124">Using just in time access</span></span>

<span data-ttu-id="dc35d-125">Hej **precis i tid VM access** panelen på hello **Security Center** bladet visar hello antalet virtuella datorer som konfigurerats för just-in-tid åtkomst och hello antal godkända åtkomstbegäranden som görs i hello föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="dc35d-125">hello **Just in time VM access** tile on hello **Security Center** blade shows hello number of VMs configured for just in time access and hello number of approved access requests made in hello last week.</span></span>

<span data-ttu-id="dc35d-126">Välj hello **precis i tid VM access** panelen och hello **precis i tid VM access** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="dc35d-126">Select hello **Just in time VM access** tile and hello **Just in time VM access** blade opens.</span></span>

![Precis i tid VM åtkomst panelen][2]

<span data-ttu-id="dc35d-128">Hej **precis i tid VM access** bladet innehåller information om hello tillståndet för dina virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="dc35d-128">hello **Just in time VM access** blade provides information on hello state of your VMs:</span></span>

- <span data-ttu-id="dc35d-129">**Konfigurerad** -virtuella datorer som har konfigurerade toosupport just-in-time VM-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dc35d-129">**Configured** - VMs that have been configured toosupport just in time VM access.</span></span> <span data-ttu-id="dc35d-130">hello data presenteras för hello föregående vecka och inkluderar för varje VM hello antal godkända begäranden, datumet och tiden och senast användare.</span><span class="sxs-lookup"><span data-stu-id="dc35d-130">hello data presented is for hello last week and includes for each VM hello number of approved requests, last access date and time, and last user.</span></span>
- <span data-ttu-id="dc35d-131">**Rekommenderade** -virtuella datorer som stöder just-in-time-åtkomst för VM men inte har konfigurerats att.</span><span class="sxs-lookup"><span data-stu-id="dc35d-131">**Recommended** - VMs that can support just in time VM access but have not been configured to.</span></span> <span data-ttu-id="dc35d-132">Vi rekommenderar att du aktiverar precis i tid VM åtkomstkontroll för dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="dc35d-132">We recommend that you enable just in time VM access control for these VMs.</span></span> <span data-ttu-id="dc35d-133">Se [aktivera just-in-time-åtkomst för VM](#enable-just-in-time-vm-access).</span><span class="sxs-lookup"><span data-stu-id="dc35d-133">See [Enable just in time VM access](#enable-just-in-time-vm-access).</span></span>
- <span data-ttu-id="dc35d-134">**Ingen rekommendation** -orsaker till att en virtuell dator inte toobe som rekommenderas är:</span><span class="sxs-lookup"><span data-stu-id="dc35d-134">**No recommendation** - Reasons that can cause a VM not toobe recommended are:</span></span>
  - <span data-ttu-id="dc35d-135">Saknas NSG - hello precis i Tidslösning kräver en NSG toobe på plats.</span><span class="sxs-lookup"><span data-stu-id="dc35d-135">Missing NSG - hello just in time solution requires an NSG toobe in place.</span></span>
  - <span data-ttu-id="dc35d-136">Klassiska VM - Security Center just-in-time-åtkomst för VM stöder för närvarande endast virtuella datorer som distribueras via Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="dc35d-136">Classic VM - Security Center just in time VM access currently supports only VMs deployed through Azure Resource Manager.</span></span> <span data-ttu-id="dc35d-137">Klassisk distribution stöds inte av hello precis i Tidslösning.</span><span class="sxs-lookup"><span data-stu-id="dc35d-137">A classic deployment is not supported by hello just in time solution.</span></span>
  - <span data-ttu-id="dc35d-138">Andra - en virtuell dator finns i den här kategorin om hello precis i tid lösningen har inaktiverats i hello säkerhetsprincipen för hello prenumeration eller resursgrupp hello eller att hello VM saknar en offentlig IP-adress och inte har en NSG på plats.</span><span class="sxs-lookup"><span data-stu-id="dc35d-138">Other - A VM is in this category if hello just in time solution is turned off in hello security policy of hello subscription or hello resource group, or that hello VM is missing a public IP and doesn't have an NSG in place.</span></span>

## <a name="configuring-a-just-in-time-access-policy"></a><span data-ttu-id="dc35d-139">Konfigurera bara i tid åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="dc35d-139">Configuring a just in time access policy</span></span>

<span data-ttu-id="dc35d-140">tooselect hello virtuella datorer som du vill tooenable:</span><span class="sxs-lookup"><span data-stu-id="dc35d-140">tooselect hello VMs that you want tooenable:</span></span>

1. <span data-ttu-id="dc35d-141">På hello **precis i tid VM access** bladet, Välj hello **rekommenderas** fliken.</span><span class="sxs-lookup"><span data-stu-id="dc35d-141">On hello **Just in time VM access** blade, select hello **Recommended** tab.</span></span>

  ![Aktivera just-in-time-åtkomst][3]

2. <span data-ttu-id="dc35d-143">Under **VMs**, Välj hello virtuella datorer som du vill tooenable.</span><span class="sxs-lookup"><span data-stu-id="dc35d-143">Under **VMs**, select hello VMs that you want tooenable.</span></span> <span data-ttu-id="dc35d-144">Detta placerar en markering nästa tooa VM.</span><span class="sxs-lookup"><span data-stu-id="dc35d-144">This puts a checkmark next tooa VM.</span></span>
3. <span data-ttu-id="dc35d-145">Välj **aktivera JIT på virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-145">Select **Enable JIT on VMs**.</span></span>
4. <span data-ttu-id="dc35d-146">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-146">Select **Save**.</span></span>

### <a name="default-ports"></a><span data-ttu-id="dc35d-147">Standardportar</span><span class="sxs-lookup"><span data-stu-id="dc35d-147">Default ports</span></span>

<span data-ttu-id="dc35d-148">Du kan se hello standardportarna som Security Center rekommenderar att aktivera precis i tid.</span><span class="sxs-lookup"><span data-stu-id="dc35d-148">You can see hello default ports that Security Center recommends enabling just in time.</span></span>

1. <span data-ttu-id="dc35d-149">På hello **precis i tid VM access** bladet, Välj hello **rekommenderas** fliken.</span><span class="sxs-lookup"><span data-stu-id="dc35d-149">On hello **Just in time VM access** blade, select hello **Recommended** tab.</span></span>

  ![Visa standardportarna][6]

2. <span data-ttu-id="dc35d-151">Under **VMs**, Välj en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="dc35d-151">Under **VMs**, select a VM.</span></span> <span data-ttu-id="dc35d-152">Detta placerar en markering nästa toohello virtuella datorn och öppnar hello **JIT VM konfiguration** bladet.</span><span class="sxs-lookup"><span data-stu-id="dc35d-152">This puts a checkmark next toohello VM and opens hello **JIT VM access configuration** blade.</span></span> <span data-ttu-id="dc35d-153">Det här bladet visar hello standardportarna.</span><span class="sxs-lookup"><span data-stu-id="dc35d-153">This blade displays hello default ports.</span></span>

### <a name="add-ports"></a><span data-ttu-id="dc35d-154">Lägga till portar</span><span class="sxs-lookup"><span data-stu-id="dc35d-154">Add ports</span></span>

<span data-ttu-id="dc35d-155">Från hello **JIT VM konfiguration** bladet du kan också lägga till och konfigurera en ny port som du vill använda tooenable hello precis i Tidslösning.</span><span class="sxs-lookup"><span data-stu-id="dc35d-155">From hello **JIT VM access configuration** blade, you can also add and configure a new port on which you want tooenable hello just in time solution.</span></span>

1. <span data-ttu-id="dc35d-156">På hello **JIT VM konfiguration** bladet väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-156">On hello **JIT VM access configuration** blade, select **Add**.</span></span> <span data-ttu-id="dc35d-157">Då öppnas hello **Lägg till Portkonfiguration** bladet.</span><span class="sxs-lookup"><span data-stu-id="dc35d-157">This opens hello **Add port configuration** blade.</span></span>

  ![Portkonfiguration][7]

2. <span data-ttu-id="dc35d-159">På **Lägg till Portkonfiguration** bladet du identifiera hello port protokolltyp tillåtna IP-adresser för källa och maximal tid för begäran.</span><span class="sxs-lookup"><span data-stu-id="dc35d-159">On **Add port configuration** blade, you identify hello port, protocol type, allowed source IPs, and maximum request time.</span></span>

  <span data-ttu-id="dc35d-160">Tillåtna käll-IP-adresser är hello IP-adressintervall tillåtna tooget åtkomst vid ett godkänt förfrågan.</span><span class="sxs-lookup"><span data-stu-id="dc35d-160">Allowed source IPs are hello IP ranges allowed tooget access upon an approved request.</span></span>

  <span data-ttu-id="dc35d-161">Tid för maximal begäran är hello maximala tidsperioden att en specifik port kan öppnas.</span><span class="sxs-lookup"><span data-stu-id="dc35d-161">Maximum request time is hello maximum time window that a specific port can be opened.</span></span>

3. <span data-ttu-id="dc35d-162">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-162">Select **OK**.</span></span>

## <a name="requesting-access-tooa-vm"></a><span data-ttu-id="dc35d-163">Begär åtkomst tooa VM</span><span class="sxs-lookup"><span data-stu-id="dc35d-163">Requesting access tooa VM</span></span>

<span data-ttu-id="dc35d-164">toorequest åtkomst tooa VM:</span><span class="sxs-lookup"><span data-stu-id="dc35d-164">toorequest access tooa VM:</span></span>

1. <span data-ttu-id="dc35d-165">På hello **precis i tid VM access** bladet, Välj hello **konfigurerad** fliken.</span><span class="sxs-lookup"><span data-stu-id="dc35d-165">On hello **Just in time VM access** blade, select hello **Configured** tab.</span></span>
2. <span data-ttu-id="dc35d-166">Under **VMs**, Välj hello virtuella datorer som du vill tooenable åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dc35d-166">Under **VMs**, select hello VMs that you want tooenable access.</span></span> <span data-ttu-id="dc35d-167">Detta placerar en markering nästa tooa VM.</span><span class="sxs-lookup"><span data-stu-id="dc35d-167">This puts a checkmark next tooa VM.</span></span>
3. <span data-ttu-id="dc35d-168">Välj **begära åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-168">Select **Request access**.</span></span> <span data-ttu-id="dc35d-169">Då öppnas hello **begära åtkomst** bladet.</span><span class="sxs-lookup"><span data-stu-id="dc35d-169">This opens hello **Request access** blade.</span></span>

  ![Begär åtkomst tooa VM][4]

4. <span data-ttu-id="dc35d-171">På hello **begära åtkomst** bladet som du konfigurerar för varje VM hello portar tooopen tillsammans med hello käll-IP att hello port öppnas tooand hello tidsfönstret för vilka hello porten är öppen.</span><span class="sxs-lookup"><span data-stu-id="dc35d-171">On hello **Request access** blade, you configure for each VM hello ports tooopen along with hello source IP that hello port is opened tooand hello time window for which hello port is opened.</span></span> <span data-ttu-id="dc35d-172">Du kan begära åtkomst endast toohello portar som är konfigurerade i hello precis i tid princip.</span><span class="sxs-lookup"><span data-stu-id="dc35d-172">You can request access only toohello ports that are configured in hello just in time policy.</span></span> <span data-ttu-id="dc35d-173">Varje port har en maximal tillåten tid som härletts från hello precis i tid princip.</span><span class="sxs-lookup"><span data-stu-id="dc35d-173">Each port has a maximum allowed time derived from hello just in time policy.</span></span>
5. <span data-ttu-id="dc35d-174">Välj **öppna portarna**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-174">Select **Open ports**.</span></span>

## <a name="editing-a-just-in-time-access-policy"></a><span data-ttu-id="dc35d-175">Redigera bara i tid åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="dc35d-175">Editing a just in time access policy</span></span>

<span data-ttu-id="dc35d-176">Du kan ändra en virtuell dator finns bara i tid princip genom att lägga till och konfigurera en ny port tooopen för den virtuella datorn eller genom att ändra andra parametrar relaterade tooan skyddad redan port.</span><span class="sxs-lookup"><span data-stu-id="dc35d-176">You can change a VM's existing just in time policy by adding and configuring a new port tooopen for that VM, or by changing any other parameter related tooan already protected port.</span></span>

<span data-ttu-id="dc35d-177">I ordning tooedit en befintlig bara i en virtuell dator tid princip hello **konfigurerad** används:</span><span class="sxs-lookup"><span data-stu-id="dc35d-177">In order tooedit an existing just in time policy of a VM, hello **Configured** tab is used:</span></span>

1. <span data-ttu-id="dc35d-178">Under **VMs**, Välj ett VM-tooadd en port tooby att klicka på hello tre punkter inom hello rad för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="dc35d-178">Under **VMs**, select a VM tooadd a port tooby clicking on hello three dots within hello row for that VM.</span></span> <span data-ttu-id="dc35d-179">Då öppnas en meny.</span><span class="sxs-lookup"><span data-stu-id="dc35d-179">This opens a menu.</span></span>
2. <span data-ttu-id="dc35d-180">Välj **redigera** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="dc35d-180">Select **Edit** in hello menu.</span></span> <span data-ttu-id="dc35d-181">Då öppnas hello **JIT VM konfiguration** bladet.</span><span class="sxs-lookup"><span data-stu-id="dc35d-181">This opens hello **JIT VM access configuration** blade.</span></span>

  ![Redigera princip][8]

3. <span data-ttu-id="dc35d-183">På hello **JIT VM konfiguration** bladet du kan antingen redigera hello befintliga inställningar för redan skyddade porten genom att klicka på dess port eller kan du välja **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-183">On hello **JIT VM access configuration** blade, you can either edit hello existing settings of an already protected port by clicking on its port, or you can select **Add**.</span></span> <span data-ttu-id="dc35d-184">Då öppnas hello **Lägg till Portkonfiguration** bladet.</span><span class="sxs-lookup"><span data-stu-id="dc35d-184">This opens hello **Add port configuration** blade.</span></span>

  ![Lägga till en port][7]

4. <span data-ttu-id="dc35d-186">På **Lägg till Portkonfiguration** bladet identifiera hello port, protokolltyp, tillåtna käll-IP-adresser och tid för maximal begäran.</span><span class="sxs-lookup"><span data-stu-id="dc35d-186">On **Add port configuration** blade identify hello port, protocol type, allowed source IPs, and maximum request time.</span></span>
5. <span data-ttu-id="dc35d-187">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-187">Select **OK**.</span></span>
6. <span data-ttu-id="dc35d-188">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-188">Select **Save**.</span></span>

## <a name="auditing-just-in-time-access-activity"></a><span data-ttu-id="dc35d-189">Granskning precis i tid access-aktivitet</span><span class="sxs-lookup"><span data-stu-id="dc35d-189">Auditing just in time access activity</span></span>

<span data-ttu-id="dc35d-190">Du kan få insikter om VM aktiviteter loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="dc35d-190">You can gain insights into VM activities using log search.</span></span> <span data-ttu-id="dc35d-191">tooview loggar:</span><span class="sxs-lookup"><span data-stu-id="dc35d-191">tooview logs:</span></span>

1. <span data-ttu-id="dc35d-192">På hello **precis i tid VM access** bladet, Välj hello **konfigurerad** fliken.</span><span class="sxs-lookup"><span data-stu-id="dc35d-192">On hello **Just in time VM access** blade, select hello **Configured** tab.</span></span>
2. <span data-ttu-id="dc35d-193">Under **VMs**, Välj en VM tooview information om genom att klicka på hello tre punkter inom hello rad för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="dc35d-193">Under **VMs**, select a VM tooview information about by clicking on hello three dots within hello row for that VM.</span></span> <span data-ttu-id="dc35d-194">Då öppnas en meny.</span><span class="sxs-lookup"><span data-stu-id="dc35d-194">This opens a menu.</span></span>
3. <span data-ttu-id="dc35d-195">Välj **aktivitetsloggen** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="dc35d-195">Select **Activity Log** in hello menu.</span></span> <span data-ttu-id="dc35d-196">Då öppnas hello **aktivitetsloggen** bladet.</span><span class="sxs-lookup"><span data-stu-id="dc35d-196">This opens hello **Activity log** blade.</span></span>

![Välj aktivitetsloggen][9]

<span data-ttu-id="dc35d-198">Hej **aktivitetsloggen** bladet innehåller en filtrerad vy av tidigare åtgärder för den virtuella datorn tillsammans med tid och datum prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dc35d-198">hello **Activity log** blade provides a filtered view of previous operations for that VM along with time, date, and subscription.</span></span>

![Visa aktivitetsloggen][5]

<span data-ttu-id="dc35d-200">Du kan hämta hello logginformation genom att välja **Klicka här toodownload alla hello-objekt som CSV**.</span><span class="sxs-lookup"><span data-stu-id="dc35d-200">You can download hello log information by selecting **Click here toodownload all hello items as CSV**.</span></span>

<span data-ttu-id="dc35d-201">Ändra hello-filter och välj **tillämpa** toocreate Sök- och log.</span><span class="sxs-lookup"><span data-stu-id="dc35d-201">Modify hello filters and select **Apply** toocreate a search and log.</span></span>

## <a name="using-just-in-time-vm-access-via-powershell"></a><span data-ttu-id="dc35d-202">Med hjälp av precis i tid VM åtkomst via PowerShell</span><span class="sxs-lookup"><span data-stu-id="dc35d-202">Using just in time VM access via PowerShell</span></span>

<span data-ttu-id="dc35d-203">Kontrollera att du har hello i ordning toouse hello precis i Tidslösning via PowerShell, [senaste](/powershell/azure/install-azurerm-ps) Azure PowerShell-version.</span><span class="sxs-lookup"><span data-stu-id="dc35d-203">In order toouse hello just in time solution via PowerShell, make sure you have hello [latest](/powershell/azure/install-azurerm-ps) Azure PowerShell version.</span></span>
<span data-ttu-id="dc35d-204">När du gör det, måste tooinstall hello [senaste](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center från hello PowerShell-galleriet.</span><span class="sxs-lookup"><span data-stu-id="dc35d-204">Once you do, you need tooinstall hello [latest](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.12) Azure Security Center from hello PowerShell gallery.</span></span>

### <a name="configuring-a-just-in-time-policy-for-a-vm"></a><span data-ttu-id="dc35d-205">Konfigurera bara i tid princip för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="dc35d-205">Configuring a just in time policy for a VM</span></span>

<span data-ttu-id="dc35d-206">tooconfigure bara i tid princip på en specifik VM, behöver du toorun detta kommando i PowerShell-sessionen: Set-ASCJITAccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="dc35d-206">tooconfigure a just in time policy on a specific VM, you need toorun this command in your PowerShell session: Set-ASCJITAccessPolicy.</span></span>
<span data-ttu-id="dc35d-207">Följ hello cmdlet-dokumentationen toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="dc35d-207">Follow hello cmdlet documentation toolearn more.</span></span>

### <a name="requesting-access-tooa-vm"></a><span data-ttu-id="dc35d-208">Begär åtkomst tooa VM</span><span class="sxs-lookup"><span data-stu-id="dc35d-208">Requesting access tooa VM</span></span>

<span data-ttu-id="dc35d-209">tooaccess en specifik VM som skyddas med hello precis i tid lösningen måste du toorun detta kommando i PowerShell-sessionen: anropa ASCJITAccess.</span><span class="sxs-lookup"><span data-stu-id="dc35d-209">tooaccess a specific VM that is protected with hello just in time solution, you need toorun this command in your PowerShell session: Invoke-ASCJITAccess.</span></span>
<span data-ttu-id="dc35d-210">Följ hello cmdlet-dokumentationen toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="dc35d-210">Follow hello cmdlet documentation toolearn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc35d-211">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc35d-211">Next steps</span></span>
<span data-ttu-id="dc35d-212">I den här artikeln har du lärt dig hur precis i tid VM åtkomst i Security Center hjälper du styr åtkomst tooyour Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="dc35d-212">In this article, you learned how just in time VM access in Security Center helps you control access tooyour Azure virtual machines.</span></span>

<span data-ttu-id="dc35d-213">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="dc35d-213">toolearn more about Security Center, see hello following:</span></span>

- <span data-ttu-id="dc35d-214">[Ställa in säkerhetsprinciper](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="dc35d-214">[Setting security policies](security-center-policies.md) — Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
- <span data-ttu-id="dc35d-215">[Hantera säkerhetsrekommendationer](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="dc35d-215">[Managing security recommendations](security-center-recommendations.md) — Learn how recommendations help you protect your Azure resources.</span></span>
- <span data-ttu-id="dc35d-216">[Övervakning av säkerhetshälsa](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="dc35d-216">[Security health monitoring](security-center-monitoring.md) — Learn how toomonitor hello health of your Azure resources.</span></span>
- <span data-ttu-id="dc35d-217">[Hantera och åtgärda toosecurity aviseringar](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="dc35d-217">[Managing and responding toosecurity alerts](security-center-managing-and-responding-alerts.md) — Learn how toomanage and respond toosecurity alerts.</span></span>
- <span data-ttu-id="dc35d-218">[Övervaka partnerlösningar](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="dc35d-218">[Monitoring partner solutions](security-center-partner-solutions.md) — Learn how toomonitor hello health status of your partner solutions.</span></span>
- <span data-ttu-id="dc35d-219">[Vanliga frågor om Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dc35d-219">[Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
- <span data-ttu-id="dc35d-220">[Azures säkerhetsblogg](https://blogs.msdn.microsoft.com/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc35d-220">[Azure Security blog](https://blogs.msdn.microsoft.com/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
