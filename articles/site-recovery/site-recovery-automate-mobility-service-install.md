---
title: "Distribuera Site Recovery-mobilitetstjänsten med Azure Automation DSC | Microsoft Docs"
description: "Beskriver hur du använder Azure Automation DSC för att automatiskt distribuera Azure Site Recovery-mobilitetstjänsten och Azure-agenten för VMware VM och fysisk serverreplikering till Azure"
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
ms.openlocfilehash: bcc5f11afbecac8fe63935f3401dd3e2d767e8aa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a><span data-ttu-id="340e1-103">Distribuera mobilitetstjänsten med Azure Automation DSC för replikering av virtuell dator</span><span class="sxs-lookup"><span data-stu-id="340e1-103">Deploy the Mobility service with Azure Automation DSC for replication of VM</span></span>
<span data-ttu-id="340e1-104">I Operations Management Suite ger vi dig en omfattande säkerhetskopiering och haveriberedskapslösning som du kan använda som en del av en kontinuitetsplan.</span><span class="sxs-lookup"><span data-stu-id="340e1-104">In Operations Management Suite, we provide you with a comprehensive backup and disaster recovery solution that you can use as part of your business continuity plan.</span></span>

<span data-ttu-id="340e1-105">Vi har startats denna resa tillsammans med Hyper-V med hjälp av Hyper-V-replikering.</span><span class="sxs-lookup"><span data-stu-id="340e1-105">We started this journey together with Hyper-V by using Hyper-V Replica.</span></span> <span data-ttu-id="340e1-106">Men vi har utökat stöd för en inställning för heterogena eftersom kunder har flera hypervisorer och plattformar i sina moln.</span><span class="sxs-lookup"><span data-stu-id="340e1-106">But we have expanded to support a heterogeneous setup because customers have multiple hypervisors and platforms in their clouds.</span></span>

<span data-ttu-id="340e1-107">Om du kör VMware arbetsbelastningar och/eller fysiska servrar idag, körs alla Azure Site Recovery-komponenter i din miljö för att hantera replikering för kommunikation och data med Azure, när Azure är målet i en hanteringsserver.</span><span class="sxs-lookup"><span data-stu-id="340e1-107">If you are running VMware workloads and/or physical servers today, a management server runs all of the Azure Site Recovery components in your environment to handle the communication and data replication with Azure, when Azure is your destination.</span></span>

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a><span data-ttu-id="340e1-108">Distribuera Site Recovery-mobilitetstjänsten med hjälp av Automation DSC</span><span class="sxs-lookup"><span data-stu-id="340e1-108">Deploy the Site Recovery Mobility service by using Automation DSC</span></span>
<span data-ttu-id="340e1-109">Låt oss börja genom att göra en snabb uppdelning av den här hanteringsservern har.</span><span class="sxs-lookup"><span data-stu-id="340e1-109">Let's start by doing a quick breakdown of what this management server does.</span></span>

<span data-ttu-id="340e1-110">Management-servern kör flera serverroller.</span><span class="sxs-lookup"><span data-stu-id="340e1-110">The management server runs several server roles.</span></span> <span data-ttu-id="340e1-111">En av dessa roller är *configuration*, som samordnar kommunikationen och hanterar processer för replikering och återställning av data.</span><span class="sxs-lookup"><span data-stu-id="340e1-111">One of these roles is *configuration*, which coordinates communication and manages data replication and recovery processes.</span></span>

<span data-ttu-id="340e1-112">Dessutom kan den *processen* roll som fungerar som en replikerings-gateway.</span><span class="sxs-lookup"><span data-stu-id="340e1-112">In addition, the *process* role acts as a replication gateway.</span></span> <span data-ttu-id="340e1-113">Den här rollen tar emot replikeringsdata från skyddade källdatorer, optimerar dem med cachelagring, komprimering och kryptering och skickar det till ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="340e1-113">This role receives replication data from protected source machines, optimizes it with caching, compression, and encryption, and then sends it to an Azure storage account.</span></span> <span data-ttu-id="340e1-114">En av funktionerna för processen är också att push-installation av mobilitetstjänsten till skyddade datorer och utföra automatisk identifiering av virtuella VMware-datorer.</span><span class="sxs-lookup"><span data-stu-id="340e1-114">One of the functions for the process role is also to push installation of the Mobility service to protected machines and perform automatic discovery of VMware VMs.</span></span>

<span data-ttu-id="340e1-115">Om det finns en återställning från Azure, den *huvudmålservern* roll hanterar replikeringsdata som en del av den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="340e1-115">If there's a failback from Azure, the *master target* role will handle the replication data as part of this operation.</span></span>

<span data-ttu-id="340e1-116">För de skydda datorerna vi förlitar sig på den *mobilitetstjänsten*.</span><span class="sxs-lookup"><span data-stu-id="340e1-116">For the protected machines, we rely on the *Mobility service*.</span></span> <span data-ttu-id="340e1-117">Den här komponenten distribueras till varje dator (VMware VM eller fysiska server) som du vill replikera till Azure.</span><span class="sxs-lookup"><span data-stu-id="340e1-117">This component is deployed to every machine (VMware VM or physical server) that you want to replicate to Azure.</span></span> <span data-ttu-id="340e1-118">Den samlar in dataskrivningar på datorn och vidarebefordrar dem till hanteringsservern (Processroll).</span><span class="sxs-lookup"><span data-stu-id="340e1-118">It captures data writes on the machine and forwards them to the management server (process role).</span></span>

