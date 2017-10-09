---
title: "aaaSet in Apache Tomcat på en Linux-dator | Microsoft Docs"
description: "Lär dig hur tooset in Apache Tomcat7 med hjälp av Azure virtuella datorer som kör Linux."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: b837a73e91fcb25d5459d993a0e93ceef1a1fc8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a><span data-ttu-id="1de8d-103">Ställa in Tomcat7 på en Linux-dator med Azure</span><span class="sxs-lookup"><span data-stu-id="1de8d-103">Set up Tomcat7 on a Linux virtual machine with Azure</span></span>
<span data-ttu-id="1de8d-104">Apache Tomcat (eller bara Tomcat kan också kallades Djakarta Tomcat) är en öppen källkod webbservern och servlet-behållare som utvecklats av hello Apache Software Foundation (ASF).</span><span class="sxs-lookup"><span data-stu-id="1de8d-104">Apache Tomcat (or simply Tomcat, also formerly called Jakarta Tomcat) is an open source web server and servlet container developed by hello Apache Software Foundation (ASF).</span></span> <span data-ttu-id="1de8d-105">Tomcat implementerar hello Java Servlet och hello JavaServer sidor (JSP) specifikationer från Sun Microsystems.</span><span class="sxs-lookup"><span data-stu-id="1de8d-105">Tomcat implements hello Java Servlet and hello JavaServer Pages (JSP) specifications from Sun Microsystems.</span></span> <span data-ttu-id="1de8d-106">Tomcat ger en ren Java HTTP web server-miljö i vilken toorun Java-kod.</span><span class="sxs-lookup"><span data-stu-id="1de8d-106">Tomcat provides a pure Java HTTP web server environment in which toorun Java code.</span></span> <span data-ttu-id="1de8d-107">I hello enklaste konfigurationen körs Tomcat i en enda process.</span><span class="sxs-lookup"><span data-stu-id="1de8d-107">In hello simplest configuration, Tomcat runs in a single operating system process.</span></span> <span data-ttu-id="1de8d-108">Den här processen körs en Java virtual machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="1de8d-108">This process runs a Java virtual machine (JVM).</span></span> <span data-ttu-id="1de8d-109">Varje HTTP-begäran från en webbläsare tooTomcat behandlas som en separat tråd hello Tomcat pågår.</span><span class="sxs-lookup"><span data-stu-id="1de8d-109">Every HTTP request from a browser tooTomcat is processed as a separate thread in hello Tomcat process.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="1de8d-110">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="1de8d-110">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="1de8d-111">Den här artikeln beskriver hur toouse hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="1de8d-111">This article covers how toouse hello classic deployment model.</span></span> <span data-ttu-id="1de8d-112">Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="1de8d-112">We recommend that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="1de8d-113">toouse toodeploy en Resource Manager-mallen en Ubuntu VM med öppna JDK och Tomcat, se [i den här artikeln](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span><span class="sxs-lookup"><span data-stu-id="1de8d-113">toouse a Resource Manager template toodeploy an Ubuntu VM with Open JDK and Tomcat, see [this article](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span></span>

<span data-ttu-id="1de8d-114">I den här artikeln får du installerar Tomcat7 på en Linux-avbildning och distribuera den i Azure.</span><span class="sxs-lookup"><span data-stu-id="1de8d-114">In this article, you will install Tomcat7 on a Linux image and deploy it in Azure.</span></span>  

<span data-ttu-id="1de8d-115">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="1de8d-115">You will learn:</span></span>  

* <span data-ttu-id="1de8d-116">Hur toocreate en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="1de8d-116">How toocreate a virtual machine in Azure.</span></span>
* <span data-ttu-id="1de8d-117">Hur hello tooprepare virtuell dator för Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="1de8d-117">How tooprepare hello virtual machine for Tomcat7.</span></span>
* <span data-ttu-id="1de8d-118">Hur tooinstall Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="1de8d-118">How tooinstall Tomcat7.</span></span>

<span data-ttu-id="1de8d-119">Det förutsätts att du redan har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1de8d-119">It is assumed that you already have an Azure subscription.</span></span>  <span data-ttu-id="1de8d-120">Om inte, du kan registrera dig för en kostnadsfri utvärderingsversion på [hello Azure-webbplatsen](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="1de8d-120">If not, you can sign up for a free trial at [hello Azure website](https://azure.microsoft.com/).</span></span> <span data-ttu-id="1de8d-121">Om du har en MSDN-prenumeration, se [Microsoft särskilda priser för Azure: MSDN, MPN och BizSpark fördelar](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span><span class="sxs-lookup"><span data-stu-id="1de8d-121">If you have an MSDN subscription, see [Microsoft Azure Special Pricing: MSDN, MPN, and BizSpark Benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span></span> <span data-ttu-id="1de8d-122">toolearn mer om Azure, se [vad är Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span><span class="sxs-lookup"><span data-stu-id="1de8d-122">toolearn more about Azure, see [What is Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span></span>

<span data-ttu-id="1de8d-123">Den här artikeln förutsätter att du har grundläggande kunskaper om Tomcat- och Linux.</span><span class="sxs-lookup"><span data-stu-id="1de8d-123">This article assumes that you have a basic working knowledge of Tomcat and Linux.</span></span>  

## <a name="phase-1-create-an-image"></a><span data-ttu-id="1de8d-124">Fas 1: Skapa en avbildning</span><span class="sxs-lookup"><span data-stu-id="1de8d-124">Phase 1: Create an image</span></span>
<span data-ttu-id="1de8d-125">I det här steget ska du skapa en virtuell dator med hjälp av en Linux-avbildning i Azure.</span><span class="sxs-lookup"><span data-stu-id="1de8d-125">In this phase, you will create a virtual machine by using a Linux image in Azure.</span></span>  

### <a name="step-1-generate-an-ssh-authentication-key"></a><span data-ttu-id="1de8d-126">Steg 1: Skapa en SSH-nyckel för autentisering</span><span class="sxs-lookup"><span data-stu-id="1de8d-126">Step 1: Generate an SSH authentication key</span></span>
<span data-ttu-id="1de8d-127">SSH är ett viktigt verktyg för administratörer.</span><span class="sxs-lookup"><span data-stu-id="1de8d-127">SSH is an important tool for system administrators.</span></span> <span data-ttu-id="1de8d-128">Dock rekommenderas konfigurera åtkomstsäkerhet baserat på ett fastställt mänskliga lösenord inte.</span><span class="sxs-lookup"><span data-stu-id="1de8d-128">However, configuring access security based on a human-determined password is not recommended.</span></span> <span data-ttu-id="1de8d-129">Angripare kan dela upp i ditt system baserat på ett användarnamn och ett svagt lösenord.</span><span class="sxs-lookup"><span data-stu-id="1de8d-129">Malicious users can break into your system based on a username and a weak password.</span></span>

<span data-ttu-id="1de8d-130">hello bra är att det finns ett sätt tooleave fjärråtkomst öppna och oroa dig inte om lösenord.</span><span class="sxs-lookup"><span data-stu-id="1de8d-130">hello good news is that there is a way tooleave remote access open and not worry about passwords.</span></span> <span data-ttu-id="1de8d-131">Den här metoden består av autentisering med asymmetrisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="1de8d-131">This method consists of authentication with asymmetric cryptography.</span></span> <span data-ttu-id="1de8d-132">hello är användares privata nyckel hello som ger hello-autentisering.</span><span class="sxs-lookup"><span data-stu-id="1de8d-132">hello user’s private key is hello one that grants hello authentication.</span></span> <span data-ttu-id="1de8d-133">Du kan även låsa hello användarkonto toonot Tillåt lösenordsautentisering.</span><span class="sxs-lookup"><span data-stu-id="1de8d-133">You can even lock hello user’s account toonot allow password authentication.</span></span>

<span data-ttu-id="1de8d-134">En annan fördel med den här metoden är att du inte behöver olika lösenord toosign i toodifferent servrar.</span><span class="sxs-lookup"><span data-stu-id="1de8d-134">Another advantage of this method is that you do not need different passwords toosign in toodifferent servers.</span></span> <span data-ttu-id="1de8d-135">Du kan autentisera med hjälp av hello personliga privata nyckel på alla servrar som förhindrar att tooremember flera lösenord.</span><span class="sxs-lookup"><span data-stu-id="1de8d-135">You can authenticate by using hello personal private key on all servers, which prevents you from having tooremember several passwords.</span></span>



<span data-ttu-id="1de8d-136">Följ dessa steg toogenerate hello SSH-autentisering-nyckel.</span><span class="sxs-lookup"><span data-stu-id="1de8d-136">Follow these steps toogenerate hello SSH authentication key.</span></span>

1. <span data-ttu-id="1de8d-137">Hämta och installera PuTTYgen från hello följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="1de8d-137">Download and install PuTTYgen from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="1de8d-138">Kör Puttygen.exe.</span><span class="sxs-lookup"><span data-stu-id="1de8d-138">Run Puttygen.exe.</span></span>
3. <span data-ttu-id="1de8d-139">Klicka på **generera** toogenerate hello nycklar.</span><span class="sxs-lookup"><span data-stu-id="1de8d-139">Click **Generate** toogenerate hello keys.</span></span> <span data-ttu-id="1de8d-140">Hello processen att du kan öka slumpmässighet genom glidande hello muspekaren över hello tomt område i hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="1de8d-140">In hello process, you can increase randomness by moving hello mouse over hello blank area in hello window.</span></span>  
   <span data-ttu-id="1de8d-141">![PuTTY nyckel Generator skärmbild som visar hello genererar nya nycklar knappen][1]</span><span class="sxs-lookup"><span data-stu-id="1de8d-141">![PuTTY Key Generator screenshot that shows hello generate new key button][1]</span></span>
4. <span data-ttu-id="1de8d-142">När hello generera process, visas Puttygen.exe den offentliga nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1de8d-142">After hello generate process, Puttygen.exe will show your new public key.</span></span>  
   ![PuTTY nyckel Generator skärmbild som visar hello ny offentlig nyckel och hello Spara privat nyckel knapp][2]
5. <span data-ttu-id="1de8d-144">Markera och kopiera hello offentliga nyckeln och spara det i en fil med namnet publicKey.pem.</span><span class="sxs-lookup"><span data-stu-id="1de8d-144">Select and copy hello public key, and save it in a file named publicKey.pem.</span></span> <span data-ttu-id="1de8d-145">Klicka inte på **spara offentlig nyckel**eftersom hello spara offentlig nyckel filformatet skiljer sig från hello offentlig nyckel som vi vill.</span><span class="sxs-lookup"><span data-stu-id="1de8d-145">Don’t click **Save public key**, because hello saved public key’s file format is different from hello public key we want.</span></span>
6. <span data-ttu-id="1de8d-146">Klicka på **Spara privat nyckel**, och spara den i en fil med namnet privateKey.ppk.</span><span class="sxs-lookup"><span data-stu-id="1de8d-146">Click **Save private key**, and save it in a file named privateKey.ppk.</span></span>

### <a name="step-2-create-hello-image-in-hello-azure-portal"></a><span data-ttu-id="1de8d-147">Steg 2: Skapa hello bild i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1de8d-147">Step 2: Create hello image in hello Azure portal</span></span>
1. <span data-ttu-id="1de8d-148">I hello [portal](https://portal.azure.com/), klickar du på **ny** i hello uppgift liggande toocreate en bild.</span><span class="sxs-lookup"><span data-stu-id="1de8d-148">In hello [portal](https://portal.azure.com/), click **New** in hello task bar toocreate an image.</span></span> <span data-ttu-id="1de8d-149">Välj hello Linux bild är baserad på dina behov.</span><span class="sxs-lookup"><span data-stu-id="1de8d-149">Then choose hello Linux image that is based on your needs.</span></span> <span data-ttu-id="1de8d-150">hello används följande exempel hello Ubuntu 14.04 bild.</span><span class="sxs-lookup"><span data-stu-id="1de8d-150">hello following example uses hello Ubuntu 14.04 image.</span></span>
<span data-ttu-id="1de8d-151">![Skärmbild av hello portal som visar hello ny knapp][3]</span><span class="sxs-lookup"><span data-stu-id="1de8d-151">![Screenshot of hello portal that shows hello New button][3]</span></span>

2. <span data-ttu-id="1de8d-152">För **värdnamn**, ange hello namn för hello-URL att du och Internet-klienter kommer att använda tooaccess den här virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-152">For **Host Name**, specify hello name for hello URL that you and Internet clients will use tooaccess this virtual machine.</span></span> <span data-ttu-id="1de8d-153">Definiera hello sista delen av hello DNS-namn, till exempel tomcatdemo.</span><span class="sxs-lookup"><span data-stu-id="1de8d-153">Define hello last part of hello DNS name, for example, tomcatdemo.</span></span> <span data-ttu-id="1de8d-154">Azure skapar sedan hello URL som tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="1de8d-154">Azure will then generate hello URL as tomcatdemo.cloudapp.net.</span></span>  

3. <span data-ttu-id="1de8d-155">För **SSH autentiseringsnyckel**, kopiera hello nyckelvärde från hello publicKey.pem fil som innehåller hello offentliga nyckel som genererats av PuTTYgen.</span><span class="sxs-lookup"><span data-stu-id="1de8d-155">For **SSH Authentication Key**, copy hello key value from hello publicKey.pem file, which contains hello public key generated by PuTTYgen.</span></span>  
<span data-ttu-id="1de8d-156">![Autentiseringsnyckeln för SSH rutan i hello-portalen][4]</span><span class="sxs-lookup"><span data-stu-id="1de8d-156">![SSH Authentication Key box in hello portal][4]</span></span>

4. <span data-ttu-id="1de8d-157">Konfigurera övriga inställningar efter behov och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-157">Configure other settings as needed, and then click **Create**.</span></span>  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a><span data-ttu-id="1de8d-158">Fas 2: Förbereda den virtuella datorn för Tomcat7</span><span class="sxs-lookup"><span data-stu-id="1de8d-158">Phase 2: Prepare your virtual machine for Tomcat7</span></span>
<span data-ttu-id="1de8d-159">I det här steget kommer du konfigurerar en slutpunkt för Tomcat trafik och ansluter sedan tooyour ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1de8d-159">In this phase, you will configure an endpoint for Tomcat traffic, and then connect tooyour new virtual machine.</span></span>

### <a name="step-1-open-hello-http-port-tooallow-web-access"></a><span data-ttu-id="1de8d-160">Steg 1: Öppna hello HTTP port tooallow webbåtkomst</span><span class="sxs-lookup"><span data-stu-id="1de8d-160">Step 1: Open hello HTTP port tooallow web access</span></span>
<span data-ttu-id="1de8d-161">Slutpunkter i Azure består av ett TCP- eller UDP-protokoll, tillsammans med en offentlig och privat port.</span><span class="sxs-lookup"><span data-stu-id="1de8d-161">Endpoints in Azure consist of a TCP or UDP protocol, along with a public and private port.</span></span> <span data-ttu-id="1de8d-162">hello privata porten är hello hello tjänsten lyssnar tooon hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1de8d-162">hello private port is hello port that hello service is listening tooon hello virtual machine.</span></span> <span data-ttu-id="1de8d-163">hello offentlig port är hello-port som hello Azure-molntjänst lyssnar tooexternally för inkommande, Internet-baserade trafik.</span><span class="sxs-lookup"><span data-stu-id="1de8d-163">hello public port is hello port that hello Azure cloud service listens tooexternally for incoming, Internet-based traffic.</span></span>  

<span data-ttu-id="1de8d-164">TCP-port 8080 är hello standardportnumret att Tomcat använder toolisten.</span><span class="sxs-lookup"><span data-stu-id="1de8d-164">TCP port 8080 is hello default port number that Tomcat uses toolisten.</span></span> <span data-ttu-id="1de8d-165">Om den här porten är öppen med en Azure slutpunkt du och andra Internet-klienter kan komma åt Tomcat sidor.</span><span class="sxs-lookup"><span data-stu-id="1de8d-165">If this port is opened with an Azure endpoint, you and other Internet clients can access Tomcat pages.</span></span>  

1. <span data-ttu-id="1de8d-166">I hello-portalen klickar du på **Bläddra** > **virtuella datorer**, och klicka sedan på hello virtuell dator som du skapade.</span><span class="sxs-lookup"><span data-stu-id="1de8d-166">In hello portal, click **Browse** > **Virtual machines**, and then click hello virtual machine that you created.</span></span>  
   <span data-ttu-id="1de8d-167">![Skärmbild av hello virtuella datorer directory][5]</span><span class="sxs-lookup"><span data-stu-id="1de8d-167">![Screenshot of hello Virtual machines directory][5]</span></span>
2. <span data-ttu-id="1de8d-168">tooadd en slutpunkt tooyour virtuell dator, klicka på hello **slutpunkter** rutan.</span><span class="sxs-lookup"><span data-stu-id="1de8d-168">tooadd an endpoint tooyour virtual machine, click hello **Endpoints** box.</span></span>
   <span data-ttu-id="1de8d-169">![Skärmbild som visar hello slutpunkter rutan][6]</span><span class="sxs-lookup"><span data-stu-id="1de8d-169">![Screenshot that shows hello Endpoints box][6]</span></span>
3. <span data-ttu-id="1de8d-170">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-170">Click **Add**.</span></span>  

   1. <span data-ttu-id="1de8d-171">För hello slutpunkt, ange ett namn för hello slutpunkt i **Endpoint**, och ange sedan 80 i **offentlig Port**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-171">For hello endpoint, enter a name for hello endpoint in **Endpoint**, and then enter 80 in **Public Port**.</span></span>  

      <span data-ttu-id="1de8d-172">Om du anger det too80, behöver du inte tooinclude hello portnumret i hello URL-adress används tooaccess Tomcat.</span><span class="sxs-lookup"><span data-stu-id="1de8d-172">If you set it too80, you don’t need tooinclude hello port number in hello URL that is used tooaccess Tomcat.</span></span> <span data-ttu-id="1de8d-173">Till exempel http://tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="1de8d-173">For example, http://tomcatdemo.cloudapp.net.</span></span>    

      <span data-ttu-id="1de8d-174">Om du anger det tooanother värde, exempelvis 81, måste tooadd hello port number toohello URL tooaccess Tomcat.</span><span class="sxs-lookup"><span data-stu-id="1de8d-174">If you set it tooanother value, such as 81, you need tooadd hello port number toohello URL tooaccess Tomcat.</span></span> <span data-ttu-id="1de8d-175">Till exempel http://tomcatdemo.cloudapp.net:81 /.</span><span class="sxs-lookup"><span data-stu-id="1de8d-175">For example,  http://tomcatdemo.cloudapp.net:81/.</span></span>
   2. <span data-ttu-id="1de8d-176">Ange 8080 i **privat Port**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-176">Enter 8080 in **Private Port**.</span></span> <span data-ttu-id="1de8d-177">Som standard lyssnar Tomcat på TCP-port 8080.</span><span class="sxs-lookup"><span data-stu-id="1de8d-177">By default, Tomcat listens on TCP port 8080.</span></span> <span data-ttu-id="1de8d-178">Om du har ändrat hello standard lyssna port Tomcat bör du uppdatera **privat Port** toobe hello samma som hello lyssningsporten för Tomcat.</span><span class="sxs-lookup"><span data-stu-id="1de8d-178">If you changed hello default listen port of Tomcat, you should update **Private Port** toobe hello same as hello Tomcat listen port.</span></span>  
      <span data-ttu-id="1de8d-179">![Skärmbild av Användargränssnittet som visar Lägg till kommandot offentlig Port och privat Port][7]</span><span class="sxs-lookup"><span data-stu-id="1de8d-179">![Screenshot of UI that shows Add command, Public Port, and Private Port][7]</span></span>
4. <span data-ttu-id="1de8d-180">Klicka på **OK** tooadd hello endpoint tooyour virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-180">Click **OK** tooadd hello endpoint tooyour virtual machine.</span></span>

### <a name="step-2-connect-toohello-image-you-created"></a><span data-ttu-id="1de8d-181">Steg 2: Anslut toohello bilden som du skapat</span><span class="sxs-lookup"><span data-stu-id="1de8d-181">Step 2: Connect toohello image you created</span></span>
<span data-ttu-id="1de8d-182">Du kan välja en SSH-verktyget tooconnect tooyour virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1de8d-182">You can choose any SSH tool tooconnect tooyour virtual machine.</span></span> <span data-ttu-id="1de8d-183">I det här exemplet använder vi PuTTY.</span><span class="sxs-lookup"><span data-stu-id="1de8d-183">In this example, we use PuTTY.</span></span>  

1. <span data-ttu-id="1de8d-184">Hämta hello DNS-namnet på den virtuella datorn från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="1de8d-184">Get hello DNS name of your virtual machine from hello portal.</span></span>
    1. <span data-ttu-id="1de8d-185">Klicka på **Bläddra** > **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-185">Click **Browse** > **Virtual machines**.</span></span>
    2. <span data-ttu-id="1de8d-186">Välj hello namnet på den virtuella datorn och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-186">Select hello name of your virtual machine, and then click **Properties**.</span></span>
    3. <span data-ttu-id="1de8d-187">I hello **egenskaper** panelen, titta i hello **domännamn** rutan tooget hello DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-187">In hello **Properties** tile, look in hello **Domain Name** box tooget hello DNS name.</span></span>  

2. <span data-ttu-id="1de8d-188">Hämta hello port för SSH-anslutningar från hello **SSH** rutan.</span><span class="sxs-lookup"><span data-stu-id="1de8d-188">Get hello port number for SSH connections from hello **SSH** box.</span></span>  
<span data-ttu-id="1de8d-189">![Skärmbild som visar hello SSH anslutningsnummer port][8]</span><span class="sxs-lookup"><span data-stu-id="1de8d-189">![Screenshot that shows hello SSH connection port number][8]</span></span>

3. <span data-ttu-id="1de8d-190">Hämta [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="1de8d-190">Download [PuTTY](http://www.putty.org/).</span></span>  

4. <span data-ttu-id="1de8d-191">Klicka på hello körbar fil Putty.exe efter hämtningen.</span><span class="sxs-lookup"><span data-stu-id="1de8d-191">After downloading, click hello executable file Putty.exe.</span></span> <span data-ttu-id="1de8d-192">PuTTY-konfiguration, konfigurera hello grundalternativ med hello värdnamn och portnummer som erhålls från hello egenskaperna för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-192">In PuTTY configuration, configure hello basic options with hello host name and port number that is obtained from hello properties of your virtual machine.</span></span>   
![Skärmbild som visar hello PuTTY configuration värden namn på och portnummer alternativ][9]

5. <span data-ttu-id="1de8d-194">Hello vänster klickar du på **anslutning** > **SSH** > **Auth**, och klicka sedan på **Bläddra** toospecify hello sökväg till hello privateKey.ppk.</span><span class="sxs-lookup"><span data-stu-id="1de8d-194">In hello left pane, click **Connection** > **SSH** > **Auth**, and then click **Browse** toospecify hello location of hello privateKey.ppk file.</span></span> <span data-ttu-id="1de8d-195">Hej privateKey.ppk filen innehåller hello privata nyckeln som genereras av PuTTYgen tidigare i hello ”fas 1: skapa en avbildning” i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="1de8d-195">hello privateKey.ppk file contains hello private key that is generated by PuTTYgen earlier in hello "Phase 1: Create an image" section of this article.</span></span>  
<span data-ttu-id="1de8d-196">![Skärmbild som visar hello anslutning directory-hierarkin och knappen Bläddra.][10]</span><span class="sxs-lookup"><span data-stu-id="1de8d-196">![Screenshot that shows hello Connection directory hierarchy and Browse button][10]</span></span>

6. <span data-ttu-id="1de8d-197">Klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-197">Click **Open**.</span></span> <span data-ttu-id="1de8d-198">Du kan bli aviserad genom en meddelanderuta.</span><span class="sxs-lookup"><span data-stu-id="1de8d-198">You might be alerted by a message box.</span></span> <span data-ttu-id="1de8d-199">Om du har konfigurerat hello DNS-namn och portnummer korrekt, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="1de8d-199">If you have configured hello DNS name and port number correctly, click **Yes**.</span></span>
<span data-ttu-id="1de8d-200">![Skärmbild som visar hello-meddelande][11]</span><span class="sxs-lookup"><span data-stu-id="1de8d-200">![Screenshot that shows hello notification][11]</span></span>

7. <span data-ttu-id="1de8d-201">Du är tooenter ange ditt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-201">You are prompted tooenter your username.</span></span>  
![Skärmbild som visar var tooenter användarnamn][12]

8. <span data-ttu-id="1de8d-203">Ange hello användarnamn som du använt toocreate hello virtuell dator i hello ”fas 1: skapa en avbildning” tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="1de8d-203">Enter hello username that you used toocreate hello virtual machine in hello "Phase 1: Create an image" section earlier in this article.</span></span> <span data-ttu-id="1de8d-204">Du ser något som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="1de8d-204">You will see something like hello following:</span></span>  
![Skärmbild som visar hello autentisering bekräftelse][13]

## <a name="phase-3-install-software"></a><span data-ttu-id="1de8d-206">Fas 3: Installera programvara</span><span class="sxs-lookup"><span data-stu-id="1de8d-206">Phase 3: Install software</span></span>
<span data-ttu-id="1de8d-207">I det här steget installera hello Java runtime environment, Tomcat7 och andra Tomcat7-komponenter.</span><span class="sxs-lookup"><span data-stu-id="1de8d-207">In this phase, you install hello Java runtime environment, Tomcat7, and other Tomcat7 components.</span></span>  

### <a name="java-runtime-environment"></a><span data-ttu-id="1de8d-208">Java runtime environment</span><span class="sxs-lookup"><span data-stu-id="1de8d-208">Java runtime environment</span></span>
<span data-ttu-id="1de8d-209">Tomcat är skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="1de8d-209">Tomcat is written in Java.</span></span> <span data-ttu-id="1de8d-210">Det finns två typer av Java Development Kit (JDKs), OpenJDK och Oracle JDK.</span><span class="sxs-lookup"><span data-stu-id="1de8d-210">There are two kinds of Java Development Kits (JDKs), OpenJDK and Oracle JDK.</span></span> <span data-ttu-id="1de8d-211">Du kan välja hello du.</span><span class="sxs-lookup"><span data-stu-id="1de8d-211">You can choose hello one you want.</span></span>  

> [!NOTE]
> <span data-ttu-id="1de8d-212">Både JDKs har nästan hello samma Platskod för hello klasser i hello Java API, men hello koden för hello virtuell dator är olika.</span><span class="sxs-lookup"><span data-stu-id="1de8d-212">Both JDKs have almost hello same code for hello classes in hello Java API, but hello code for hello virtual machine is different.</span></span> <span data-ttu-id="1de8d-213">OpenJDK tenderar toouse öppna bibliotek, medan Oracle JDK tenderar toouse stängd viktiga.</span><span class="sxs-lookup"><span data-stu-id="1de8d-213">OpenJDK tends toouse open libraries, while Oracle JDK tends toouse closed ones.</span></span> <span data-ttu-id="1de8d-214">Oracle JDK har flera klasser och vissa fast buggar och Oracle JDK är mer stabilt än OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="1de8d-214">Oracle JDK has more classes and some fixed bugs, and Oracle JDK is more stable than OpenJDK.</span></span>

#### <a name="install-openjdk"></a><span data-ttu-id="1de8d-215">Installera OpenJDK</span><span class="sxs-lookup"><span data-stu-id="1de8d-215">Install OpenJDK</span></span>  

<span data-ttu-id="1de8d-216">Använd följande kommando toodownload OpenJDK hello.</span><span class="sxs-lookup"><span data-stu-id="1de8d-216">Use hello following command toodownload OpenJDK.</span></span>   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* <span data-ttu-id="1de8d-217">toocreate en directory toocontain hello JDK-filer:</span><span class="sxs-lookup"><span data-stu-id="1de8d-217">toocreate a directory toocontain hello JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="1de8d-218">tooextract hello JDK filer i katalogen/usr/lib/jvm/hello:</span><span class="sxs-lookup"><span data-stu-id="1de8d-218">tooextract hello JDK files into hello /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a><span data-ttu-id="1de8d-219">Installera Oracle JDK</span><span class="sxs-lookup"><span data-stu-id="1de8d-219">Install Oracle JDK</span></span>


<span data-ttu-id="1de8d-220">Använd hello efter kommandot toodownload Oracle JDK från hello Oracle webbplats.</span><span class="sxs-lookup"><span data-stu-id="1de8d-220">Use hello following command toodownload Oracle JDK from hello Oracle website.</span></span>  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* <span data-ttu-id="1de8d-221">toocreate en directory toocontain hello JDK-filer:</span><span class="sxs-lookup"><span data-stu-id="1de8d-221">toocreate a directory toocontain hello JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="1de8d-222">tooextract hello JDK filer i katalogen/usr/lib/jvm/hello:</span><span class="sxs-lookup"><span data-stu-id="1de8d-222">tooextract hello JDK files into hello /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* <span data-ttu-id="1de8d-223">tooset Oracle JDK som hello standard Java virtual machine:</span><span class="sxs-lookup"><span data-stu-id="1de8d-223">tooset Oracle JDK as hello default Java virtual machine:</span></span>  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a><span data-ttu-id="1de8d-224">Bekräfta att Java-installationen har lyckats</span><span class="sxs-lookup"><span data-stu-id="1de8d-224">Confirm that Java installation is successful</span></span>
<span data-ttu-id="1de8d-225">Du kan använda ett kommando som hello efter tootest om hello Java runtime environment har installerats på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="1de8d-225">You can use a command like hello following tootest if hello Java runtime environment is installed correctly:</span></span>  

    java -version  

<span data-ttu-id="1de8d-226">Om du har installerat OpenJDK, bör du se ett meddelande som hello nedan: ![lyckade OpenJDK installationen visas][14]</span><span class="sxs-lookup"><span data-stu-id="1de8d-226">If you installed OpenJDK, you should see a message like hello following: ![Successful OpenJDK installation message][14]</span></span>

<span data-ttu-id="1de8d-227">Om du har installerat Oracle JDK, bör du se ett meddelande som hello nedan: ![lyckade Oracle JDK installationen visas][15]</span><span class="sxs-lookup"><span data-stu-id="1de8d-227">If you installed Oracle JDK, you should see a message like hello following: ![Successful Oracle JDK installation message][15]</span></span>

### <a name="install-tomcat7"></a><span data-ttu-id="1de8d-228">Installera Tomcat7</span><span class="sxs-lookup"><span data-stu-id="1de8d-228">Install Tomcat7</span></span>
<span data-ttu-id="1de8d-229">Använd följande kommando tooinstall Tomcat7 hello.</span><span class="sxs-lookup"><span data-stu-id="1de8d-229">Use hello following command tooinstall Tomcat7.</span></span>  

    sudo apt-get install tomcat7  

<span data-ttu-id="1de8d-230">Om du inte använder Tomcat7, använder du hello lämplig variation av det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="1de8d-230">If you are not using Tomcat7, use hello appropriate variation of this command.</span></span>  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a><span data-ttu-id="1de8d-231">Bekräfta att Tomcat7 installationen har lyckats</span><span class="sxs-lookup"><span data-stu-id="1de8d-231">Confirm that Tomcat7 installation is successful</span></span>
<span data-ttu-id="1de8d-232">toocheck om Tomcat7 har installerats, bläddra tooyour Tomcat server DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-232">toocheck if Tomcat7 is successfully installed, browse tooyour Tomcat server’s DNS name.</span></span> <span data-ttu-id="1de8d-233">Hello exempel-URL är http://tomcatexample.cloudapp.net/ i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="1de8d-233">In this article, hello example URL is http://tomcatexample.cloudapp.net/.</span></span> <span data-ttu-id="1de8d-234">Om du ser ett meddelande som hello följande är Tomcat7 korrekt installerad.</span><span class="sxs-lookup"><span data-stu-id="1de8d-234">If you see a message like hello following, Tomcat7 is installed correctly.</span></span>
<span data-ttu-id="1de8d-235">![Lyckad Tomcat7 installationen visas][16]</span><span class="sxs-lookup"><span data-stu-id="1de8d-235">![Successful Tomcat7 installation message][16]</span></span>

### <a name="install-other-tomcat7-components"></a><span data-ttu-id="1de8d-236">Installera komponenter för andra Tomcat7</span><span class="sxs-lookup"><span data-stu-id="1de8d-236">Install other Tomcat7 components</span></span>
<span data-ttu-id="1de8d-237">Det finns andra valfria Tomcat-komponenter som du kan installera.</span><span class="sxs-lookup"><span data-stu-id="1de8d-237">There are other optional Tomcat components that you can install.</span></span>  

<span data-ttu-id="1de8d-238">Använd hello **sudo lgh cache-sökning tomcat7** kommandot toosee alla tillgängliga hello-komponenter.</span><span class="sxs-lookup"><span data-stu-id="1de8d-238">Use hello **sudo apt-cache search tomcat7** command toosee all of hello available components.</span></span> <span data-ttu-id="1de8d-239">Använd följande kommandon tooinstall hello vissa användbara komponenter.</span><span class="sxs-lookup"><span data-stu-id="1de8d-239">Use hello following commands tooinstall some useful components.</span></span>  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools toocreate user instances  

## <a name="phase-4-configure-tomcat7"></a><span data-ttu-id="1de8d-240">Steg 4: Konfigurera Tomcat7</span><span class="sxs-lookup"><span data-stu-id="1de8d-240">Phase 4: Configure Tomcat7</span></span>
<span data-ttu-id="1de8d-241">I det här steget kan du administrera Tomcat.</span><span class="sxs-lookup"><span data-stu-id="1de8d-241">In this phase, you administer Tomcat.</span></span>

### <a name="start-and-stop-tomcat7"></a><span data-ttu-id="1de8d-242">Starta och stoppa Tomcat7</span><span class="sxs-lookup"><span data-stu-id="1de8d-242">Start and stop Tomcat7</span></span>
<span data-ttu-id="1de8d-243">Hej Tomcat7 servern startar automatiskt när du installerar den.</span><span class="sxs-lookup"><span data-stu-id="1de8d-243">hello Tomcat7 server automatically starts when you install it.</span></span> <span data-ttu-id="1de8d-244">Du kan också starta den med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1de8d-244">You can also start it with hello following command:</span></span>   

    sudo /etc/init.d/tomcat7 start

<span data-ttu-id="1de8d-245">toostop Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="1de8d-245">toostop Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 stop

<span data-ttu-id="1de8d-246">tooview hello status för Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="1de8d-246">tooview hello status of Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 status

<span data-ttu-id="1de8d-247">toorestart Tomcat-tjänster:</span><span class="sxs-lookup"><span data-stu-id="1de8d-247">toorestart Tomcat services:</span></span> 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a><span data-ttu-id="1de8d-248">Tomcat7 administration</span><span class="sxs-lookup"><span data-stu-id="1de8d-248">Tomcat7 administration</span></span>
<span data-ttu-id="1de8d-249">Du kan redigera hello Tomcat användaren configuration file tooset in dina administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1de8d-249">You can edit hello Tomcat user configuration file tooset up your admin credentials.</span></span> <span data-ttu-id="1de8d-250">Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1de8d-250">Use hello following command:</span></span>  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

<span data-ttu-id="1de8d-251">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="1de8d-251">Here is an example:</span></span>  
![Skärmbild som visar hello sudo vi kommandoutdata][17]  

> [!NOTE]
> <span data-ttu-id="1de8d-253">Skapa ett starkt lösenord för hello administratörsanvändarnamnet.</span><span class="sxs-lookup"><span data-stu-id="1de8d-253">Create a strong password for hello admin username.</span></span>  

<span data-ttu-id="1de8d-254">När du redigerar filen, bör du starta om Tomcat7 tjänster med hello efter kommandot tooensure att hello ändringarna gälla:</span><span class="sxs-lookup"><span data-stu-id="1de8d-254">After editing this file, you should restart Tomcat7 services with hello following command tooensure that hello changes take effect:</span></span>  

    sudo /etc/init.d/tomcat7 restart  

<span data-ttu-id="1de8d-255">Öppna din webbläsare och ange **http://<your tomcat server DNS name>/manager/html** som hello URL.</span><span class="sxs-lookup"><span data-stu-id="1de8d-255">Open your browser, and enter **http://<your tomcat server DNS name>/manager/html** as hello URL.</span></span> <span data-ttu-id="1de8d-256">I den här artikeln hello exempelvis är hello URL http://tomcatexample.cloudapp.net/manager/html.</span><span class="sxs-lookup"><span data-stu-id="1de8d-256">For hello example in this article, hello URL is http://tomcatexample.cloudapp.net/manager/html.</span></span>  

<span data-ttu-id="1de8d-257">Efter anslutning, bör du se något liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="1de8d-257">After connecting, you should see something similar toohello following:</span></span>  
![Skärmbild av hello Tomcat Web Application Manager][18]

## <a name="common-issues"></a><span data-ttu-id="1de8d-259">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="1de8d-259">Common issues</span></span>
### <a name="cant-access-hello-virtual-machine-with-tomcat-and-moodle-from-hello-internet"></a><span data-ttu-id="1de8d-260">Det går inte att komma åt hello virtuell dator med Tomcat och Moodle från hello Internet</span><span class="sxs-lookup"><span data-stu-id="1de8d-260">Can't access hello virtual machine with Tomcat and Moodle from hello Internet</span></span>
#### <a name="symptom"></a><span data-ttu-id="1de8d-261">Symtom</span><span class="sxs-lookup"><span data-stu-id="1de8d-261">Symptom</span></span>  
  <span data-ttu-id="1de8d-262">Tomcat körs men du kan inte se hello Tomcat standardsida med din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="1de8d-262">Tomcat is running but you can’t see hello Tomcat default page with your browser.</span></span>
#### <a name="possible-root-cause"></a><span data-ttu-id="1de8d-263">Möjliga underliggande orsaker</span><span class="sxs-lookup"><span data-stu-id="1de8d-263">Possible root cause</span></span>   

  * <span data-ttu-id="1de8d-264">Hej Tomcat lyssna port är inte hello samma som hello privat port för den virtuella datorns slutpunkten för Tomcat-trafik.</span><span class="sxs-lookup"><span data-stu-id="1de8d-264">hello Tomcat listen port is not hello same as hello private port of your virtual machine's endpoint for Tomcat traffic.</span></span>  

     <span data-ttu-id="1de8d-265">Kontrollera din offentliga porten och privat portinställningarna endpoint och kontrollera hello privat port är hello samma som hello Tomcat lyssningsport.</span><span class="sxs-lookup"><span data-stu-id="1de8d-265">Check your public port and private port endpoint settings and make sure hello private port is hello same as hello Tomcat listen port.</span></span> <span data-ttu-id="1de8d-266">Se ”fas 1: skapa en avbildning” i den här artikeln för instruktioner om hur du konfigurerar slutpunkter för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-266">See "Phase 1: Create an image" section of this article for instructions on configuring endpoints for your virtual machine.</span></span>  

     <span data-ttu-id="1de8d-267">toodetermine hello Tomcat lyssningsport, öppna /etc/httpd/conf/httpd.conf (Red Hat-version) eller /etc/tomcat7/server.xml (Debian-version).</span><span class="sxs-lookup"><span data-stu-id="1de8d-267">toodetermine hello Tomcat listen port, open /etc/httpd/conf/httpd.conf (Red Hat release), or /etc/tomcat7/server.xml (Debian release).</span></span> <span data-ttu-id="1de8d-268">Som standard är hello Tomcat lyssna port 8080.</span><span class="sxs-lookup"><span data-stu-id="1de8d-268">By default, hello Tomcat listen port is 8080.</span></span> <span data-ttu-id="1de8d-269">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="1de8d-269">Here is an example:</span></span>  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     <span data-ttu-id="1de8d-270">Om du använder en virtuell dator som Debian och Ubuntu och du vill att toochange hello standard port för Tomcat lyssna (till exempel 8081) kan öppna du också hello port för hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="1de8d-270">If you are using a virtual machine like Debian or Ubuntu and you want toochange hello default port of Tomcat Listen (for example 8081), you should also open hello port for hello operating system.</span></span> <span data-ttu-id="1de8d-271">Öppna först hello-profil:</span><span class="sxs-lookup"><span data-stu-id="1de8d-271">First, open hello profile:</span></span>  

        sudo vi /etc/default/tomcat7  

     <span data-ttu-id="1de8d-272">Sedan ta bort kommentarerna hello sista raden och ändra ”Nej” för ”Ja”.</span><span class="sxs-lookup"><span data-stu-id="1de8d-272">Then uncomment hello last line and change “no” too“yes”.</span></span>  

        AUTHBIND=yes
  2. <span data-ttu-id="1de8d-273">hello-brandväggen har inaktiverat hello lyssna port för Tomcat.</span><span class="sxs-lookup"><span data-stu-id="1de8d-273">hello firewall has disabled hello listen port of Tomcat.</span></span>

     <span data-ttu-id="1de8d-274">Du kan bara se hello Tomcat standardsida från hello lokala värden.</span><span class="sxs-lookup"><span data-stu-id="1de8d-274">You can only see hello Tomcat default page from hello local host.</span></span> <span data-ttu-id="1de8d-275">hello problemet sannolikt att hello-port som lyssnar tooby Tomcat, blockeras av hello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="1de8d-275">hello problem is most likely that hello port, which is listened tooby Tomcat, is blocked by hello firewall.</span></span> <span data-ttu-id="1de8d-276">Du kan använda hello w3m verktyget toobrowse hello webbsidan.</span><span class="sxs-lookup"><span data-stu-id="1de8d-276">You can use hello w3m tool toobrowse hello webpage.</span></span> <span data-ttu-id="1de8d-277">hello följande kommandon installera w3m och bläddra toohello Tomcat standardsida:</span><span class="sxs-lookup"><span data-stu-id="1de8d-277">hello following commands install w3m and browse toohello Tomcat default page:</span></span>  


        <span data-ttu-id="1de8d-278">sudo yum installera w3m w3m-img</span><span class="sxs-lookup"><span data-stu-id="1de8d-278">sudo yum  install w3m w3m-img</span></span>


        <span data-ttu-id="1de8d-279">w3m http://localhost: 8080</span><span class="sxs-lookup"><span data-stu-id="1de8d-279">w3m http://localhost:8080</span></span>  
#### <a name="solution"></a><span data-ttu-id="1de8d-280">Lösning</span><span class="sxs-lookup"><span data-stu-id="1de8d-280">Solution</span></span>

  * <span data-ttu-id="1de8d-281">Om hello lyssningsporten för Tomcat inte är hello samma som hello privat port för hello slutpunkten för trafik toohello virtuell dator, behöver du ändra hello privat port toobe hello samma som hello lyssningsporten för Tomcat.</span><span class="sxs-lookup"><span data-stu-id="1de8d-281">If hello Tomcat listen port is not hello same as hello private port of hello endpoint for traffic toohello virtual machine, you need change hello private port toobe hello same as hello Tomcat listen port.</span></span>   
  2. <span data-ttu-id="1de8d-282">Lägg till följande rader för/etc/sysconfig/iptables hello om hello problemet orsakas av brandvägg/iptables.</span><span class="sxs-lookup"><span data-stu-id="1de8d-282">If hello issue is caused by firewall/iptables, add hello following lines too/etc/sysconfig/iptables.</span></span> <span data-ttu-id="1de8d-283">hello andra raden krävs endast för https-trafik:</span><span class="sxs-lookup"><span data-stu-id="1de8d-283">hello second line is only needed for https traffic:</span></span>  

      <span data-ttu-id="1de8d-284">-A -p tcp -m tcp--datorer 80 -j ACCEPTERA indata</span><span class="sxs-lookup"><span data-stu-id="1de8d-284">-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT</span></span>

      <span data-ttu-id="1de8d-285">-A -p tcp -m tcp--datorer 443 -j ACCEPTERA indata</span><span class="sxs-lookup"><span data-stu-id="1de8d-285">-A INPUT -p tcp -m tcp --dport 443 -j ACCEPT</span></span>  

     > [!IMPORTANT]
     > <span data-ttu-id="1de8d-286">Kontrollera att hello tidigare rader placeras ovanför de rader som skulle globalt att begränsa åtkomsten till exempel hello följande: - A indata -j AVVISA--avvisa-med icmp värden förbjuden</span><span class="sxs-lookup"><span data-stu-id="1de8d-286">Make sure hello previous lines are positioned above any lines that would globally restrict access, such as hello following: -A INPUT -j REJECT --reject-with icmp-host-prohibited</span></span>



<span data-ttu-id="1de8d-287">tooreload hello iptables kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1de8d-287">tooreload hello iptables, run hello following command:</span></span>

    service iptables restart

<span data-ttu-id="1de8d-288">Det här har testats på CentOS 6.3.</span><span class="sxs-lookup"><span data-stu-id="1de8d-288">This has been tested on CentOS 6.3.</span></span>

### <a name="permission-denied-when-you-upload-project-files-toovarlibtomcat7webapps"></a><span data-ttu-id="1de8d-289">Åtkomst nekad när du överför projektet filer för/var/lib/tomcat7/webbappar /</span><span class="sxs-lookup"><span data-stu-id="1de8d-289">Permission denied when you upload project files too/var/lib/tomcat7/webapps/</span></span>
#### <a name="symptom"></a><span data-ttu-id="1de8d-290">Symtom</span><span class="sxs-lookup"><span data-stu-id="1de8d-290">Symptom</span></span>
  <span data-ttu-id="1de8d-291">När du använder en SFTP (till exempel FileZilla) tooconnect tooyour virtuella klientdatorn och gå för/var/lib/tomcat7/webbappar/toopublish webbplatsen du får ett fel meddelande liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="1de8d-291">When you use an SFTP client (such as FileZilla) tooconnect tooyour virtual machine and navigate too/var/lib/tomcat7/webapps/ toopublish your site, you get an error message similar toohello following:</span></span>  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a><span data-ttu-id="1de8d-292">Möjliga underliggande orsaker</span><span class="sxs-lookup"><span data-stu-id="1de8d-292">Possible root cause</span></span>
  <span data-ttu-id="1de8d-293">Du har inga behörigheter tooaccess hello /var/lib/tomcat7/webapps mapp.</span><span class="sxs-lookup"><span data-stu-id="1de8d-293">You have no permissions tooaccess hello /var/lib/tomcat7/webapps folder.</span></span>  
#### <a name="solution"></a><span data-ttu-id="1de8d-294">Lösning</span><span class="sxs-lookup"><span data-stu-id="1de8d-294">Solution</span></span>  
  <span data-ttu-id="1de8d-295">Du behöver tooget behörighet från hello rotkontot.</span><span class="sxs-lookup"><span data-stu-id="1de8d-295">You need tooget permission from hello root account.</span></span> <span data-ttu-id="1de8d-296">Du kan ändra hello ägare till den mappen i roten toohello användarnamn som du använde när du har etablerat hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="1de8d-296">You can change hello ownership of that folder from root toohello username you used when you provisioned hello machine.</span></span> <span data-ttu-id="1de8d-297">Här är ett exempel med hello azureuser kontonamn:</span><span class="sxs-lookup"><span data-stu-id="1de8d-297">Here is an example with hello azureuser account name:</span></span>  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  <span data-ttu-id="1de8d-298">Använd hello -R alternativet tooapply hello behörigheter för alla filer i en katalog för.</span><span class="sxs-lookup"><span data-stu-id="1de8d-298">Use hello -R option tooapply hello permissions for all files inside of a directory too.</span></span>  

  <span data-ttu-id="1de8d-299">Det här kommandot fungerar även för kataloger.</span><span class="sxs-lookup"><span data-stu-id="1de8d-299">This command also works for directories.</span></span> <span data-ttu-id="1de8d-300">hello -R alternativet ändringar hello behörigheter för alla filer och kataloger i hello directory.</span><span class="sxs-lookup"><span data-stu-id="1de8d-300">hello -R option changes hello permissions for all files and directories inside hello directory.</span></span> <span data-ttu-id="1de8d-301">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="1de8d-301">Here is an example:</span></span>  

     sudo chown -R username:group directory  

  <span data-ttu-id="1de8d-302">Det här kommandot ändrar ägarskap (både användare och grupper) för alla filer och mappar som finns inuti hello directory.</span><span class="sxs-lookup"><span data-stu-id="1de8d-302">This command changes ownership (both user and group) for all files and directories that are inside hello directory.</span></span>  

  <span data-ttu-id="1de8d-303">hello ändrar följande kommando bara hello behörighet för hello mappkatalog.</span><span class="sxs-lookup"><span data-stu-id="1de8d-303">hello following command only changes hello permission of hello folder directory.</span></span> <span data-ttu-id="1de8d-304">hello filer och mappar i hello directory ändras inte.</span><span class="sxs-lookup"><span data-stu-id="1de8d-304">hello files and folders inside hello directory are not changed.</span></span>  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
