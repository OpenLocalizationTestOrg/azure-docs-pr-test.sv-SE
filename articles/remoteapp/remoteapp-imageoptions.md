---
title: Skapa en Azure RemoteApp-avbildning | Microsoft Docs
description: "Lär dig mer om alternativen för att skapa avbildningar för Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: cb0f9424-6185-45a1-abe9-c23f1edf34f2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 4b8ba6f264f982e03930c5ad4ccdb2d80f2c8665
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-remoteapp-image"></a><span data-ttu-id="9bb22-103">Skapa en Azure RemoteApp-avbildning</span><span class="sxs-lookup"><span data-stu-id="9bb22-103">Create an Azure RemoteApp image</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9bb22-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="9bb22-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="9bb22-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="9bb22-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="9bb22-106">Azure RemoteApp använder avbildningar för att lagra de appar som du delar med dina användare.</span><span class="sxs-lookup"><span data-stu-id="9bb22-106">Azure RemoteApp uses images to hold the apps that you share with your users.</span></span> <span data-ttu-id="9bb22-107">(Vi tar avbildningen och använda den för att skapa virtuella datorer – det är vad användarna åtkomst när de loggar in på Azure RemoteApp.) För att skapa en Azure RemoteApp-samling med ditt val av program, om det är molnet eller hybrid, börja med att skapa en bild med de program som är installerade.</span><span class="sxs-lookup"><span data-stu-id="9bb22-107">(We take your image and use it to create VMs - that's what the users access when they sign into Azure RemoteApp.) To create an Azure RemoteApp collection with your choice of applications, whether it is cloud or hybrid, you  start by creating an image with those applications installed.</span></span> <span data-ttu-id="9bb22-108">Skapa sedan en samling som använder avbildningen, tilldela användare till samlingen och publicera appar till dessa användare.</span><span class="sxs-lookup"><span data-stu-id="9bb22-108">Then, create a collection that uses that image, assign users to the collection, and publish apps to those users.</span></span>

<span data-ttu-id="9bb22-109">Du har flera alternativ för att skapa eller använda avbildningar.</span><span class="sxs-lookup"><span data-stu-id="9bb22-109">You have several options for creating or using images.</span></span> <span data-ttu-id="9bb22-110">Grundläggande [krav](remoteapp-imagereqs.md) för en bild är att den kör Windows Server 2012 R2 och har installerat fjärråtkomst värd för fjärrskrivbordssession (RDSH)-roll.</span><span class="sxs-lookup"><span data-stu-id="9bb22-110">The basic [requirement](remoteapp-imagereqs.md) for an image is that it run Windows Server 2012 R2 and have the Remote Desktop Session Host (RDSH) role installed.</span></span> <span data-ttu-id="9bb22-111">Hur du får som är där det blir intressant.</span><span class="sxs-lookup"><span data-stu-id="9bb22-111">How you get that is where things get interesting.</span></span>

<span data-ttu-id="9bb22-112">Du har följande alternativ när det gäller bilder:</span><span class="sxs-lookup"><span data-stu-id="9bb22-112">You have the following options when it comes to images:</span></span>

* <span data-ttu-id="9bb22-113">Du kan importera och använda en [bilden baserat på en virtuell dator i Azure](remoteapp-image-on-azurevm.md).</span><span class="sxs-lookup"><span data-stu-id="9bb22-113">You can import and use an [image based on an Azure virtual machine](remoteapp-image-on-azurevm.md).</span></span> <span data-ttu-id="9bb22-114">Det här är bra för av branschspecifika appar som kräver anpassade inställningar.</span><span class="sxs-lookup"><span data-stu-id="9bb22-114">This is good for line-of-business apps that require custom settings.</span></span> <span data-ttu-id="9bb22-115">Du kan anpassa avbildningen ska fungera för appen.</span><span class="sxs-lookup"><span data-stu-id="9bb22-115">You can customize the image to work for the app.</span></span>
* <span data-ttu-id="9bb22-116">Du kan [skapa och ladda upp en anpassad avbildning](remoteapp-create-custom-image.md).</span><span class="sxs-lookup"><span data-stu-id="9bb22-116">You can [create and upload a custom image](remoteapp-create-custom-image.md).</span></span> <span data-ttu-id="9bb22-117">Det här är bra om du redan har en avbildning som du använder för din lokala Remote Desktop Services-distribution.</span><span class="sxs-lookup"><span data-stu-id="9bb22-117">This is good if you already have an image that you use for your on-premises Remote Desktop Services deployment.</span></span>
* <span data-ttu-id="9bb22-118">Du kan använda en av de [mallavbildningarna](remoteapp-images.md) ingår i RemoteApp-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9bb22-118">You can use one of the [template images](remoteapp-images.md) included in your RemoteApp subscription.</span></span> <span data-ttu-id="9bb22-119">Dessa avbildningar skapas och underhålls av RemoteApp-teamet och innehåller vissa standard program (till exempel Office-paket) som du kan göra tillgängliga för användarna.</span><span class="sxs-lookup"><span data-stu-id="9bb22-119">These images are created and maintained by the RemoteApp team and contain some standard applications (like the Office suite) that you can make available to your users.</span></span> <span data-ttu-id="9bb22-120">Observera att endast Office 365 Pro Plus bilden kan användas i en Produktionsinställning.</span><span class="sxs-lookup"><span data-stu-id="9bb22-120">Note that only the Office 365 Pro Plus image can be used in a production setting.</span></span>

<span data-ttu-id="9bb22-121">Oavsett var du får en avbildning eller hur du skapar du vill kontrollera att du förstår de [appkrav](remoteapp-appreqs.md) så att din app fungerar bra i RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="9bb22-121">Regardless of where you get your image or how you create it, you'll want to make sure you understand the [app requirements](remoteapp-appreqs.md) to ensure that your app works well in RemoteApp.</span></span> <span data-ttu-id="9bb22-122">Nästa steg är att skapa en [moln](remoteapp-create-cloud-deployment.md) eller [hybrid](remoteapp-create-hybrid-deployment.md) samling.</span><span class="sxs-lookup"><span data-stu-id="9bb22-122">Then, the next step is to create a [cloud](remoteapp-create-cloud-deployment.md) or [hybrid](remoteapp-create-hybrid-deployment.md) collection.</span></span>