<span data-ttu-id="340e1-119">När du hantera affärskontinuitet, är det viktigt att förstå dina arbetsbelastningar och din infrastruktur och komponenterna som ingår.</span><span class="sxs-lookup"><span data-stu-id="340e1-119">When you're dealing with business continuity, it's important to understand your workloads, your infrastructure, and the components involved.</span></span> <span data-ttu-id="340e1-120">Du kan sedan uppfyller kraven för återställning tid mål för Återställningstid och mål för återställningspunkt (RPO).</span><span class="sxs-lookup"><span data-stu-id="340e1-120">You can then meet the requirements for your recovery time objective (RTO) and recovery point objective (RPO).</span></span> <span data-ttu-id="340e1-121">I den här kontexten är mobilitetstjänsten nyckeln till att säkerställa att dina arbetsbelastningar skyddas som förväntat.</span><span class="sxs-lookup"><span data-stu-id="340e1-121">In this context, the Mobility service is key to ensuring that your workloads are protected as you would expect.</span></span>

<span data-ttu-id="340e1-122">Hur kan du, på ett optimerat sätt se till att du har en tillförlitlig skyddade installationen med hjälp av vissa Operations Management Suite-komponenter?</span><span class="sxs-lookup"><span data-stu-id="340e1-122">So how can you, in an optimized way, ensure that you have a reliable protected setup with help from some Operations Management Suite components?</span></span>

<span data-ttu-id="340e1-123">Den här artikeln innehåller ett exempel på hur du kan använda Azure Automation önskad tillstånd Configuration (DSC), tillsammans med Site Recovery för att se till att:</span><span class="sxs-lookup"><span data-stu-id="340e1-123">This article provides an example of how you can use Azure Automation Desired State Configuration (DSC), together with Site Recovery, to ensure that:</span></span>

* <span data-ttu-id="340e1-124">Mobilitetstjänstversionen och Virtuella Azure-agenten distribueras till de Windows-datorer som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="340e1-124">The Mobility service and Azure VM agent are deployed to the Windows machines that you want to protect.</span></span>
* <span data-ttu-id="340e1-125">Mobilitetstjänstversionen och Virtuella Azure-agenten körs alltid när Azure är replikeringsmålet.</span><span class="sxs-lookup"><span data-stu-id="340e1-125">The Mobility service and Azure VM agent are always running when Azure is the replication target.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="340e1-126">Krav</span><span class="sxs-lookup"><span data-stu-id="340e1-126">Prerequisites</span></span>
* <span data-ttu-id="340e1-127">En databas för lagring av de nödvändiga inställningarna</span><span class="sxs-lookup"><span data-stu-id="340e1-127">A repository to store the required setup</span></span>
* <span data-ttu-id="340e1-128">En databas för att lagra nödvändiga lösenfrasen registreras hos management server</span><span class="sxs-lookup"><span data-stu-id="340e1-128">A repository to store the required passphrase to register with the management server</span></span>

  > [!NOTE]
  > <span data-ttu-id="340e1-129">En unik lösenfras genereras för varje hanteringsserver.</span><span class="sxs-lookup"><span data-stu-id="340e1-129">A unique passphrase is generated for each management server.</span></span> <span data-ttu-id="340e1-130">Om du ska distribuera flera hanteringsservrar, måste du se till att rätt lösenfrasen lagras i filen passphrase.txt.</span><span class="sxs-lookup"><span data-stu-id="340e1-130">If you are going to deploy multiple management servers, you have to ensure that the correct passphrase is stored in the passphrase.txt file.</span></span>
  >
  >
