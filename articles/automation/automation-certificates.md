---
title: "aaaCertificate tillgångar i Azure Automation | Microsoft Docs"
description: "Certifikat kan lagras på ett säkert sätt i Azure Automation så att de kan nås av runbooks eller DSC-konfigurationer tooauthenticate mot Azure och resurser från tredje part.  Den här artikeln förklarar hello information om certifikat och hur toowork med dem i både text och grafiska redigering."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 2c25bee937890438ea9022669be2c24c77a110b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-assets-in-azure-automation"></a><span data-ttu-id="72db3-104">Certifikattillgångar i Azure Automation</span><span class="sxs-lookup"><span data-stu-id="72db3-104">Certificate assets in Azure Automation</span></span>

<span data-ttu-id="72db3-105">Certifikat kan lagras på ett säkert sätt i Azure Automation så att de kan nås av runbooks eller DSC-konfigurationer med hjälp av hello **Get-AzureRmAutomationRmCertificate** aktivitet för Azure Resource Manager-resurser.</span><span class="sxs-lookup"><span data-stu-id="72db3-105">Certificates can be stored securely in Azure Automation so they can be accessed by runbooks or DSC configurations using hello **Get-AzureRmAutomationRmCertificate** activity for Azure Resource Manager resources.</span></span> <span data-ttu-id="72db3-106">Detta gör att du toocreate runbooks och DSC-konfigurationer som använder certifikat för autentisering eller lägger till dem tooAzure eller tredje parts resurser.</span><span class="sxs-lookup"><span data-stu-id="72db3-106">This allows you toocreate runbooks and DSC configurations that use certificates for authentication or adds them tooAzure or third party resources.</span></span>

> [!NOTE] 
> <span data-ttu-id="72db3-107">Säkra tillgångar i Azure Automation inkluderar autentiseringsuppgifter, certifikat, anslutningar och krypterade variabler.</span><span class="sxs-lookup"><span data-stu-id="72db3-107">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="72db3-108">Dessa tillgångar krypteras och lagras i hello Azure Automation med en unik nyckel som skapas för varje automation-konto.</span><span class="sxs-lookup"><span data-stu-id="72db3-108">These assets are encrypted and stored in hello Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="72db3-109">Den här nyckeln är krypterad med ett certifikat för master och lagras i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="72db3-109">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="72db3-110">Innan de lagras en säker tillgång hello nyckel för hello automation-konto dekrypteras med hello master certifikatet och sedan används tooencrypt hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="72db3-110">Before storing a secure asset, hello key for hello automation account is decrypted using hello master certificate and then used tooencrypt hello asset.</span></span>
> 

## <a name="windows-powershell-cmdlets"></a><span data-ttu-id="72db3-111">Windows PowerShell-Cmdlets</span><span class="sxs-lookup"><span data-stu-id="72db3-111">Windows PowerShell Cmdlets</span></span>

