---
title: "aaaDeploy hello Site Recovery mobilitetstjänsten med Azure Automation DSC | Microsoft Docs"
description: "Beskriver hur toouse Azure Automation DSC tooautomatically distribuera hello Azure Site Recovery mobilitetstjänsten och Azure-agenten för VMware VM och fysisk server replication tooAzure"
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: krnese
ms.openlocfilehash: 52cdd13ceb61718a21137180c55db86919af5929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a><span data-ttu-id="352b8-103">Distribuera hello mobilitetstjänsten med Azure Automation DSC för replikering av virtuell dator</span><span class="sxs-lookup"><span data-stu-id="352b8-103">Deploy hello Mobility service with Azure Automation DSC for replication of VM</span></span>
<span data-ttu-id="352b8-104">I Operations Management Suite ger vi dig en omfattande säkerhetskopiering och haveriberedskapslösning som du kan använda som en del av en kontinuitetsplan.</span><span class="sxs-lookup"><span data-stu-id="352b8-104">In Operations Management Suite, we provide you with a comprehensive backup and disaster recovery solution that you can use as part of your business continuity plan.</span></span>

<span data-ttu-id="352b8-105">Vi har startats denna resa tillsammans med Hyper-V med hjälp av Hyper-V-replikering.</span><span class="sxs-lookup"><span data-stu-id="352b8-105">We started this journey together with Hyper-V by using Hyper-V Replica.</span></span> <span data-ttu-id="352b8-106">Men vi har utökade toosupport heterogena installationen eftersom kunder har flera hypervisorer och plattformar i sina moln.</span><span class="sxs-lookup"><span data-stu-id="352b8-106">But we have expanded toosupport a heterogeneous setup because customers have multiple hypervisors and platforms in their clouds.</span></span>

<span data-ttu-id="352b8-107">Om du kör VMware arbetsbelastningar och/eller fysiska servrar idag, kör en hanteringsserver alla hello Azure Site Recovery-komponenter i din miljö toohandle hello kommunikations- och replikering med Azure, när Azure är målet.</span><span class="sxs-lookup"><span data-stu-id="352b8-107">If you are running VMware workloads and/or physical servers today, a management server runs all of hello Azure Site Recovery components in your environment toohandle hello communication and data replication with Azure, when Azure is your destination.</span></span>

## <a name="deploy-hello-site-recovery-mobility-service-by-using-automation-dsc"></a><span data-ttu-id="352b8-108">Distribuera hello på Site Recovery-mobilitetstjänsten med hjälp av Automation DSC</span><span class="sxs-lookup"><span data-stu-id="352b8-108">Deploy hello Site Recovery Mobility service by using Automation DSC</span></span>
<span data-ttu-id="352b8-109">Låt oss börja genom att göra en snabb uppdelning av den här hanteringsservern har.</span><span class="sxs-lookup"><span data-stu-id="352b8-109">Let's start by doing a quick breakdown of what this management server does.</span></span>

<span data-ttu-id="352b8-110">hello management-servern kör flera serverroller.</span><span class="sxs-lookup"><span data-stu-id="352b8-110">hello management server runs several server roles.</span></span> <span data-ttu-id="352b8-111">En av dessa roller är *configuration*, som samordnar kommunikationen och hanterar processer för replikering och återställning av data.</span><span class="sxs-lookup"><span data-stu-id="352b8-111">One of these roles is *configuration*, which coordinates communication and manages data replication and recovery processes.</span></span>

<span data-ttu-id="352b8-112">Dessutom hello *processen* roll som fungerar som en replikerings-gateway.</span><span class="sxs-lookup"><span data-stu-id="352b8-112">In addition, hello *process* role acts as a replication gateway.</span></span> <span data-ttu-id="352b8-113">Den här rollen tar emot replikeringsdata från skyddade källdatorer, optimerar dem med cachelagring, komprimering och kryptering och skickar sedan tooan Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="352b8-113">This role receives replication data from protected source machines, optimizes it with caching, compression, and encryption, and then sends it tooan Azure storage account.</span></span> <span data-ttu-id="352b8-114">En av hello funktioner för hello Processroll är också toopush installation av hello Mobility service tooprotected datorer och utför automatisk identifiering av virtuella VMware-datorer.</span><span class="sxs-lookup"><span data-stu-id="352b8-114">One of hello functions for hello process role is also toopush installation of hello Mobility service tooprotected machines and perform automatic discovery of VMware VMs.</span></span>

<span data-ttu-id="352b8-115">Om det finns en återställning från Azure, hello *huvudmålservern* roll hanterar replikeringsdata hello som en del av den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="352b8-115">If there's a failback from Azure, hello *master target* role will handle hello replication data as part of this operation.</span></span>

<span data-ttu-id="352b8-116">För hello skyddade datorer, vi förlitar sig på hello *mobilitetstjänsten*.</span><span class="sxs-lookup"><span data-stu-id="352b8-116">For hello protected machines, we rely on hello *Mobility service*.</span></span> <span data-ttu-id="352b8-117">Den här komponenten är distribuerade tooevery dator (VMware VM eller fysiska server) som du vill tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="352b8-117">This component is deployed tooevery machine (VMware VM or physical server) that you want tooreplicate tooAzure.</span></span> <span data-ttu-id="352b8-118">Den samlar in dataskrivningar på hello datorn och vidarebefordrar dem toohello hanteringsservern (Processroll).</span><span class="sxs-lookup"><span data-stu-id="352b8-118">It captures data writes on hello machine and forwards them toohello management server (process role).</span></span>

