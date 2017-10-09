---
title: aaaAzure RemoteApp-avbildning krav | Microsoft Docs
description: "Lär dig mer om hello kraven för att skapa avbildningar toobe används med Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7cbb90f4-6dc9-462c-a429-088cdb57414e
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 4e35203eb93a866d4e0bd591d42b34746c7ffa4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a><span data-ttu-id="b381f-103">Krav för Azure RemoteApp-bilder</span><span class="sxs-lookup"><span data-stu-id="b381f-103">Requirements for Azure RemoteApp images</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b381f-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="b381f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="b381f-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="b381f-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="b381f-106">Azure RemoteApp använder en Windows Server 2012 R2-bilden toohost alla hello program som du vill tooshare med dina användare.</span><span class="sxs-lookup"><span data-stu-id="b381f-106">Azure RemoteApp uses a Windows Server 2012 R2 image toohost all hello programs that you want tooshare with your users.</span></span> <span data-ttu-id="b381f-107">toocreate en anpassad avbildning som du kan börja med en befintlig avbildning eller [skapa en ny](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="b381f-107">toocreate a custom image, you can start with an existing image or [create a new one](remoteapp-create-custom-image.md).</span></span>

> [!TIP]
> <span data-ttu-id="b381f-108">Visste du att din Azure RemoteApp-prenumeration du får åtkomst tooa Windows Server 2012 R2-avbildningen i hello Virtuella Azure-galleriet som du kan använda toocreate mallavbildningen?</span><span class="sxs-lookup"><span data-stu-id="b381f-108">Did you know that your Azure RemoteApp subscription gives you access tooa Windows Server 2012 R2 image in hello Azure VM gallery that you can use toocreate your own template image?</span></span> <span data-ttu-id="b381f-109">[Checka ut](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="b381f-109">[Check it out](remoteapp-image-on-azurevm.md).</span></span>  
> 
> 

<span data-ttu-id="b381f-110">hello kraven för hello-avbildning som kan överföras för användning med Azure RemoteApp är:</span><span class="sxs-lookup"><span data-stu-id="b381f-110">hello requirements for hello image that can be uploaded for use with Azure RemoteApp are:</span></span>

* <span data-ttu-id="b381f-111">Anpassade program lagra inte data lokalt på hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="b381f-111">Custom applications don’t store data locally on hello image.</span></span> <span data-ttu-id="b381f-112">Dessa avbildningar är tillståndslösa och får endast innehålla program.</span><span class="sxs-lookup"><span data-stu-id="b381f-112">These images are stateless and should only contain applications.</span></span>
* <span data-ttu-id="b381f-113">hello bilden innehåller inte data som kan gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="b381f-113">hello image does not contain data that can be lost.</span></span>
* <span data-ttu-id="b381f-114">hello avbildningens storlek bör vara en multipel av MB.</span><span class="sxs-lookup"><span data-stu-id="b381f-114">hello image size should be a multiple of MBs.</span></span> <span data-ttu-id="b381f-115">Om du försöker tooupload en avbildning som inte är en exakt multipel misslyckas hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="b381f-115">If you try tooupload an image that is not an exact multiple, hello upload will fail.</span></span>
* <span data-ttu-id="b381f-116">hello bildstorleken måste vara 127 GB eller mindre.</span><span class="sxs-lookup"><span data-stu-id="b381f-116">hello image size must be 127 GB or smaller.</span></span>
* <span data-ttu-id="b381f-117">Det måste finnas i en VHD-fil (VHDX-filer för närvarande stöds inte).</span><span class="sxs-lookup"><span data-stu-id="b381f-117">It must be on a VHD file (VHDX files are not currently supported).</span></span>
* <span data-ttu-id="b381f-118">hello VHD får inte vara en virtuell dator i generation 2.</span><span class="sxs-lookup"><span data-stu-id="b381f-118">hello VHD must not be a generation 2 virtual machine.</span></span>
* <span data-ttu-id="b381f-119">hello VHD kan vara fast storlek eller dynamiskt expanderande.</span><span class="sxs-lookup"><span data-stu-id="b381f-119">hello VHD can be either fixed-size or dynamically expanding.</span></span> <span data-ttu-id="b381f-120">En dynamiskt expanderande virtuell Hårddisk rekommenderas eftersom det tar mindre tid tooupload tooAzure än en VHD-fil med fast storlek.</span><span class="sxs-lookup"><span data-stu-id="b381f-120">A dynamically expanding VHD is recommended because it takes less time tooupload tooAzure than a fixed-size VHD file.</span></span>
* <span data-ttu-id="b381f-121">hello disken måste initieras med hello Master Boot Record (MBR) partitionering format.</span><span class="sxs-lookup"><span data-stu-id="b381f-121">hello disk must be initialized using hello Master Boot Record (MBR) partitioning style.</span></span> <span data-ttu-id="b381f-122">hello partitionstyp för GUID partition table (GPT) stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b381f-122">hello GUID partition table (GPT) partition style is not supported.</span></span>
* <span data-ttu-id="b381f-123">hello VHD måste innehålla en enkel installation av Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="b381f-123">hello VHD must contain a single installation of Windows Server 2012 R2.</span></span> <span data-ttu-id="b381f-124">Den kan innehålla flera volymer, men bara en som innehåller en installation av Windows.</span><span class="sxs-lookup"><span data-stu-id="b381f-124">It can contain multiple volumes, but only one that contains an installation of Windows.</span></span>
* <span data-ttu-id="b381f-125">hello Remote värd för fjärrskrivbordssession (RDSH)-rollen och hello Skrivbordsmiljö måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="b381f-125">hello Remote Desktop Session Host (RDSH) role and hello Desktop Experience feature must be installed.</span></span>
* <span data-ttu-id="b381f-126">hello Anslutningsutjämning för fjärrskrivbord roll måste *inte* installeras.</span><span class="sxs-lookup"><span data-stu-id="b381f-126">hello Remote Desktop Connection Broker role must *not* be installed.</span></span>
* <span data-ttu-id="b381f-127">hello Krypterande filsystem (EFS) måste vara inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="b381f-127">hello Encrypting File System (EFS) must be disabled.</span></span>
* <span data-ttu-id="b381f-128">hello bilden måste vara Sysprep med hello parametrar **/oobe / generalize/shutdown** (Använd inte hello **/mode:vm** parametern).</span><span class="sxs-lookup"><span data-stu-id="b381f-128">hello image must be SYSPREPed using hello parameters **/oobe /generalize /shutdown** (DO NOT use hello **/mode:vm** parameter).</span></span>
* <span data-ttu-id="b381f-129">Överför den virtuella Hårddisken från en ögonblicksbild kedja stöds inte.</span><span class="sxs-lookup"><span data-stu-id="b381f-129">Uploading your VHD from a snapshot chain is not supported.</span></span>

<span data-ttu-id="b381f-130">Se [skapa en Azure RemoteApp-avbildning](remoteapp-imageoptions.md) för mer information om hur du skapar bilder för Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b381f-130">See [Create an Azure RemoteApp image](remoteapp-imageoptions.md) for more information about creating images for Azure RemoteApp.</span></span>