<span data-ttu-id="72db3-112">hello-cmdlets i följande tabell hello är används toocreate och hantera automatisering certifikattillgångar med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="72db3-112">hello cmdlets in hello following table are used toocreate and manage automation certificate assets with Windows PowerShell.</span></span> <span data-ttu-id="72db3-113">De levereras som en del av hello [Azure PowerShell-modulen](../powershell-install-configure.md) som är tillgänglig för användning i Automation-runbooks och DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="72db3-113">They ship as part of hello [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configurations.</span></span>

|<span data-ttu-id="72db3-114">Cmdlet: ar</span><span class="sxs-lookup"><span data-stu-id="72db3-114">Cmdlets</span></span>|<span data-ttu-id="72db3-115">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="72db3-115">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="72db3-116">Get-AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="72db3-116">Get-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603765.aspx)|<span data-ttu-id="72db3-117">Hämtar information om ett certifikat toouse i en runbook eller DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="72db3-117">Retrieves information about a certificate toouse in a runbook or DSC configuration.</span></span> <span data-ttu-id="72db3-118">Du kan bara hämta hello certifikatet från Get-AutomationCertificate aktivitet.</span><span class="sxs-lookup"><span data-stu-id="72db3-118">You can only retrieve hello certificate itself from Get-AutomationCertificate activity.</span></span>|
|[<span data-ttu-id="72db3-119">Ny AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="72db3-119">New-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603604.aspx)|<span data-ttu-id="72db3-120">Skapar ett nytt certifikat till Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="72db3-120">Creates a new certificate into Azure Automation.</span></span>|
[<span data-ttu-id="72db3-121">Ta bort AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="72db3-121">Remove-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603529.aspx)|<span data-ttu-id="72db3-122">Tar bort ett certifikat från Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="72db3-122">Removes a certificate from Azure Automation.</span></span>|<span data-ttu-id="72db3-123">Skapar ett nytt certifikat till Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="72db3-123">Creates a new certificate into Azure Automation.</span></span>
|[<span data-ttu-id="72db3-124">Ange AzureRmAutomationCertificate</span><span class="sxs-lookup"><span data-stu-id="72db3-124">Set-AzureRmAutomationCertificate</span></span>](https://msdn.microsoft.com/library/mt603760.aspx)|<span data-ttu-id="72db3-125">Anger hello egenskaperna för ett befintligt certifikat, inklusive överföring hello certifikatfilen och inställning hello lösenordet för en PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="72db3-125">Sets hello properties for an existing certificate including uploading hello certificate file and setting hello password for a .pfx.</span></span>|
|[<span data-ttu-id="72db3-126">Lägg till AzureCertificate</span><span class="sxs-lookup"><span data-stu-id="72db3-126">Add-AzureCertificate</span></span>](https://msdn.microsoft.com/library/azure/dn495214.aspx)|<span data-ttu-id="72db3-127">Överföringar som ett tjänstcertifikat för hello angetts tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="72db3-127">Uploads a service certificate for hello specified cloud service.</span></span>|


## <a name="creating-a-new-certificate"></a><span data-ttu-id="72db3-128">Skapa ett nytt certifikat</span><span class="sxs-lookup"><span data-stu-id="72db3-128">Creating a new certificate</span></span>

<span data-ttu-id="72db3-129">När du skapar ett nytt certifikat kan du ladda upp en CER- eller PFX-filen-tooAzure Automation.</span><span class="sxs-lookup"><span data-stu-id="72db3-129">When you create a new certificate, you upload a .cer or .pfx file tooAzure Automation.</span></span> <span data-ttu-id="72db3-130">Om du markerar hello certifikat kan exporteras, kan du överföra den utanför hello Azure Automation-certifikatarkivet.</span><span class="sxs-lookup"><span data-stu-id="72db3-130">If you mark hello certificate as exportable, then you can transfer it out of hello Azure Automation certificate store.</span></span> <span data-ttu-id="72db3-131">Om du inte kan exporteras kan sedan den endast användas för signering inom hello runbook eller DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="72db3-131">If it is not exportable, then it can only be used for signing within hello runbook or DSC configuration.</span></span>


### <a name="toocreate-a-new-certificate-with-hello-azure-portal"></a><span data-ttu-id="72db3-132">toocreate ett nytt certifikat med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="72db3-132">toocreate a new certificate with hello Azure portal</span></span>

1. <span data-ttu-id="72db3-133">Från ditt Automation-konto klickar du på hello **tillgångar** panelen tooopen hello **tillgångar** bladet.</span><span class="sxs-lookup"><span data-stu-id="72db3-133">From your Automation account, click hello **Assets** tile tooopen hello **Assets** blade.</span></span>
1. <span data-ttu-id="72db3-134">Klicka på hello **certifikat** panelen tooopen hello **certifikat** bladet.</span><span class="sxs-lookup"><span data-stu-id="72db3-134">Click hello **Certificates** tile tooopen hello **Certificates** blade.</span></span>
1. <span data-ttu-id="72db3-135">Klicka på **lägga till ett certifikat** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="72db3-135">Click **Add a certificate** at hello top of hello blade.</span></span>
2. <span data-ttu-id="72db3-136">Ange ett namn för hello certifikatet i hello **namn** rutan.</span><span class="sxs-lookup"><span data-stu-id="72db3-136">Type a name for hello certificate in hello **Name** box.</span></span>
2. <span data-ttu-id="72db3-137">Klicka på **Välj en fil** under **överför en certifikatfil** toobrowse för en CER- eller PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="72db3-137">Click **Select a file** under **Upload a certificate file** toobrowse for a .cer or .pfx file.</span></span>  <span data-ttu-id="72db3-138">Om du väljer en .pfx-fil, ange ett lösenord och om den ska tillåtas toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="72db3-138">If you select a .pfx file, specify a password and whether it should be allowed toobe exported.</span></span>
1. <span data-ttu-id="72db3-139">Klicka på **skapa** toosave hello nya certifikat tillgången.</span><span class="sxs-lookup"><span data-stu-id="72db3-139">Click **Create** toosave hello new certificate asset.</span></span>


### <a name="toocreate-a-new-certificate-with-windows-powershell"></a><span data-ttu-id="72db3-140">toocreate ett nytt certifikat med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="72db3-140">toocreate a new certificate with Windows PowerShell</span></span>

<span data-ttu-id="72db3-141">hello exemplet nedan visar hur toocreate en ny Automation-certifikatet och markera det kan exporteras.</span><span class="sxs-lookup"><span data-stu-id="72db3-141">hello following example demonstrates how toocreate a new Automation certificate and mark it exportable.</span></span> <span data-ttu-id="72db3-142">Detta importerar en befintlig .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="72db3-142">This imports an existing .pfx file.</span></span>

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a><span data-ttu-id="72db3-143">Använder ett certifikat</span><span class="sxs-lookup"><span data-stu-id="72db3-143">Using a certificate</span></span>

<span data-ttu-id="72db3-144">Du måste använda hello **Get-AutomationCertificate** aktiviteten toouse ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="72db3-144">You must use hello **Get-AutomationCertificate** activity toouse a certificate.</span></span> <span data-ttu-id="72db3-145">Du kan inte använda hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet eftersom den returnerar information om hello certifikattillgång men inte hello själva certifikaten.</span><span class="sxs-lookup"><span data-stu-id="72db3-145">You cannot use hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) cmdlet since it returns information about hello certificate asset but not hello certificate itself.</span></span>

### <a name="textual-runbook-sample"></a><span data-ttu-id="72db3-146">Text-runbook-exempel</span><span class="sxs-lookup"><span data-stu-id="72db3-146">Textual runbook sample</span></span>

<span data-ttu-id="72db3-147">hello följande exempelkod visar hur tooadd en certifikat-tooa Molntjänsten i en runbook.</span><span class="sxs-lookup"><span data-stu-id="72db3-147">hello following sample code shows how tooadd a certificate tooa cloud service in a runbook.</span></span> <span data-ttu-id="72db3-148">I det här exemplet hämtas hello lösenord från en krypterad automation-variabel.</span><span class="sxs-lookup"><span data-stu-id="72db3-148">In this sample, hello password is retrieved from an encrypted automation variable.</span></span>

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a><span data-ttu-id="72db3-149">Grafisk runbook-exempel</span><span class="sxs-lookup"><span data-stu-id="72db3-149">Graphical runbook sample</span></span>

<span data-ttu-id="72db3-150">Du lägger till en **Get-AutomationCertificate** tooa grafisk runbook genom att högerklicka på certifikatet hello hello biblioteket ruta hello grafiska redigerare och välja **lägga till toocanvas**.</span><span class="sxs-lookup"><span data-stu-id="72db3-150">You add a **Get-AutomationCertificate** tooa graphical runbook by right-clicking on hello certificate in hello Library pane of hello graphical editor and selecting **Add toocanvas**.</span></span>

![Lägg till certifikat toohello arbetsytan](media/automation-certificates/automation-certificate-add-to-canvas.png)

<span data-ttu-id="72db3-152">hello visar följande bild ett exempel på hur du använder ett certifikat i en grafisk runbook.</span><span class="sxs-lookup"><span data-stu-id="72db3-152">hello following image shows an example of using a certificate in a graphical runbook.</span></span>  <span data-ttu-id="72db3-153">Detta är hello samma exemplet ovan för att lägga till en molntjänst för tooa av certifikat från en textrepresentation runbook.</span><span class="sxs-lookup"><span data-stu-id="72db3-153">This is hello same example shown above for adding a certificate tooa cloud service from a textual runbook.</span></span>

![<span data-ttu-id="72db3-154">Exempel grafiska redigering</span><span class="sxs-lookup"><span data-stu-id="72db3-154">Example Graphical Authoring</span></span> ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a><span data-ttu-id="72db3-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72db3-155">Next steps</span></span>

- <span data-ttu-id="72db3-156">toolearn mer om hur du arbetar med länkar toocontrol hello logiska flödet av aktiviteter din runbook är utformad tooperform, se [länkar i grafiska redigering](automation-graphical-authoring-intro.md#links-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="72db3-156">toolearn more about working with links toocontrol hello logical flow of activities your runbook is designed tooperform, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow).</span></span> 