<span data-ttu-id="352b8-119">När du behandlar kontinuitet för företag är viktigt toounderstand dina arbetsbelastningar, din infrastruktur och hello komponenter inblandade.</span><span class="sxs-lookup"><span data-stu-id="352b8-119">When you're dealing with business continuity, it's important toounderstand your workloads, your infrastructure, and hello components involved.</span></span> <span data-ttu-id="352b8-120">Du kan sedan uppfyller hello krav för recovery tid mål för Återställningstid och mål för återställningspunkt (RPO).</span><span class="sxs-lookup"><span data-stu-id="352b8-120">You can then meet hello requirements for your recovery time objective (RTO) and recovery point objective (RPO).</span></span> <span data-ttu-id="352b8-121">I den här kontexten hello mobilitetstjänsten är viktiga tooensuring som dina arbetsbelastningar skyddas som förväntat.</span><span class="sxs-lookup"><span data-stu-id="352b8-121">In this context, hello Mobility service is key tooensuring that your workloads are protected as you would expect.</span></span>

<span data-ttu-id="352b8-122">Hur kan du, på ett optimerat sätt se till att du har en tillförlitlig skyddade installationen med hjälp av vissa Operations Management Suite-komponenter?</span><span class="sxs-lookup"><span data-stu-id="352b8-122">So how can you, in an optimized way, ensure that you have a reliable protected setup with help from some Operations Management Suite components?</span></span>

<span data-ttu-id="352b8-123">Den här artikeln innehåller ett exempel på hur du kan använda Azure Automation önskad tillstånd Configuration (DSC), tillsammans med Site Recovery tooensure som:</span><span class="sxs-lookup"><span data-stu-id="352b8-123">This article provides an example of how you can use Azure Automation Desired State Configuration (DSC), together with Site Recovery, tooensure that:</span></span>

* <span data-ttu-id="352b8-124">Hej mobilitetstjänsten och Virtuella Azure-agenten är distribuerade toohello Windows-datorer som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="352b8-124">hello Mobility service and Azure VM agent are deployed toohello Windows machines that you want tooprotect.</span></span>
* <span data-ttu-id="352b8-125">Hej mobilitetstjänsten och Virtuella Azure-agenten körs alltid när Azure är hello replikeringsmål.</span><span class="sxs-lookup"><span data-stu-id="352b8-125">hello Mobility service and Azure VM agent are always running when Azure is hello replication target.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="352b8-126">Krav</span><span class="sxs-lookup"><span data-stu-id="352b8-126">Prerequisites</span></span>
* <span data-ttu-id="352b8-127">En databas toostore hello som krävs för installationen</span><span class="sxs-lookup"><span data-stu-id="352b8-127">A repository toostore hello required setup</span></span>
* <span data-ttu-id="352b8-128">En databas toostore hello krävs lösenfras tooregister hello management server</span><span class="sxs-lookup"><span data-stu-id="352b8-128">A repository toostore hello required passphrase tooregister with hello management server</span></span>

  > [!NOTE]
  > <span data-ttu-id="352b8-129">En unik lösenfras genereras för varje hanteringsserver.</span><span class="sxs-lookup"><span data-stu-id="352b8-129">A unique passphrase is generated for each management server.</span></span> <span data-ttu-id="352b8-130">Om du vill toodeploy flera hanteringsservrar blir har tooensure som hello rätt lösenfras lagras i hello passphrase.txt filen.</span><span class="sxs-lookup"><span data-stu-id="352b8-130">If you are going toodeploy multiple management servers, you have tooensure that hello correct passphrase is stored in hello passphrase.txt file.</span></span>
  >
  >
