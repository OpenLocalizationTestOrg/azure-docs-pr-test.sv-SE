---
title: aaaMove en Windows AWS VMs tooAzure | Microsoft Docs
description: "Flytta en Amazon Web Services (AWS) EC2 Windows instans tooAzure virtuella datorer med hjälp av Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: cynthn
ms.openlocfilehash: f912c28d3ffe585162c3add715a1318ac3cd4643
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-windows-vm-from-amazon-web-services-aws-tooazure-using-powershell"></a><span data-ttu-id="8aab6-103">Flytta en virtuell Windows-dator från Amazon Web Services (AWS) tooAzure med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8aab6-103">Move a Windows VM from Amazon Web Services (AWS) tooAzure using PowerShell</span></span>

<span data-ttu-id="8aab6-104">Om du utvärderar virtuella datorer i Azure som värd för din arbetsbelastning, kan du exportera en befintlig Amazon Web Services (AWS) EC2 Windows VM-instans sedan överför hello virtuell hårddisk (VHD) tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8aab6-104">If you are evaluating Azure virtual machines for hosting your workloads, you can export an existing Amazon Web Services (AWS) EC2 Windows VM instance then upload hello virtual hard disk (VHD) tooAzure.</span></span> <span data-ttu-id="8aab6-105">En gång hello VHD överförs, du kan skapa en ny virtuell dator i Azure från hello VHD.</span><span class="sxs-lookup"><span data-stu-id="8aab6-105">Once hello VHD is uploaded, you can create a new VM in Azure from hello VHD.</span></span> 

<span data-ttu-id="8aab6-106">Det här avsnittet beskriver flyttar en enda virtuell dator från AWS tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8aab6-106">This topic covers moving a single VM from AWS tooAzure.</span></span> <span data-ttu-id="8aab6-107">Om du vill toomove virtuella datorer från AWS tooAzure i skala, se [migrera virtuella datorer i Amazon Web Services (AWS) tooAzure med Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="8aab6-107">If you want toomove VMs from AWS tooAzure at scale, see [Migrate virtual machines in Amazon Web Services (AWS) tooAzure with Azure Site Recovery](../../site-recovery/site-recovery-migrate-aws-to-azure.md).</span></span>

## <a name="prepare-hello-vm"></a><span data-ttu-id="8aab6-108">Förbereda hello VM</span><span class="sxs-lookup"><span data-stu-id="8aab6-108">Prepare hello VM</span></span> 
 
<span data-ttu-id="8aab6-109">Du kan ladda upp tooAzure både generaliserad och specialiserade virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="8aab6-109">You can upload both generalized and specialized VHDs tooAzure.</span></span> <span data-ttu-id="8aab6-110">Varje typ måste du förbereda hello VM innan du exporterar från AWS.</span><span class="sxs-lookup"><span data-stu-id="8aab6-110">Each type requires that you prepare hello VM before exporting from AWS.</span></span> 