* <span data-ttu-id="340e1-131">Windows Management Framework (WMF) 5.0 installerat på de datorer som du vill aktivera skydd (ett krav för Automation DSC)</span><span class="sxs-lookup"><span data-stu-id="340e1-131">Windows Management Framework (WMF) 5.0 installed on the machines that you want to enable for protection (a requirement for Automation DSC)</span></span>

  > [!NOTE]
  > <span data-ttu-id="340e1-132">Om du vill använda DSC för Windows-datorer som har WMF 4.0 installerat finns i avsnittet [använder DSC i frånkopplade miljöer](## Use DSC in disconnected environments).</span><span class="sxs-lookup"><span data-stu-id="340e1-132">If you want to use DSC for Windows machines that have WMF 4.0 installed, see the section [Use DSC in disconnected environments](## Use DSC in disconnected environments).</span></span>
  

<span data-ttu-id="340e1-133">Mobilitetstjänsten kan installeras via kommandoraden och tar flera argument.</span><span class="sxs-lookup"><span data-stu-id="340e1-133">The Mobility service can be installed through the command line and accepts several arguments.</span></span> <span data-ttu-id="340e1-134">Därför måste du ha binärfilerna (när du extraherar dem från din konfiguration) och lagra dem på en plats där du kan hämta dem med hjälp av DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="340e1-134">That’s why you need to have the binaries (after extracting them from your setup) and store them in a place where you can retrieve them by using a DSC configuration.</span></span>

## <a name="step-1-extract-binaries"></a><span data-ttu-id="340e1-135">Steg 1: Extrahera binärfiler</span><span class="sxs-lookup"><span data-stu-id="340e1-135">Step 1: Extract binaries</span></span>
1. <span data-ttu-id="340e1-136">Bläddra till följande katalog på hanteringsservern för att extrahera filerna som du behöver för den här installationen:</span><span class="sxs-lookup"><span data-stu-id="340e1-136">To extract the files that you need for this setup, browse to the following directory on your management server:</span></span>

    <span data-ttu-id="340e1-137">**\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span><span class="sxs-lookup"><span data-stu-id="340e1-137">**\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**</span></span>

    <span data-ttu-id="340e1-138">I den här mappen bör du se en MSI-fil med namnet:</span><span class="sxs-lookup"><span data-stu-id="340e1-138">In this folder, you should see an MSI file named:</span></span>

    <span data-ttu-id="340e1-139">**Microsoft ASR_UA_version_Windows_GA_date_Release.exe**</span><span class="sxs-lookup"><span data-stu-id="340e1-139">**Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**</span></span>

    <span data-ttu-id="340e1-140">Använd följande kommando för att extrahera installationsprogrammet:</span><span class="sxs-lookup"><span data-stu-id="340e1-140">Use the following command to extract the installer:</span></span>

    <span data-ttu-id="340e1-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span><span class="sxs-lookup"><span data-stu-id="340e1-141">**.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**</span></span>
2. <span data-ttu-id="340e1-142">Markera alla filer och skicka dem till en komprimerad mapp.</span><span class="sxs-lookup"><span data-stu-id="340e1-142">Select all files and send them to a compressed (zipped) folder.</span></span>

<span data-ttu-id="340e1-143">Nu har du de binära filerna som behövs för att automatisera installationen av mobilitetstjänsten med hjälp av Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="340e1-143">You now have the binaries that you need to automate the setup of the Mobility service by using Automation DSC.</span></span>

### <a name="passphrase"></a><span data-ttu-id="340e1-144">Lösenfrasen</span><span class="sxs-lookup"><span data-stu-id="340e1-144">Passphrase</span></span>
<span data-ttu-id="340e1-145">Därefter måste du bestämma om du vill placera komprimerade mappen.</span><span class="sxs-lookup"><span data-stu-id="340e1-145">Next, you need to determine where you want to place this zipped folder.</span></span> <span data-ttu-id="340e1-146">Du kan använda ett Azure storage-konto som du ser senare för att lagra lösenfrasen som ska användas för installationen.</span><span class="sxs-lookup"><span data-stu-id="340e1-146">You can use an Azure storage account, as shown later, to store the passphrase that you need for the setup.</span></span> <span data-ttu-id="340e1-147">Sedan registrerar agenten med hanteringsservern som en del av processen.</span><span class="sxs-lookup"><span data-stu-id="340e1-147">The agent will then register with the management server as part of the process.</span></span>

<span data-ttu-id="340e1-148">Lösenordet som du fick när du distribuerade hanteringsservern kan sparas till en textfil som passphrase.txt.</span><span class="sxs-lookup"><span data-stu-id="340e1-148">The passphrase that you got when you deployed the management server can be saved to a text file as passphrase.txt.</span></span>

<span data-ttu-id="340e1-149">Placera både den komprimerade mappen och lösenfrasen i en dedikerad behållare i Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="340e1-149">Place both the zipped folder and the passphrase in a dedicated container in the Azure storage account.</span></span>

![Mapp](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

<span data-ttu-id="340e1-151">Om du vill behålla dessa filer på en resurs i nätverket, kan du göra detta.</span><span class="sxs-lookup"><span data-stu-id="340e1-151">If you prefer to keep these files on a share on your network, you can do so.</span></span> <span data-ttu-id="340e1-152">Du behöver bara se till att DSC-resurs som du kommer att använda senare har åtkomst och kan få installationen och lösenfrasen.</span><span class="sxs-lookup"><span data-stu-id="340e1-152">You just need to ensure that the DSC resource that you will be using later has access and can get the setup and passphrase.</span></span>

## <a name="step-2-create-the-dsc-configuration"></a><span data-ttu-id="340e1-153">Steg 2: Skapa DSC-konfigurationen</span><span class="sxs-lookup"><span data-stu-id="340e1-153">Step 2: Create the DSC configuration</span></span>
<span data-ttu-id="340e1-154">Inställningen beror på WMF 5.0.</span><span class="sxs-lookup"><span data-stu-id="340e1-154">The setup depends on WMF 5.0.</span></span> <span data-ttu-id="340e1-155">WMF 5.0 måste finnas för att kunna tillämpa konfigurationen med hjälp av Automation DSC datorn.</span><span class="sxs-lookup"><span data-stu-id="340e1-155">For the machine to successfully apply the configuration through Automation DSC, WMF 5.0 needs to be present.</span></span>

<span data-ttu-id="340e1-156">Miljön använder följande exempel DSC-konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="340e1-156">The environment uses the following example DSC configuration:</span></span>

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
<span data-ttu-id="340e1-157">Konfigurationen att göra följande:</span><span class="sxs-lookup"><span data-stu-id="340e1-157">The configuration will do the following:</span></span>

* <span data-ttu-id="340e1-158">Variablerna talar om hur var du kan hämta de binära filerna för mobilitetstjänsten och Virtuella Azure-agenten var du kan hämta lösenfrasen och var du lagrar utdata.</span><span class="sxs-lookup"><span data-stu-id="340e1-158">The variables will tell the configuration where to get the binaries for the Mobility service and the Azure VM agent, where to get the passphrase, and where to store the output.</span></span>
* <span data-ttu-id="340e1-159">Konfigurationen importeras xPSDesiredStateConfiguration DSC-resurs så att du kan använda `xRemoteFile` ska hämta filerna från databasen.</span><span class="sxs-lookup"><span data-stu-id="340e1-159">The configuration will import the xPSDesiredStateConfiguration DSC resource, so that you can use `xRemoteFile` to download the files from the repository.</span></span>
* <span data-ttu-id="340e1-160">Konfigurationen skapar en katalog där du vill lagra de binära filerna.</span><span class="sxs-lookup"><span data-stu-id="340e1-160">The configuration will create a directory where you want to store the binaries.</span></span>
* <span data-ttu-id="340e1-161">Arkivera resursen kommer att extrahera filerna från den komprimerade mappen.</span><span class="sxs-lookup"><span data-stu-id="340e1-161">The archive resource will extract the files from the zipped folder.</span></span>
* <span data-ttu-id="340e1-162">Paketet installera resursen kommer att installera mobilitetstjänsten från UNIFIEDAGENT. EXE-installationsprogram med specifika argument.</span><span class="sxs-lookup"><span data-stu-id="340e1-162">The package Install resource will install the Mobility service from the UNIFIEDAGENT.EXE installer with the specific arguments.</span></span> <span data-ttu-id="340e1-163">(De variabler som skapar argumenten måste ändras för att avspegla din miljö.)</span><span class="sxs-lookup"><span data-stu-id="340e1-163">(The variables that construct the arguments need to be changed to reflect your environment.)</span></span>
* <span data-ttu-id="340e1-164">Virtuella Azure-agenten, vilket rekommenderas på varje virtuell dator som körs i Azure installerar paketet AzureAgent resurs.</span><span class="sxs-lookup"><span data-stu-id="340e1-164">The package AzureAgent resource will install the Azure VM agent, which is recommended on every VM that runs in Azure.</span></span> <span data-ttu-id="340e1-165">Virtuella Azure-agenten gör det också möjligt att lägga till tillägg till den virtuella datorn efter redundans.</span><span class="sxs-lookup"><span data-stu-id="340e1-165">The Azure VM agent also makes it possible to add extensions to the VM after failover.</span></span>
* <span data-ttu-id="340e1-166">Den tjänsten eller de resurser som säkerställer att körs alltid relaterade mobilitetstjänsterna och Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="340e1-166">The service resource or resources will ensure that the related Mobility services and the Azure services are always running.</span></span>

<span data-ttu-id="340e1-167">Spara konfigurationen som **ASRMobilityService**.</span><span class="sxs-lookup"><span data-stu-id="340e1-167">Save the configuration as **ASRMobilityService**.</span></span>

> [!NOTE]
> <span data-ttu-id="340e1-168">Kom ihåg att ersätta CSIP i konfigurationen för att återspegla faktiska management-servern så att agenten ska anslutas på rätt sätt och använder rätt lösenfras.</span><span class="sxs-lookup"><span data-stu-id="340e1-168">Remember to replace the CSIP in your configuration to reflect the actual management server, so that the agent will be connected correctly and will use the correct passphrase.</span></span>
>
>

## <a name="step-3-upload-to-automation-dsc"></a><span data-ttu-id="340e1-169">Steg 3: Överför till Automation DSC</span><span class="sxs-lookup"><span data-stu-id="340e1-169">Step 3: Upload to Automation DSC</span></span>
<span data-ttu-id="340e1-170">Eftersom DSC-konfigurationen som du har gjort kommer att importera en modul för nödvändiga DSC-resurs (xPSDesiredStateConfiguration), måste du importera modulen i Automation innan du laddar upp DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="340e1-170">Because the DSC configuration that you made will import a required DSC resource module (xPSDesiredStateConfiguration), you need to import that module in Automation before you upload the DSC configuration.</span></span>

<span data-ttu-id="340e1-171">Logga in på ditt Automation-konto, bläddra till **tillgångar** > **moduler**, och klicka på **Bläddra galleriet**.</span><span class="sxs-lookup"><span data-stu-id="340e1-171">Sign in to your Automation account, browse to **Assets** > **Modules**, and click **Browse Gallery**.</span></span>

<span data-ttu-id="340e1-172">Här kan du söka efter modulen och importera dem till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="340e1-172">Here you can search for the module and import it to your account.</span></span>

![Importera modulen](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

<span data-ttu-id="340e1-174">När du är klar detta går du till datorn där du har Azure Resource Manager-moduler som har installerats och fortsätta importera nyskapade DSC-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="340e1-174">When you finish this, go to your machine where you have the Azure Resource Manager modules installed and proceed to import the newly created DSC configuration.</span></span>

### <a name="import-cmdlets"></a><span data-ttu-id="340e1-175">Importera cmdlet: ar</span><span class="sxs-lookup"><span data-stu-id="340e1-175">Import cmdlets</span></span>
<span data-ttu-id="340e1-176">Logga in på Azure-prenumerationen i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="340e1-176">In PowerShell, sign in to your Azure subscription.</span></span> <span data-ttu-id="340e1-177">Ändra cmdlet: ar för att avspegla din miljö och avbilda kontoinformationen Automation i en variabel:</span><span class="sxs-lookup"><span data-stu-id="340e1-177">Modify the cmdlets to reflect your environment and capture your Automation account information in a variable:</span></span>

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

<span data-ttu-id="340e1-178">Överför konfigurationen till Automation DSC genom att använda följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="340e1-178">Upload the configuration to Automation DSC by using the following cmdlet:</span></span>

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a><span data-ttu-id="340e1-179">Kompilera konfigurationen i Automation DSC</span><span class="sxs-lookup"><span data-stu-id="340e1-179">Compile the configuration in Automation DSC</span></span>
<span data-ttu-id="340e1-180">Därefter måste kompilera konfigurationen i Automation DSC, så att du kan börja registrera noderna till den.</span><span class="sxs-lookup"><span data-stu-id="340e1-180">Next, you need to compile the configuration in Automation DSC, so that you can start to register nodes to it.</span></span> <span data-ttu-id="340e1-181">Du kan åstadkomma som genom att köra följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="340e1-181">You achieve that by running the following cmdlet:</span></span>

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

<span data-ttu-id="340e1-182">Detta kan ta några minuter, eftersom du i princip distribuerar konfigurationen till den värdbaserade DSC pull-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="340e1-182">This can take a few minutes, because you're basically deploying the configuration to the hosted DSC pull service.</span></span>

<span data-ttu-id="340e1-183">När du sammanställer konfigurationen du kan hämta jobbinformation med hjälp av PowerShell (Get-AzureRmAutomationDscCompilationJob) eller genom att använda den [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="340e1-183">After you compile the configuration, you can retrieve the job information by using PowerShell (Get-AzureRmAutomationDscCompilationJob) or by using the [Azure portal](https://portal.azure.com/).</span></span>

![Hämta jobb](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

<span data-ttu-id="340e1-185">Du har nu har publicerats och överföra DSC-konfigurationen till Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="340e1-185">You have now successfully published and uploaded your DSC configuration to Automation DSC.</span></span>

## <a name="step-4-onboard-machines-to-automation-dsc"></a><span data-ttu-id="340e1-186">Steg 4: Publicera datorer till Automation DSC</span><span class="sxs-lookup"><span data-stu-id="340e1-186">Step 4: Onboard machines to Automation DSC</span></span>
> [!NOTE]
> <span data-ttu-id="340e1-187">En av förutsättningarna för att slutföra det här scenariot är att din Windows-datorer uppdateras med den senaste versionen av WMF.</span><span class="sxs-lookup"><span data-stu-id="340e1-187">One of the prerequisites for completing this scenario is that your Windows machines are updated with the latest version of WMF.</span></span> <span data-ttu-id="340e1-188">Du kan hämta och installera rätt version för din plattform från den [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span><span class="sxs-lookup"><span data-stu-id="340e1-188">You can download and install the correct version for your platform from the [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).</span></span>
>
>

<span data-ttu-id="340e1-189">Nu skapar du en metaconfig för DSC som du ska gälla för noderna.</span><span class="sxs-lookup"><span data-stu-id="340e1-189">You will now create a metaconfig for DSC that you will apply to your nodes.</span></span> <span data-ttu-id="340e1-190">Du måste hämta slutpunkts-URL och den primära nyckeln för det valda Automation-kontot i Azure för att lyckas med den här.</span><span class="sxs-lookup"><span data-stu-id="340e1-190">To succeed with this, you need to retrieve the endpoint URL and the primary key for your selected Automation account in Azure.</span></span> <span data-ttu-id="340e1-191">Du hittar dessa värden under **nycklar** på den **alla inställningar** bladet för Automation-kontot.</span><span class="sxs-lookup"><span data-stu-id="340e1-191">You can find these values under **Keys** on the **All settings** blade for the Automation account.</span></span>

![Nyckelvärden](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

<span data-ttu-id="340e1-193">I det här exemplet har du en Windows Server 2012 R2 fysisk server som du vill skydda med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="340e1-193">In this example, you have a Windows Server 2012 R2 physical server that you want to protect by using Site Recovery.</span></span>

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a><span data-ttu-id="340e1-194">Kontrollera om det finns väntande filåtgärder Byt namn i registret</span><span class="sxs-lookup"><span data-stu-id="340e1-194">Check for any pending file rename operations in the registry</span></span>
<span data-ttu-id="340e1-195">Innan du börjar koppla servern till slutpunkten för Automation DSC, rekommenderar vi att du söker efter eventuella väntande filåtgärder Byt namn i registret.</span><span class="sxs-lookup"><span data-stu-id="340e1-195">Before you start to associate the server with the Automation DSC endpoint, we recommend that you check for any pending file rename operations in the registry.</span></span> <span data-ttu-id="340e1-196">De kan förhindra installationen slutförs på grund av en väntande omstart.</span><span class="sxs-lookup"><span data-stu-id="340e1-196">They might prohibit the setup from finishing due to a pending reboot.</span></span>

<span data-ttu-id="340e1-197">Kör följande cmdlet för att kontrollera att det finns ingen väntande omstart på servern:</span><span class="sxs-lookup"><span data-stu-id="340e1-197">Run the following cmdlet to verify that there’s no pending reboot on the server:</span></span>

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
<span data-ttu-id="340e1-198">Om det visar tomt är OK för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="340e1-198">If this shows empty, you are OK to proceed.</span></span> <span data-ttu-id="340e1-199">Om inte, du kan åtgärda detta genom att servern startas om under en underhållsperiod.</span><span class="sxs-lookup"><span data-stu-id="340e1-199">If not, you should address this by rebooting the server during a maintenance window.</span></span>

<span data-ttu-id="340e1-200">Starta PowerShell Integrated Scripting Environment (ISE) och kör följande skript för att tillämpa konfigurationen på servern.</span><span class="sxs-lookup"><span data-stu-id="340e1-200">To apply the configuration on the server, start the PowerShell Integrated Scripting Environment (ISE) and run the following script.</span></span> <span data-ttu-id="340e1-201">Detta är i grunden en DSC lokala konfiguration som instruerar Local Configuration Manager-motorn att registrera med Automation DSC-tjänsten och hämta viss konfiguration (ASRMobilityService.localhost).</span><span class="sxs-lookup"><span data-stu-id="340e1-201">This is essentially a DSC local configuration that will instruct the Local Configuration Manager engine to register with the Automation DSC service and retrieve the specific configuration (ASRMobilityService.localhost).</span></span>

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

<span data-ttu-id="340e1-202">Den här konfigurationen gör att den lokala Configuration Manager-motorn att registrera sig med Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="340e1-202">This configuration will cause the Local Configuration Manager engine to register itself with Automation DSC.</span></span> <span data-ttu-id="340e1-203">Den avgör också hur motorn ska köras, vad den ska göra om det finns en konfigurationsavvikelser (ApplyAndAutoCorrect) och hur den ska fortsätta med konfigurationen av en omstart krävs.</span><span class="sxs-lookup"><span data-stu-id="340e1-203">It will also determine how the engine should operate, what it should do if there's a configuration drift (ApplyAndAutoCorrect), and how it should proceed with the configuration if a reboot is required.</span></span>

<span data-ttu-id="340e1-204">När du har kört skriptet bör noden börja registrera med Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="340e1-204">After you run this script, the node should start to register with Automation DSC.</span></span>

![Noden registrering pågår](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

<span data-ttu-id="340e1-206">Om du går tillbaka till Azure-portalen kan se du att noden nyregistrerade nu har visats i portalen.</span><span class="sxs-lookup"><span data-stu-id="340e1-206">If you go back to the Azure portal, you can see that the newly registered node has now appeared in the portal.</span></span>

![Registrerade nod i portalen](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

<span data-ttu-id="340e1-208">Du kan köra följande PowerShell-cmdlet för att kontrollera att noden har registrerats korrekt på servern:</span><span class="sxs-lookup"><span data-stu-id="340e1-208">On the server, you can run the following PowerShell cmdlet to verify that the node has been registered correctly:</span></span>

```powershell
Get-DscLocalConfigurationManager
```

<span data-ttu-id="340e1-209">Efter konfigurationen har hämtas och tillämpas på servern, kan du kontrollera detta genom att köra följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="340e1-209">After the configuration has been pulled and applied to the server, you can verify this by running the following cmdlet:</span></span>

```powershell
Get-DscConfigurationStatus
```

<span data-ttu-id="340e1-210">Utdata visar att servern har mottagit dess konfiguration:</span><span class="sxs-lookup"><span data-stu-id="340e1-210">The output shows that the server has successfully pulled its configuration:</span></span>

![Resultat](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

<span data-ttu-id="340e1-212">Dessutom tjänstinställning Mobility har sin egen logg som finns på *SystemDrive*\ProgramData\ASRSetupLogs.</span><span class="sxs-lookup"><span data-stu-id="340e1-212">In addition, the Mobility service setup has its own log that can be found at *SystemDrive*\ProgramData\ASRSetupLogs.</span></span>

<span data-ttu-id="340e1-213">Det stämmer.</span><span class="sxs-lookup"><span data-stu-id="340e1-213">That’s it.</span></span> <span data-ttu-id="340e1-214">Du har nu distribuerats och registrerad mobilitetstjänsten på den dator som du vill skydda med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="340e1-214">You have now successfully deployed and registered the Mobility service on the machine that you want to protect by using Site Recovery.</span></span> <span data-ttu-id="340e1-215">DSC ska kontrollera att de nödvändiga tjänsterna körs alltid.</span><span class="sxs-lookup"><span data-stu-id="340e1-215">DSC will make sure that the required services are always running.</span></span>

![Slutförd distribution](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

<span data-ttu-id="340e1-217">När hanteringsservern identifierar lyckad distribution, kan du konfigurera skyddet och aktivera replikering på datorn med hjälp av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="340e1-217">After the management server detects the successful deployment, you can configure protection and enable replication on the machine by using Site Recovery.</span></span>

## <a name="use-dsc-in-disconnected-environments"></a><span data-ttu-id="340e1-218">Använd DSC i frånkopplade miljöer</span><span class="sxs-lookup"><span data-stu-id="340e1-218">Use DSC in disconnected environments</span></span>
<span data-ttu-id="340e1-219">Om dina datorer inte är ansluten till Internet, kan du fortfarande beroende DSC för att distribuera och konfigurera mobilitetstjänsten på arbetsbelastningar som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="340e1-219">If your machines aren’t connected to the Internet, you can still rely on DSC to deploy and configure the Mobility service on the workloads that you want to protect.</span></span>

<span data-ttu-id="340e1-220">Du kan initiera en egen hämtningsservern för DSC i din miljö för att ge i stort sett samma funktioner som du får från Automation DSC.</span><span class="sxs-lookup"><span data-stu-id="340e1-220">You can instantiate your own DSC pull server in your environment to essentially provide the same capabilities that you get from Automation DSC.</span></span> <span data-ttu-id="340e1-221">Det vill säga hämtar klienter konfigurationen (när den är registrerad) till DSC-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="340e1-221">That is, the clients will pull the configuration (after it's registered) to the DSC endpoint.</span></span> <span data-ttu-id="340e1-222">Ett annat alternativ är dock manuellt push DSC-konfigurationen till dina datorer, lokalt eller fjärranslutet.</span><span class="sxs-lookup"><span data-stu-id="340e1-222">However, another option is to manually push the DSC configuration to your machines, either locally or remotely.</span></span>

<span data-ttu-id="340e1-223">Observera att i det här exemplet finns det en extra parameter för namnet på datorn.</span><span class="sxs-lookup"><span data-stu-id="340e1-223">Note that in this example, there's an added parameter for the computer name.</span></span> <span data-ttu-id="340e1-224">Fjärråtkomst filerna finns nu på en fjärresurs som ska kunna nås av datorer som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="340e1-224">The remote files are now located on a remote share that should be accessible by the machines that you want to protect.</span></span> <span data-ttu-id="340e1-225">I slutet av skriptet utfärdar konfigurationen och startar sedan tillämpa DSC-konfigurationen på måldatorn.</span><span class="sxs-lookup"><span data-stu-id="340e1-225">The end of the script enacts the configuration and then starts to apply the DSC configuration to the target computer.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="340e1-226">Krav</span><span class="sxs-lookup"><span data-stu-id="340e1-226">Prerequisites</span></span>
<span data-ttu-id="340e1-227">Se till att xPSDesiredStateConfiguration PowerShell-modulen är installerad.</span><span class="sxs-lookup"><span data-stu-id="340e1-227">Make sure that the xPSDesiredStateConfiguration PowerShell module is installed.</span></span> <span data-ttu-id="340e1-228">För Windows-datorer där WMF 5.0 är installerat, kan du installera modulen xPSDesiredStateConfiguration genom att köra följande cmdlet på måldatorer:</span><span class="sxs-lookup"><span data-stu-id="340e1-228">For Windows machines where WMF 5.0 is installed, you can install the xPSDesiredStateConfiguration module by running the following cmdlet on the target machines:</span></span>

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

<span data-ttu-id="340e1-229">Du kan också hämta och spara modulen om du behöver distribuera den till Windows-datorer som har WMF 4.0.</span><span class="sxs-lookup"><span data-stu-id="340e1-229">You can also download and save the module in case you need to distribute it to Windows machines that have WMF 4.0.</span></span> <span data-ttu-id="340e1-230">Kör denna cmdlet på en dator där PowerShellGet (WMF 5.0) förekommer:</span><span class="sxs-lookup"><span data-stu-id="340e1-230">Run this cmdlet on a machine where PowerShellGet (WMF 5.0) is present:</span></span>

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

<span data-ttu-id="340e1-231">Även WMF 4.0, se till att den [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) installeras på datorer.</span><span class="sxs-lookup"><span data-stu-id="340e1-231">Also for WMF 4.0, ensure that the [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) is installed on the machines.</span></span>

<span data-ttu-id="340e1-232">Följande konfiguration kan skickas till Windows-datorer som har WMF 5.0 och WMF 4.0:</span><span class="sxs-lookup"><span data-stu-id="340e1-232">The following configuration can be pushed to Windows machines that have WMF 5.0 and WMF 4.0:</span></span>

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

<span data-ttu-id="340e1-233">Om du vill skapa en instans av en egen DSC pull-server i företagsnätverket för att efterlikna de funktioner som du kan hämta från Automation DSC, se [ställer in en pull webbserver DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="340e1-233">If you want to instantiate your own DSC pull server on your corporate network to mimic the capabilities that you can get from Automation DSC, see [Setting up a DSC web pull server](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).</span></span>

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a><span data-ttu-id="340e1-234">Valfritt: Distribuera en DSC-konfigurationen med hjälp av en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="340e1-234">Optional: Deploy a DSC configuration by using an Azure Resource Manager template</span></span>
<span data-ttu-id="340e1-235">Den här artikeln har fokuserar på hur du kan skapa egna DSC-konfiguration för att automatiskt distribuera mobilitetstjänsten och Azure VM-agenten-- och kontrollera som de körs på de datorer som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="340e1-235">This article has focused on how you can create your own DSC configuration to automatically deploy the Mobility service and the Azure VM Agent--and ensure that they are running on the machines that you want to protect.</span></span> <span data-ttu-id="340e1-236">Vi har också en Azure Resource Manager-mall som ska distribuera den här DSC-konfigurationen till en ny eller befintlig Azure Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="340e1-236">We also have an Azure Resource Manager template that will deploy this DSC configuration to a new or existing Azure Automation account.</span></span> <span data-ttu-id="340e1-237">Mallen använder indataparametrar för att skapa Automation tillgångar som innehåller variabler för din miljö.</span><span class="sxs-lookup"><span data-stu-id="340e1-237">The template will use input parameters to create Automation assets that will contain the variables for your environment.</span></span>

<span data-ttu-id="340e1-238">När du distribuerar mallen kan du bara referera till steg 4 i den här guiden för att publicera dina datorer.</span><span class="sxs-lookup"><span data-stu-id="340e1-238">After you deploy the template, you can simply refer to step 4 in this guide to onboard your machines.</span></span>

<span data-ttu-id="340e1-239">Mallen kommer att göra följande:</span><span class="sxs-lookup"><span data-stu-id="340e1-239">The template will do the following:</span></span>

1. <span data-ttu-id="340e1-240">Använd ett befintligt Automation-konto eller skapa en ny</span><span class="sxs-lookup"><span data-stu-id="340e1-240">Use an existing Automation account or create a new one</span></span>
2. <span data-ttu-id="340e1-241">Ta indataparametrar:</span><span class="sxs-lookup"><span data-stu-id="340e1-241">Take input parameters for:</span></span>
   * <span data-ttu-id="340e1-242">ASRRemoteFile--den plats där du har sparat tjänstinställning Mobility</span><span class="sxs-lookup"><span data-stu-id="340e1-242">ASRRemoteFile--the location where you have stored the Mobility service setup</span></span>
   * <span data-ttu-id="340e1-243">ASRPassphrase--den plats där du har sparat filen passphrase.txt</span><span class="sxs-lookup"><span data-stu-id="340e1-243">ASRPassphrase--the location where you have stored the passphrase.txt file</span></span>
   * <span data-ttu-id="340e1-244">ASRCSEndpoint--IP-adressen för hanteringsservern</span><span class="sxs-lookup"><span data-stu-id="340e1-244">ASRCSEndpoint--the IP address of your management server</span></span>
3. <span data-ttu-id="340e1-245">Importera xPSDesiredStateConfiguration PowerShell-modulen</span><span class="sxs-lookup"><span data-stu-id="340e1-245">Import the xPSDesiredStateConfiguration PowerShell module</span></span>
4. <span data-ttu-id="340e1-246">Skapa och kompilera DSC-konfigurationen</span><span class="sxs-lookup"><span data-stu-id="340e1-246">Create and compile the DSC configuration</span></span>

<span data-ttu-id="340e1-247">De föregående stegen sker i rätt ordning, så att du kan starta onboarding dina datorer för skydd.</span><span class="sxs-lookup"><span data-stu-id="340e1-247">All the preceding steps will happen in the right order, so that you can start onboarding your machines for protection.</span></span>

<span data-ttu-id="340e1-248">Mallen med instruktioner för distribution, finns på [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span><span class="sxs-lookup"><span data-stu-id="340e1-248">The template, with instructions for deployment, is located on [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).</span></span>

<span data-ttu-id="340e1-249">Distribuera mallen med hjälp av PowerShell:</span><span class="sxs-lookup"><span data-stu-id="340e1-249">Deploy the template by using PowerShell:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="340e1-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="340e1-250">Next steps</span></span>
<span data-ttu-id="340e1-251">När du har distribuerat agenterna Mobility service kan du [Aktivera replikering](site-recovery-vmware-to-azure.md) för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="340e1-251">After you deploy the Mobility service agents, you can [enable replication](site-recovery-vmware-to-azure.md) for the virtual machines.</span></span>