* <span data-ttu-id="352b8-131">Windows Management Framework (WMF) 5.0 installerat på hello datorer som du vill tooenable för skydd (ett krav för Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="352b8-131">Windows Management Framework (WMF) 5.0 installed on hello machines that you want tooenable for protection (a requirement for Automation DSC)</span></span>

  > [!NOTE]
  > <span data-ttu-id="352b8-132">Om du vill toouse DSC för Windows-datorer som har WMF 4.0 installerat avsnittet hello [Använd DSC i frånkopplade miljöer](## Use DSC in disconnected environments).</span><span class="sxs-lookup"><span data-stu-id="352b8-132">If you want toouse DSC for Windows machines that have WMF 4.0 installed, see hello section [Use DSC in disconnected environments](## Use DSC in disconnected environments).</span></span>
  

<span data-ttu-id="352b8-133">Hej mobilitetstjänsten kan installeras via hello kommandoraden och tar flera argument.</span><span class="sxs-lookup"><span data-stu-id="352b8-133">hello Mobility service can be installed through hello command line and accepts several arguments.</span></span> <span data-ttu-id="352b8-134">Det är därför du behöver toohave hello-binärfiler (när du extraherar dem från din konfiguration) och lagra dem på en plats där du kan hämta dem med hjälp av DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="352b8-134">That’s why you need toohave hello binaries (after extracting them from your setup) and store them in a place where you can retrieve them by using a DSC configuration.</span></span>

## <a name="step-1-extract-binaries"></a><span data-ttu-id="352b8-135">Steg 1: Extrahera binärfiler</span><span class="sxs-lookup"><span data-stu-id="352b8-135">Step 1: Extract binaries</span></span>
1. <span data-ttu-id="352b8-136">tooextract hello filer som behövs för den här installationen Bläddra toohello följande katalog på hanteringsservern:</span><span class="sxs-lookup"><span data-stu-id="352b8-136">tooextract hello files that you need for this setup, browse toohello following directory on your management server:</span></span>

    <span data-ttu-id="352b8-137">**\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span><span class="sxs-lookup"><span data-stu-id="352b8-137">**\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span></span>

    <span data-ttu-id="352b8-138">I den här mappen bör du se en MSI-fil med namnet:</span><span class="sxs-lookup"><span data-stu-id="352b8-138">In this folder, you should see an MSI file named:</span></span>

    <span data-ttu-id="352b8-139">**Microsoft ASR_UA_version_Windows_GA_date_Release.exe**</span><span class="sxs-lookup"><span data-stu-id="352b8-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span></span>

    <span data-ttu-id="352b8-140">Använd följande kommando tooextract hello installer hello:</span><span class="sxs-lookup"><span data-stu-id="352b8-140">Use hello following command tooextract hello installer:</span></span>

    <span data-ttu-id="352b8-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span><span class="sxs-lookup"><span data-stu-id="352b8-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span></span>
2. <span data-ttu-id="352b8-142">Markera alla filer och skicka dem tooa komprimerad mapp.</span><span class="sxs-lookup"><span data-stu-id="352b8-142">Select all files and send them tooa compressed (zipped) folder.</span></span>

<span data-ttu-id="352b8-143">Nu har du hello-binärfiler som du behöver tooautomate hello installationen av hello mobilitetstjänsten med hjälp av Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="352b8-143">You now have hello binaries that you need tooautomate hello setup of hello Mobility service by using Automation DSC.</span></span>

### <a name="passphrase"></a><span data-ttu-id="352b8-144">Lösenfrasen</span><span class="sxs-lookup"><span data-stu-id="352b8-144">Passphrase</span></span>
<span data-ttu-id="352b8-145">Sedan måste toodetermine där du vill att tooplace komprimerade mappen.</span><span class="sxs-lookup"><span data-stu-id="352b8-145">Next, you need toodetermine where you want tooplace this zipped folder.</span></span> <span data-ttu-id="352b8-146">Du kan använda ett Azure storage-konto som visas senare toostore hello lösenfras som du behöver för hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="352b8-146">You can use an Azure storage account, as shown later, toostore hello passphrase that you need for hello setup.</span></span> <span data-ttu-id="352b8-147">hello agenten registreras sedan med hello hanteringsservern som en del av hello-processen.</span><span class="sxs-lookup"><span data-stu-id="352b8-147">hello agent will then register with hello management server as part of hello process.</span></span>

<span data-ttu-id="352b8-148">hello lösenfras som du fick när du distribuerade hanteringsservern hello kan sparas tooa textfil som passphrase.txt.</span><span class="sxs-lookup"><span data-stu-id="352b8-148">hello passphrase that you got when you deployed hello management server can be saved tooa text file as passphrase.txt.</span></span>

<span data-ttu-id="352b8-149">Placera både hello komprimerade mappen och lösenfrasen hello i en dedikerad behållare i hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="352b8-149">Place both hello zipped folder and hello passphrase in a dedicated container in hello Azure storage account.</span></span>

![Mapp](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

<span data-ttu-id="352b8-151">Om du vill tookeep dessa filer på en resurs i nätverket kan göra du detta.</span><span class="sxs-lookup"><span data-stu-id="352b8-151">If you prefer tookeep these files on a share on your network, you can do so.</span></span> <span data-ttu-id="352b8-152">Du behöver bara tooensure att hello DSC-resurs som du kommer att använda senare har åtkomst och kan få hello installationen och lösenfrasen.</span><span class="sxs-lookup"><span data-stu-id="352b8-152">You just need tooensure that hello DSC resource that you will be using later has access and can get hello setup and passphrase.</span></span>

## <a name="step-2-create-hello-dsc-configuration"></a><span data-ttu-id="352b8-153">Steg 2: Skapa hello DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="352b8-153">Step 2: Create hello DSC configuration</span></span>
<span data-ttu-id="352b8-154">hello inställningen beror på WMF 5.0.</span><span class="sxs-lookup"><span data-stu-id="352b8-154">hello setup depends on WMF 5.0.</span></span> <span data-ttu-id="352b8-155">Hello datorn toosuccessfully tillämpas i hello konfigurationen med hjälp av Automation DSC, WMF 5.0 måste toobe finns.</span><span class="sxs-lookup"><span data-stu-id="352b8-155">For hello machine toosuccessfully apply hello configuration through Automation DSC, WMF 5.0 needs toobe present.</span></span>

<span data-ttu-id="352b8-156">hello miljö använder hello följande exempel DSC-konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="352b8-156">hello environment uses hello following example DSC configuration:</span></span>

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
<span data-ttu-id="352b8-157">hello konfiguration gör hello följande:</span><span class="sxs-lookup"><span data-stu-id="352b8-157">hello configuration will do hello following:</span></span>

* <span data-ttu-id="352b8-158">hello variabler talar hello konfiguration där tooget hello binärfiler för hello mobilitetstjänsten och hello Azure VM-agent, där tooget hello lösenfras och toostore hello utdata.</span><span class="sxs-lookup"><span data-stu-id="352b8-158">hello variables will tell hello configuration where tooget hello binaries for hello Mobility service and hello Azure VM agent, where tooget hello passphrase, and where toostore hello output.</span></span>
* <span data-ttu-id="352b8-159">hello konfigurationen importeras hello xPSDesiredStateConfiguration DSC-resurs, så att du kan använda `xRemoteFile` toodownload hello filer från hello-databas.</span><span class="sxs-lookup"><span data-stu-id="352b8-159">hello configuration will import hello xPSDesiredStateConfiguration DSC resource, so that you can use `xRemoteFile` toodownload hello files from hello repository.</span></span>
* <span data-ttu-id="352b8-160">hello konfigurationen skapar en katalog där du vill toostore hello binärfiler.</span><span class="sxs-lookup"><span data-stu-id="352b8-160">hello configuration will create a directory where you want toostore hello binaries.</span></span>
* <span data-ttu-id="352b8-161">hello Arkiv resursen kommer att extrahera hello filer från hello komprimerade mappen.</span><span class="sxs-lookup"><span data-stu-id="352b8-161">hello archive resource will extract hello files from hello zipped folder.</span></span>
* <span data-ttu-id="352b8-162">hello paketet installera resursen kommer att installera mobilitetstjänsten hello från hello UNIFIEDAGENT. EXE-installationsprogram med specifika hello-argument.</span><span class="sxs-lookup"><span data-stu-id="352b8-162">hello package Install resource will install hello Mobility service from hello UNIFIEDAGENT.EXE installer with hello specific arguments.</span></span> <span data-ttu-id="352b8-163">(hello variabler som skapar hello argument måste toobe ändras tooreflect din miljö.)</span><span class="sxs-lookup"><span data-stu-id="352b8-163">(hello variables that construct hello arguments need toobe changed tooreflect your environment.)</span></span>
* <span data-ttu-id="352b8-164">hello paketet AzureAgent resurs installeras hello Azure VM, vilket rekommenderas på varje virtuell dator som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="352b8-164">hello package AzureAgent resource will install hello Azure VM agent, which is recommended on every VM that runs in Azure.</span></span> <span data-ttu-id="352b8-165">hello Azure VM-agenten är det också möjligt tooadd tillägg toohello VM efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="352b8-165">hello Azure VM agent also makes it possible tooadd extensions toohello VM after failover.</span></span>
* <span data-ttu-id="352b8-166">hello säkerställer tjänsten eller de resurser som att hello relaterade mobilitetstjänsterna och hello Azure-tjänster alltid körs.</span><span class="sxs-lookup"><span data-stu-id="352b8-166">hello service resource or resources will ensure that hello related Mobility services and hello Azure services are always running.</span></span>

<span data-ttu-id="352b8-167">Spara hello konfiguration som **ASRMobilityService**.</span><span class="sxs-lookup"><span data-stu-id="352b8-167">Save hello configuration as **ASRMobilityService**.</span></span>

> [!NOTE]
> <span data-ttu-id="352b8-168">Kom ihåg tooreplace hello CSIP i din konfiguration tooreflect hello faktiska management-servern så att hello agent ansluts korrekt och använder hello rätt lösenfras.</span><span class="sxs-lookup"><span data-stu-id="352b8-168">Remember tooreplace hello CSIP in your configuration tooreflect hello actual management server, so that hello agent will be connected correctly and will use hello correct passphrase.</span></span>
>
>

## <a name="step-3-upload-tooautomation-dsc"></a><span data-ttu-id="352b8-169">Steg 3: Överför tooAutomation DSC</span><span class="sxs-lookup"><span data-stu-id="352b8-169">Step 3: Upload tooAutomation DSC</span></span>
<span data-ttu-id="352b8-170">Eftersom hello DSC-konfiguration som du gjort importerar en modul för nödvändiga DSC-resurs (xPSDesiredStateConfiguration), måste tooimport att modulen i Automation innan du laddar upp hello DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="352b8-170">Because hello DSC configuration that you made will import a required DSC resource module (xPSDesiredStateConfiguration), you need tooimport that module in Automation before you upload hello DSC configuration.</span></span>

<span data-ttu-id="352b8-171">Logga in tooyour Automation-konto för Bläddra**tillgångar** > **moduler**, och klicka på **Bläddra galleriet**.</span><span class="sxs-lookup"><span data-stu-id="352b8-171">Sign in tooyour Automation account, browse too**Assets** > **Modules**, and click **Browse Gallery**.</span></span>

<span data-ttu-id="352b8-172">Här kan du söka efter hello modulen och importera den tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="352b8-172">Here you can search for hello module and import it tooyour account.</span></span>

![Importera modulen](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

<span data-ttu-id="352b8-174">När du är klar detta går tooyour datorn där du har moduler som hello Azure Resource Manager har installerats och fortsätta tooimport hello nyskapad DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="352b8-174">When you finish this, go tooyour machine where you have hello Azure Resource Manager modules installed and proceed tooimport hello newly created DSC configuration.</span></span>

### <a name="import-cmdlets"></a><span data-ttu-id="352b8-175">Importera cmdlet: ar</span><span class="sxs-lookup"><span data-stu-id="352b8-175">Import cmdlets</span></span>
<span data-ttu-id="352b8-176">Logga in tooyour Azure-prenumeration i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="352b8-176">In PowerShell, sign in tooyour Azure subscription.</span></span> <span data-ttu-id="352b8-177">Ändra hello cmdlets tooreflect din miljö och avbilda kontoinformationen Automation i en variabel:</span><span class="sxs-lookup"><span data-stu-id="352b8-177">Modify hello cmdlets tooreflect your environment and capture your Automation account information in a variable:</span></span>

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

<span data-ttu-id="352b8-178">Överför hello configuration tooAutomation DSC med hjälp av hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="352b8-178">Upload hello configuration tooAutomation DSC by using hello following cmdlet:</span></span>

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-hello-configuration-in-automation-dsc"></a><span data-ttu-id="352b8-179">Kompilera hello konfigurationen i Automation DSC</span><span class="sxs-lookup"><span data-stu-id="352b8-179">Compile hello configuration in Automation DSC</span></span>
<span data-ttu-id="352b8-180">Sedan måste toocompile hello konfigurationen i Automation DSC, så att du kan starta tooregister noder tooit.</span><span class="sxs-lookup"><span data-stu-id="352b8-180">Next, you need toocompile hello configuration in Automation DSC, so that you can start tooregister nodes tooit.</span></span> <span data-ttu-id="352b8-181">Du kan uppnå som genom att köra följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="352b8-181">You achieve that by running hello following cmdlet:</span></span>

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

<span data-ttu-id="352b8-182">Detta kan ta några minuter, eftersom du i princip distribuerar hello toohello finns DSC pull-konfigurationstjänsten.</span><span class="sxs-lookup"><span data-stu-id="352b8-182">This can take a few minutes, because you're basically deploying hello configuration toohello hosted DSC pull service.</span></span>

<span data-ttu-id="352b8-183">När du kompilerar hello konfiguration du kan hämta hello jobbinformation med hjälp av PowerShell (Get-AzureRmAutomationDscCompilationJob) eller genom att använda hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="352b8-183">After you compile hello configuration, you can retrieve hello job information by using PowerShell (Get-AzureRmAutomationDscCompilationJob) or by using hello [Azure portal](https://portal.azure.com/).</span></span>

![Hämta jobb](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

<span data-ttu-id="352b8-185">Du har nu publicerade och överföra din DSC-konfiguration tooAutomation DSC.</span><span class="sxs-lookup"><span data-stu-id="352b8-185">You have now successfully published and uploaded your DSC configuration tooAutomation DSC.</span></span>

## <a name="step-4-onboard-machines-tooautomation-dsc"></a><span data-ttu-id="352b8-186">Steg 4: Publicera datorer tooAutomation DSC</span><span class="sxs-lookup"><span data-stu-id="352b8-186">Step 4: Onboard machines tooAutomation DSC</span></span>
> [!NOTE]
> <span data-ttu-id="352b8-187">En av hello förutsättningar för att slutföra det här scenariot är att din Windows-datorer uppdateras med hello senaste versionen av WMF.</span><span class="sxs-lookup"><span data-stu-id="352b8-187">One of hello prerequisites for completing this scenario is that your Windows machines are updated with hello latest version of WMF.</span></span> <span data-ttu-id="352b8-188">Du kan hämta och installera rätt version av hello för din plattform från hello [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="352b8-188">You can download and install hello correct version for your platform from hello [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>
>
>

<span data-ttu-id="352b8-189">Nu skapar du en metaconfig för DSC att du använder tooyour noder.</span><span class="sxs-lookup"><span data-stu-id="352b8-189">You will now create a metaconfig for DSC that you will apply tooyour nodes.</span></span> <span data-ttu-id="352b8-190">toosucceed med den här behöver du tooretrieve hello endpoint URL och hello primär nyckel för det valda Automation-kontot i Azure.</span><span class="sxs-lookup"><span data-stu-id="352b8-190">toosucceed with this, you need tooretrieve hello endpoint URL and hello primary key for your selected Automation account in Azure.</span></span> <span data-ttu-id="352b8-191">Du hittar dessa värden under **nycklar** på hello **alla inställningar** bladet för hello Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="352b8-191">You can find these values under **Keys** on hello **All settings** blade for hello Automation account.</span></span>

![Nyckelvärden](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

<span data-ttu-id="352b8-193">I det här exemplet har du en fysisk server för Windows Server 2012 R2 som du vill tooprotect genom att använda Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="352b8-193">In this example, you have a Windows Server 2012 R2 physical server that you want tooprotect by using Site Recovery.</span></span>

### <a name="check-for-any-pending-file-rename-operations-in-hello-registry"></a><span data-ttu-id="352b8-194">Kontrollera om det finns väntande filåtgärder rename i hello registret</span><span class="sxs-lookup"><span data-stu-id="352b8-194">Check for any pending file rename operations in hello registry</span></span>
<span data-ttu-id="352b8-195">Vi rekommenderar att du söker efter eventuella väntande filåtgärder rename i hello registret innan du börjar tooassociate hello server med hello Automation DSC-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="352b8-195">Before you start tooassociate hello server with hello Automation DSC endpoint, we recommend that you check for any pending file rename operations in hello registry.</span></span> <span data-ttu-id="352b8-196">De kan förhindra hello installationen att slutföra på grund av tooa väntande omstart.</span><span class="sxs-lookup"><span data-stu-id="352b8-196">They might prohibit hello setup from finishing due tooa pending reboot.</span></span>

<span data-ttu-id="352b8-197">Kör följande cmdlet tooverify att det finns ingen väntande omstart på hello server hello:</span><span class="sxs-lookup"><span data-stu-id="352b8-197">Run hello following cmdlet tooverify that there’s no pending reboot on hello server:</span></span>

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
<span data-ttu-id="352b8-198">Om det visar tomt är OK tooproceed.</span><span class="sxs-lookup"><span data-stu-id="352b8-198">If this shows empty, you are OK tooproceed.</span></span> <span data-ttu-id="352b8-199">Om inte, du kan åtgärda detta genom hello servern startas om under en underhållsperiod.</span><span class="sxs-lookup"><span data-stu-id="352b8-199">If not, you should address this by rebooting hello server during a maintenance window.</span></span>

<span data-ttu-id="352b8-200">tooapply hello konfigurationen på hello-servern, starta hello PowerShell Integrated Scripting Environment (ISE) och kör följande skript hello.</span><span class="sxs-lookup"><span data-stu-id="352b8-200">tooapply hello configuration on hello server, start hello PowerShell Integrated Scripting Environment (ISE) and run hello following script.</span></span> <span data-ttu-id="352b8-201">Detta är i grunden en DSC lokala konfiguration som instruerar hello Local Configuration Manager-motorn tooregister med hello Automation DSC-tjänsten och hämta hello specifik konfiguration (ASRMobilityService.localhost).</span><span class="sxs-lookup"><span data-stu-id="352b8-201">This is essentially a DSC local configuration that will instruct hello Local Configuration Manager engine tooregister with hello Automation DSC service and retrieve hello specific configuration (ASRMobilityService.localhost).</span></span>

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

<span data-ttu-id="352b8-202">Den här konfigurationen medför hello Local Configuration Manager motorn tooregister sig själv med Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="352b8-202">This configuration will cause hello Local Configuration Manager engine tooregister itself with Automation DSC.</span></span> <span data-ttu-id="352b8-203">Den avgör också hur hello motorn ska köras, vad den ska göra om det finns en konfigurationsavvikelser (ApplyAndAutoCorrect) och hur den ska fortsätta med hello konfiguration om en omstart krävs.</span><span class="sxs-lookup"><span data-stu-id="352b8-203">It will also determine how hello engine should operate, what it should do if there's a configuration drift (ApplyAndAutoCorrect), and how it should proceed with hello configuration if a reboot is required.</span></span>

<span data-ttu-id="352b8-204">När du har kört skriptet bör hello nod börja tooregister med Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="352b8-204">After you run this script, hello node should start tooregister with Automation DSC.</span></span>

![Noden registrering pågår](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

<span data-ttu-id="352b8-206">Om du går tillbaka toohello Azure-portalen kan se du hello nyregistrerade noden nu har visats i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="352b8-206">If you go back toohello Azure portal, you can see that hello newly registered node has now appeared in hello portal.</span></span>

![Registrerade nod i hello-portalen](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

<span data-ttu-id="352b8-208">Du kan köra hello följande PowerShell-cmdlet tooverify som hello nod har registrerats korrekt på hello-servern:</span><span class="sxs-lookup"><span data-stu-id="352b8-208">On hello server, you can run hello following PowerShell cmdlet tooverify that hello node has been registered correctly:</span></span>

```powershell
Get-DscLocalConfigurationManager
```

<span data-ttu-id="352b8-209">När hello konfiguration har ägardatabasen och tillämpade toohello server kan kan du kontrollera detta genom att köra följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="352b8-209">After hello configuration has been pulled and applied toohello server, you can verify this by running hello following cmdlet:</span></span>

```powershell
Get-DscConfigurationStatus
```

<span data-ttu-id="352b8-210">hello utdata visar hello servern har har hämtas dess konfiguration:</span><span class="sxs-lookup"><span data-stu-id="352b8-210">hello output shows that hello server has successfully pulled its configuration:</span></span>

![Resultat](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

<span data-ttu-id="352b8-212">Dessutom hello Mobility tjänstinställning har sin egen logg som finns på *SystemDrive*\ProgramData\ASRSetupLogs.</span><span class="sxs-lookup"><span data-stu-id="352b8-212">In addition, hello Mobility service setup has its own log that can be found at *SystemDrive*\ProgramData\ASRSetupLogs.</span></span>

<span data-ttu-id="352b8-213">Det stämmer.</span><span class="sxs-lookup"><span data-stu-id="352b8-213">That’s it.</span></span> <span data-ttu-id="352b8-214">Du har nu distribuerats och registrerad hello mobilitetstjänsten på hello datorn som du vill tooprotect genom att använda Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="352b8-214">You have now successfully deployed and registered hello Mobility service on hello machine that you want tooprotect by using Site Recovery.</span></span> <span data-ttu-id="352b8-215">DSC ska kontrollera att tjänsterna för hello krävs alltid är igång.</span><span class="sxs-lookup"><span data-stu-id="352b8-215">DSC will make sure that hello required services are always running.</span></span>

![Slutförd distribution](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

<span data-ttu-id="352b8-217">När hello hanteringsservern identifierar hello lyckad distribution, kan du konfigurera skyddet och aktivera replikering på hello dator genom att använda Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="352b8-217">After hello management server detects hello successful deployment, you can configure protection and enable replication on hello machine by using Site Recovery.</span></span>

## <a name="use-dsc-in-disconnected-environments"></a><span data-ttu-id="352b8-218">Använd DSC i frånkopplade miljöer</span><span class="sxs-lookup"><span data-stu-id="352b8-218">Use DSC in disconnected environments</span></span>
<span data-ttu-id="352b8-219">Om dina datorer inte är anslutna toohello Internet kan du fortfarande är beroende av DSC-toodeploy och konfigurerar hello mobilitetstjänsten på hello arbetsbelastningar som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="352b8-219">If your machines aren’t connected toohello Internet, you can still rely on DSC toodeploy and configure hello Mobility service on hello workloads that you want tooprotect.</span></span>

<span data-ttu-id="352b8-220">Du kan initiera en egen hämtningsservern för DSC i din miljö tooessentially innehåller hello samma funktioner som du får från Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="352b8-220">You can instantiate your own DSC pull server in your environment tooessentially provide hello same capabilities that you get from Automation DSC.</span></span> <span data-ttu-id="352b8-221">Det vill säga hello klienter hämtar hello-konfiguration (när den är registrerad) toohello DSC-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="352b8-221">That is, hello clients will pull hello configuration (after it's registered) toohello DSC endpoint.</span></span> <span data-ttu-id="352b8-222">Ett annat alternativ är dock toomanually push hello DSC-konfiguration tooyour datorer, lokalt eller fjärranslutet.</span><span class="sxs-lookup"><span data-stu-id="352b8-222">However, another option is toomanually push hello DSC configuration tooyour machines, either locally or remotely.</span></span>

<span data-ttu-id="352b8-223">Observera att i det här exemplet finns det en parameter som lagts till för hello datornamn.</span><span class="sxs-lookup"><span data-stu-id="352b8-223">Note that in this example, there's an added parameter for hello computer name.</span></span> <span data-ttu-id="352b8-224">hello fjärrfiler finns nu på en fjärresurs som ska vara tillgängliga av hello datorer som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="352b8-224">hello remote files are now located on a remote share that should be accessible by hello machines that you want tooprotect.</span></span> <span data-ttu-id="352b8-225">hello slutet av hello skript utfärdar hello-konfigurationen och startar sedan tooapply hello DSC-konfiguration toohello måldatorn.</span><span class="sxs-lookup"><span data-stu-id="352b8-225">hello end of hello script enacts hello configuration and then starts tooapply hello DSC configuration toohello target computer.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="352b8-226">Krav</span><span class="sxs-lookup"><span data-stu-id="352b8-226">Prerequisites</span></span>
<span data-ttu-id="352b8-227">Se till att hello xPSDesiredStateConfiguration PowerShell-modulen är installerad.</span><span class="sxs-lookup"><span data-stu-id="352b8-227">Make sure that hello xPSDesiredStateConfiguration PowerShell module is installed.</span></span> <span data-ttu-id="352b8-228">Du kan installera hello xPSDesiredStateConfiguration modul för Windows-datorer där WMF 5.0 är installerat, genom att köra följande cmdlet för hello måldatorerna hello:</span><span class="sxs-lookup"><span data-stu-id="352b8-228">For Windows machines where WMF 5.0 is installed, you can install hello xPSDesiredStateConfiguration module by running hello following cmdlet on hello target machines:</span></span>

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

<span data-ttu-id="352b8-229">Du kan också hämta och spara hello modulen om du behöver toodistribute den tooWindows datorer som har WMF 4.0.</span><span class="sxs-lookup"><span data-stu-id="352b8-229">You can also download and save hello module in case you need toodistribute it tooWindows machines that have WMF 4.0.</span></span> <span data-ttu-id="352b8-230">Kör denna cmdlet på en dator där PowerShellGet (WMF 5.0) förekommer:</span><span class="sxs-lookup"><span data-stu-id="352b8-230">Run this cmdlet on a machine where PowerShellGet (WMF 5.0) is present:</span></span>

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

<span data-ttu-id="352b8-231">Kontrollera också att hello för WMF 4.0, [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) är installerat på hello datorer.</span><span class="sxs-lookup"><span data-stu-id="352b8-231">Also for WMF 4.0, ensure that hello [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) is installed on hello machines.</span></span>

<span data-ttu-id="352b8-232">hello flyttas följande konfiguration tooWindows datorer som har WMF 5.0 och WMF 4.0:</span><span class="sxs-lookup"><span data-stu-id="352b8-232">hello following configuration can be pushed tooWindows machines that have WMF 5.0 and WMF 4.0:</span></span>

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

<span data-ttu-id="352b8-233">Om du vill tooinstantiate hämtningsservern egna DSC på ditt företagsnätverk toomimic hello-funktioner som du kan hämta från Automation DSC, se [ställer in en pull webbserver DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="352b8-233">If you want tooinstantiate your own DSC pull server on your corporate network toomimic hello capabilities that you can get from Automation DSC, see [Setting up a DSC web pull server](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span></span>

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a><span data-ttu-id="352b8-234">Valfritt: Distribuera en DSC-konfigurationen med hjälp av en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="352b8-234">Optional: Deploy a DSC configuration by using an Azure Resource Manager template</span></span>
<span data-ttu-id="352b8-235">Den här artikeln har fokuserar på hur du kan skapa egna DSC-konfigurationen tooautomatically distribuera hello mobilitetstjänsten och hello Azure VM-agenten och se till att de körs på hello datorer som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="352b8-235">This article has focused on how you can create your own DSC configuration tooautomatically deploy hello Mobility service and hello Azure VM Agent--and ensure that they are running on hello machines that you want tooprotect.</span></span> <span data-ttu-id="352b8-236">Vi har också en Azure Resource Manager-mall som ska distribuera den här DSC-konfiguration tooa ny eller befintlig Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="352b8-236">We also have an Azure Resource Manager template that will deploy this DSC configuration tooa new or existing Azure Automation account.</span></span> <span data-ttu-id="352b8-237">hello-mallen använder indataparametrar toocreate Automation tillgångar som innehåller hello variabler för din miljö.</span><span class="sxs-lookup"><span data-stu-id="352b8-237">hello template will use input parameters toocreate Automation assets that will contain hello variables for your environment.</span></span>

<span data-ttu-id="352b8-238">När du har distribuerat hello mallen kan du bara referera toostep 4 i den här guiden tooonboard dina datorer.</span><span class="sxs-lookup"><span data-stu-id="352b8-238">After you deploy hello template, you can simply refer toostep 4 in this guide tooonboard your machines.</span></span>

<span data-ttu-id="352b8-239">hello mallen gör hello följande:</span><span class="sxs-lookup"><span data-stu-id="352b8-239">hello template will do hello following:</span></span>

1. <span data-ttu-id="352b8-240">Använd ett befintligt Automation-konto eller skapa en ny</span><span class="sxs-lookup"><span data-stu-id="352b8-240">Use an existing Automation account or create a new one</span></span>
2. <span data-ttu-id="352b8-241">Ta indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="352b8-241">Take input parameters for:</span></span>
   * <span data-ttu-id="352b8-242">ASRRemoteFile--hello plats där du har sparat hello Mobility tjänstinställning</span><span class="sxs-lookup"><span data-stu-id="352b8-242">ASRRemoteFile--hello location where you have stored hello Mobility service setup</span></span>
   * <span data-ttu-id="352b8-243">ASRPassphrase--hello plats där du har sparat hello passphrase.txt fil</span><span class="sxs-lookup"><span data-stu-id="352b8-243">ASRPassphrase--hello location where you have stored hello passphrase.txt file</span></span>
   * <span data-ttu-id="352b8-244">ASRCSEndpoint--hello IP-adressen för hanteringsservern</span><span class="sxs-lookup"><span data-stu-id="352b8-244">ASRCSEndpoint--hello IP address of your management server</span></span>
3. <span data-ttu-id="352b8-245">Importera hello xPSDesiredStateConfiguration PowerShell-modulen</span><span class="sxs-lookup"><span data-stu-id="352b8-245">Import hello xPSDesiredStateConfiguration PowerShell module</span></span>
4. <span data-ttu-id="352b8-246">Skapa och kompilera hello DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="352b8-246">Create and compile hello DSC configuration</span></span>

<span data-ttu-id="352b8-247">Alla hello föregående steg sker i hello rätt ordning, så att du kan starta onboarding dina datorer för skydd.</span><span class="sxs-lookup"><span data-stu-id="352b8-247">All hello preceding steps will happen in hello right order, so that you can start onboarding your machines for protection.</span></span>

<span data-ttu-id="352b8-248">hello-mallen med instruktioner för distribution, finns på [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span><span class="sxs-lookup"><span data-stu-id="352b8-248">hello template, with instructions for deployment, is located on [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span></span>

<span data-ttu-id="352b8-249">Distribuera hello-mallen med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="352b8-249">Deploy hello template by using PowerShell:</span></span>

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a><span data-ttu-id="352b8-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="352b8-250">Next steps</span></span>
<span data-ttu-id="352b8-251">När du distribuerar hello Mobility service agenter, kan du [Aktivera replikering](site-recovery-vmware-to-azure.md) för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="352b8-251">After you deploy hello Mobility service agents, you can [enable replication](site-recovery-vmware-to-azure.md) for hello virtual machines.</span></span>