- <span data-ttu-id="8aab6-111">**Generaliserad virtuell Hårddisk** -en generaliserad virtuell Hårddisk har haft all personlig information bort med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="8aab6-111">**Generalized VHD** - a generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="8aab6-112">Om du avser toouse hello VHD som en bild toocreate nya virtuella datorer från bör du:</span><span class="sxs-lookup"><span data-stu-id="8aab6-112">If you intend toouse hello VHD as an image toocreate new VMs from, you should:</span></span> 
 
    * <span data-ttu-id="8aab6-113">[Förbereda Windows VM](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="8aab6-113">[Prepare a Windows VM](prepare-for-upload-vhd-image.md).</span></span>  
    * <span data-ttu-id="8aab6-114">Generalisera hello virtuell dator med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="8aab6-114">Generalize hello virtual machine using Sysprep.</span></span>  

 
- <span data-ttu-id="8aab6-115">**Specialiserad VHD** – en särskild virtuell Hårddisk underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8aab6-115">**Specialized VHD** - a specialized VHD maintains hello user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="8aab6-116">Om du avser toouse hello VHD som-är toocreate en ny virtuell dator, kontrollera hello följande steg har slutförts.</span><span class="sxs-lookup"><span data-stu-id="8aab6-116">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span>  
    * <span data-ttu-id="8aab6-117">[Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="8aab6-117">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span></span> <span data-ttu-id="8aab6-118">**Inte** generalisera hello VM med hjälp av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="8aab6-118">**Do not** generalize hello VM using Sysprep.</span></span> 
    * <span data-ttu-id="8aab6-119">Ta bort gäst virtualiseringsverktyg och agenter som installerats på hello VM (d.v.s. VMware-verktyg).</span><span class="sxs-lookup"><span data-stu-id="8aab6-119">Remove any guest virtualization tools and agents that are installed on hello VM (i.e. VMware tools).</span></span> 
    * <span data-ttu-id="8aab6-120">Se till att hello VM är konfigurerade toopull dess IP-adress och DNS-inställningarna via DHCP.</span><span class="sxs-lookup"><span data-stu-id="8aab6-120">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="8aab6-121">Detta säkerställer att hello-servern hämtar en IP-adress inom hello VNet när den startas.</span><span class="sxs-lookup"><span data-stu-id="8aab6-121">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span>  


## <a name="export-and-download-hello-vhd"></a><span data-ttu-id="8aab6-122">Exportera och hämta hello VHD</span><span class="sxs-lookup"><span data-stu-id="8aab6-122">Export and download hello VHD</span></span> 

<span data-ttu-id="8aab6-123">Exportera hello EC2 instans tooa VHD i en Amazon S3-bucket.</span><span class="sxs-lookup"><span data-stu-id="8aab6-123">Export hello EC2 instance tooa VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="8aab6-124">Följ hello stegen som beskrivs i avsnittet för hello Amazon dokumentationen [och exportera en instans som en virtuell dator med hjälp av VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) och kör hello [skapa-instans-export-aktivitet](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) kommandot tooexport hello EC2 instansen tooa VHD-filen.</span><span class="sxs-lookup"><span data-stu-id="8aab6-124">Follow hello steps described in hello Amazon documentation topic [Exporting an Instance as a VM Using VM Import/Export](http://docs.aws.amazon.com/vm-import/latest/userguide/vmexport.html) and run hello [create-instance-export-task](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-instance-export-task.html) command tooexport hello EC2 instance tooa VHD file.</span></span> 

<span data-ttu-id="8aab6-125">hello sparas exporterade VHD-filen i hello Amazon S3-bucket som du anger.</span><span class="sxs-lookup"><span data-stu-id="8aab6-125">hello exported VHD file is saved in hello Amazon S3 bucket you specify.</span></span> <span data-ttu-id="8aab6-126">hello grundläggande syntaxen för export av hello VHD visas nedan, bara ersätta hello platshållartexten i <brackets> med din information.</span><span class="sxs-lookup"><span data-stu-id="8aab6-126">hello basic syntax for exporting hello VHD is below, just replace hello placeholder text in <brackets> with your information.</span></span>

```
aws ec2 create-instance-export-task --instance-id <instanceID> --target-environment Microsoft \
  --export-to-s3-task DiskImageFormat=VHD,ContainerFormat=ova,S3Bucket=<bucket>,S3Prefix=<prefix>
```

<span data-ttu-id="8aab6-127">När hello VHD har exporterats, följer du anvisningarna för hello i [hur hämtar jag ett objekt från ett S3-Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) toodownload hello VHD-filen från hello S3-bucket.</span><span class="sxs-lookup"><span data-stu-id="8aab6-127">Once hello VHD has been exported, follow hello instructions in [How Do I Download an Object from an S3 Bucket?](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html) toodownload hello VHD file from hello S3 bucket.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="8aab6-128">AWS debiterar data transfer avgifter för att ladda ned hello VHD.</span><span class="sxs-lookup"><span data-stu-id="8aab6-128">AWS charges data transfer fees for downloading hello VHD.</span></span> <span data-ttu-id="8aab6-129">Se [Amazon S3 priser](https://aws.amazon.com/s3/pricing/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="8aab6-129">See [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/) for more information.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8aab6-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8aab6-130">Next steps</span></span>

<span data-ttu-id="8aab6-131">Nu kan du överföra hello VHD tooAzure och skapa en ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8aab6-131">Now you can upload hello VHD tooAzure and create a new VM.</span></span> 

- <span data-ttu-id="8aab6-132">Om du har kört Sysprep på källfilerna för**generalisera** den innan du exporterar, se [överför en generaliserad virtuell Hårddisk och använda toocreate en nya virtuella datorer i Azure](upload-generalized-managed.md)</span><span class="sxs-lookup"><span data-stu-id="8aab6-132">If you ran Sysprep on your source too**generalize** it before exporting, see [Upload a generalized VHD and use it toocreate a new VMs in Azure](upload-generalized-managed.md)</span></span>
- <span data-ttu-id="8aab6-133">Om du inte har kört Sysprep innan du exporterar hello VHD anses **specialiserade**, se [överför en särskild VHD-tooAzure och skapa en ny virtuell dator](create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="8aab6-133">If you did not run Sysprep before exporting, hello VHD is considered **specialized**, see [Upload a specialized VHD tooAzure and create a new VM](create-vm-specialized.md)</span></span>

 
