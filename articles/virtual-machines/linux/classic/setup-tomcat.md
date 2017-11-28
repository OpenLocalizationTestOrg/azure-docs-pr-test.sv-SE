---
title: "Ställa in Apache Tomcat på en Linux-dator | Microsoft Docs"
description: "Lär dig hur du ställer in Apache Tomcat7 med hjälp av Azure virtuella datorer som kör Linux."
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
ms.openlocfilehash: fa30c78a5a5d458ba8845c3c10b87538427786c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a><span data-ttu-id="5e307-103">Ställa in Tomcat7 på en Linux-dator med Azure</span><span class="sxs-lookup"><span data-stu-id="5e307-103">Set up Tomcat7 on a Linux virtual machine with Azure</span></span>
<span data-ttu-id="5e307-104">Apache Tomcat (eller bara Tomcat kan också kallades Djakarta Tomcat) är en öppen källkod webbservern och servlet-behållare som utvecklats av Apache Software Foundation (ASF).</span><span class="sxs-lookup"><span data-stu-id="5e307-104">Apache Tomcat (or simply Tomcat, also formerly called Jakarta Tomcat) is an open source web server and servlet container developed by the Apache Software Foundation (ASF).</span></span> <span data-ttu-id="5e307-105">Tomcat implementerar Java-Servlet och specifikationer för JavaServer sidor (JSP) från Sun Microsystems.</span><span class="sxs-lookup"><span data-stu-id="5e307-105">Tomcat implements the Java Servlet and the JavaServer Pages (JSP) specifications from Sun Microsystems.</span></span> <span data-ttu-id="5e307-106">Tomcat ger en ren Java HTTP web server-miljö där du kan köra Java-kod.</span><span class="sxs-lookup"><span data-stu-id="5e307-106">Tomcat provides a pure Java HTTP web server environment in which to run Java code.</span></span> <span data-ttu-id="5e307-107">I den enklaste konfigurationen körs Tomcat i en enda process.</span><span class="sxs-lookup"><span data-stu-id="5e307-107">In the simplest configuration, Tomcat runs in a single operating system process.</span></span> <span data-ttu-id="5e307-108">Den här processen körs en Java virtual machine (JVM).</span><span class="sxs-lookup"><span data-stu-id="5e307-108">This process runs a Java virtual machine (JVM).</span></span> <span data-ttu-id="5e307-109">Alla HTTP-begäran från en webbläsare till Tomcat behandlas som en separat tråd i Tomcat-processen.</span><span class="sxs-lookup"><span data-stu-id="5e307-109">Every HTTP request from a browser to Tomcat is processed as a separate thread in the Tomcat process.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="5e307-110">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5e307-110">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5e307-111">Den här artikeln beskriver hur du använder den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="5e307-111">This article covers how to use the classic deployment model.</span></span> <span data-ttu-id="5e307-112">Vi rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="5e307-112">We recommend that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="5e307-113">Om du vill använda en Resource Manager-mall för att distribuera en VM Ubuntu med öppna JDK och Tomcat, se [i den här artikeln](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span><span class="sxs-lookup"><span data-stu-id="5e307-113">To use a Resource Manager template to deploy an Ubuntu VM with Open JDK and Tomcat, see [this article](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).</span></span>

<span data-ttu-id="5e307-114">I den här artikeln får du installerar Tomcat7 på en Linux-avbildning och distribuera den i Azure.</span><span class="sxs-lookup"><span data-stu-id="5e307-114">In this article, you will install Tomcat7 on a Linux image and deploy it in Azure.</span></span>  

<span data-ttu-id="5e307-115">Du kommer att lära dig:</span><span class="sxs-lookup"><span data-stu-id="5e307-115">You will learn:</span></span>  

* <span data-ttu-id="5e307-116">Så här skapar du en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="5e307-116">How to create a virtual machine in Azure.</span></span>
* <span data-ttu-id="5e307-117">Hur du förbereder den virtuella datorn för Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="5e307-117">How to prepare the virtual machine for Tomcat7.</span></span>
* <span data-ttu-id="5e307-118">Så här installerar du Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="5e307-118">How to install Tomcat7.</span></span>

<span data-ttu-id="5e307-119">Det förutsätts att du redan har en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5e307-119">It is assumed that you already have an Azure subscription.</span></span>  <span data-ttu-id="5e307-120">Om inte, du kan registrera dig för en kostnadsfri utvärderingsversion på [Azure-webbplatsen](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="5e307-120">If not, you can sign up for a free trial at [the Azure website](https://azure.microsoft.com/).</span></span> <span data-ttu-id="5e307-121">Om du har en MSDN-prenumeration, se [Microsoft särskilda priser för Azure: MSDN, MPN och BizSpark fördelar](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span><span class="sxs-lookup"><span data-stu-id="5e307-121">If you have an MSDN subscription, see [Microsoft Azure Special Pricing: MSDN, MPN, and BizSpark Benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39).</span></span> <span data-ttu-id="5e307-122">Läs mer om Azure i [vad är Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span><span class="sxs-lookup"><span data-stu-id="5e307-122">To learn more about Azure, see [What is Azure?](https://azure.microsoft.com/overview/what-is-azure/).</span></span>

<span data-ttu-id="5e307-123">Den här artikeln förutsätter att du har grundläggande kunskaper om Tomcat- och Linux.</span><span class="sxs-lookup"><span data-stu-id="5e307-123">This article assumes that you have a basic working knowledge of Tomcat and Linux.</span></span>  

## <a name="phase-1-create-an-image"></a><span data-ttu-id="5e307-124">Fas 1: Skapa en avbildning</span><span class="sxs-lookup"><span data-stu-id="5e307-124">Phase 1: Create an image</span></span>
<span data-ttu-id="5e307-125">I det här steget ska du skapa en virtuell dator med hjälp av en Linux-avbildning i Azure.</span><span class="sxs-lookup"><span data-stu-id="5e307-125">In this phase, you will create a virtual machine by using a Linux image in Azure.</span></span>  

### <a name="step-1-generate-an-ssh-authentication-key"></a><span data-ttu-id="5e307-126">Steg 1: Skapa en SSH-nyckel för autentisering</span><span class="sxs-lookup"><span data-stu-id="5e307-126">Step 1: Generate an SSH authentication key</span></span>
<span data-ttu-id="5e307-127">SSH är ett viktigt verktyg för administratörer.</span><span class="sxs-lookup"><span data-stu-id="5e307-127">SSH is an important tool for system administrators.</span></span> <span data-ttu-id="5e307-128">Dock rekommenderas konfigurera åtkomstsäkerhet baserat på ett fastställt mänskliga lösenord inte.</span><span class="sxs-lookup"><span data-stu-id="5e307-128">However, configuring access security based on a human-determined password is not recommended.</span></span> <span data-ttu-id="5e307-129">Angripare kan dela upp i ditt system baserat på ett användarnamn och ett svagt lösenord.</span><span class="sxs-lookup"><span data-stu-id="5e307-129">Malicious users can break into your system based on a username and a weak password.</span></span>

<span data-ttu-id="5e307-130">Goda nyheter är att det är ett sätt att lämna fjärråtkomst öppen och oroa dig inte om lösenord.</span><span class="sxs-lookup"><span data-stu-id="5e307-130">The good news is that there is a way to leave remote access open and not worry about passwords.</span></span> <span data-ttu-id="5e307-131">Den här metoden består av autentisering med asymmetrisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="5e307-131">This method consists of authentication with asymmetric cryptography.</span></span> <span data-ttu-id="5e307-132">Den privata nyckeln är den som ger autentisering.</span><span class="sxs-lookup"><span data-stu-id="5e307-132">The user’s private key is the one that grants the authentication.</span></span> <span data-ttu-id="5e307-133">Du kan även låsa användarens konto för att inte tillåta lösenordsautentisering.</span><span class="sxs-lookup"><span data-stu-id="5e307-133">You can even lock the user’s account to not allow password authentication.</span></span>

<span data-ttu-id="5e307-134">En annan fördel med den här metoden är att du inte behöver olika lösenord för att logga in på olika servrar.</span><span class="sxs-lookup"><span data-stu-id="5e307-134">Another advantage of this method is that you do not need different passwords to sign in to different servers.</span></span> <span data-ttu-id="5e307-135">Du kan autentisera med hjälp av den personliga privata nyckeln på alla servrar som förhindrar att behöva komma ihåg flera lösenord.</span><span class="sxs-lookup"><span data-stu-id="5e307-135">You can authenticate by using the personal private key on all servers, which prevents you from having to remember several passwords.</span></span>



<span data-ttu-id="5e307-136">Följ dessa steg för att generera nyckeln SSH-autentisering.</span><span class="sxs-lookup"><span data-stu-id="5e307-136">Follow these steps to generate the SSH authentication key.</span></span>

1. <span data-ttu-id="5e307-137">Hämta och installera PuTTYgen från följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="5e307-137">Download and install PuTTYgen from the following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="5e307-138">Kör Puttygen.exe.</span><span class="sxs-lookup"><span data-stu-id="5e307-138">Run Puttygen.exe.</span></span>
3. <span data-ttu-id="5e307-139">Klicka på **generera** att generera nycklar.</span><span class="sxs-lookup"><span data-stu-id="5e307-139">Click **Generate** to generate the keys.</span></span> <span data-ttu-id="5e307-140">Du kan öka slumpmässighet genom att flytta musen över det tomma utrymmet i fönstret i processen.</span><span class="sxs-lookup"><span data-stu-id="5e307-140">In the process, you can increase randomness by moving the mouse over the blank area in the window.</span></span>  
   <span data-ttu-id="5e307-141">![PuTTY nyckel Generator skärmbild som visar knappen Skapa ny nyckel][1]</span><span class="sxs-lookup"><span data-stu-id="5e307-141">![PuTTY Key Generator screenshot that shows the generate new key button][1]</span></span>
4. <span data-ttu-id="5e307-142">När processen generera visas Puttygen.exe den offentliga nyckeln.</span><span class="sxs-lookup"><span data-stu-id="5e307-142">After the generate process, Puttygen.exe will show your new public key.</span></span>  
   ![PuTTY nyckel Generator skärmbild som visar den offentliga nyckeln och Spara privat nyckel knappen][2]
5. <span data-ttu-id="5e307-144">Markera och kopiera den offentliga nyckeln och spara det i en fil med namnet publicKey.pem.</span><span class="sxs-lookup"><span data-stu-id="5e307-144">Select and copy the public key, and save it in a file named publicKey.pem.</span></span> <span data-ttu-id="5e307-145">Klicka inte på **spara offentlig nyckel**, eftersom den sparade offentliga nyckeln filformatet skiljer sig från den offentliga nyckeln som vi vill.</span><span class="sxs-lookup"><span data-stu-id="5e307-145">Don’t click **Save public key**, because the saved public key’s file format is different from the public key we want.</span></span>
6. <span data-ttu-id="5e307-146">Klicka på **Spara privat nyckel**, och spara den i en fil med namnet privateKey.ppk.</span><span class="sxs-lookup"><span data-stu-id="5e307-146">Click **Save private key**, and save it in a file named privateKey.ppk.</span></span>

### <a name="step-2-create-the-image-in-the-azure-portal"></a><span data-ttu-id="5e307-147">Steg 2: Skapa avbildningen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5e307-147">Step 2: Create the image in the Azure portal</span></span>
1. <span data-ttu-id="5e307-148">I den [portal](https://portal.azure.com/), klickar du på **ny** i Aktivitetsfältet för att skapa en avbildning.</span><span class="sxs-lookup"><span data-stu-id="5e307-148">In the [portal](https://portal.azure.com/), click **New** in the task bar to create an image.</span></span> <span data-ttu-id="5e307-149">Välj den Linux-avbildning som är baserad på dina behov.</span><span class="sxs-lookup"><span data-stu-id="5e307-149">Then choose the Linux image that is based on your needs.</span></span> <span data-ttu-id="5e307-150">I följande exempel används Ubuntu 14.04 bilden.</span><span class="sxs-lookup"><span data-stu-id="5e307-150">The following example uses the Ubuntu 14.04 image.</span></span>
<span data-ttu-id="5e307-151">![Skärmbild av portalen som visar knappen Nytt][3]</span><span class="sxs-lookup"><span data-stu-id="5e307-151">![Screenshot of the portal that shows the New button][3]</span></span>

2. <span data-ttu-id="5e307-152">För **värdnamn**, ange namnet på den URL som du och Internet-klienter använder för att komma åt den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-152">For **Host Name**, specify the name for the URL that you and Internet clients will use to access this virtual machine.</span></span> <span data-ttu-id="5e307-153">Definiera den sista delen av DNS-namn, till exempel tomcatdemo.</span><span class="sxs-lookup"><span data-stu-id="5e307-153">Define the last part of the DNS name, for example, tomcatdemo.</span></span> <span data-ttu-id="5e307-154">Azure skapar sedan URL: en som tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="5e307-154">Azure will then generate the URL as tomcatdemo.cloudapp.net.</span></span>  

3. <span data-ttu-id="5e307-155">För **SSH autentiseringsnyckel**, Kopiera värdet för nyckeln från filen publicKey.pem, som innehåller den offentliga nyckeln som genererats av PuTTYgen.</span><span class="sxs-lookup"><span data-stu-id="5e307-155">For **SSH Authentication Key**, copy the key value from the publicKey.pem file, which contains the public key generated by PuTTYgen.</span></span>  
<span data-ttu-id="5e307-156">![SSH-autentiseringsnyckel rutan i portalen][4]</span><span class="sxs-lookup"><span data-stu-id="5e307-156">![SSH Authentication Key box in the portal][4]</span></span>

4. <span data-ttu-id="5e307-157">Konfigurera övriga inställningar efter behov och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5e307-157">Configure other settings as needed, and then click **Create**.</span></span>  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a><span data-ttu-id="5e307-158">Fas 2: Förbereda den virtuella datorn för Tomcat7</span><span class="sxs-lookup"><span data-stu-id="5e307-158">Phase 2: Prepare your virtual machine for Tomcat7</span></span>
<span data-ttu-id="5e307-159">I det här steget kommer du konfigurerar en slutpunkt för Tomcat trafik och ansluter sedan till den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-159">In this phase, you will configure an endpoint for Tomcat traffic, and then connect to your new virtual machine.</span></span>

### <a name="step-1-open-the-http-port-to-allow-web-access"></a><span data-ttu-id="5e307-160">Steg 1: Öppna HTTP-porten för att tillåta webbåtkomst</span><span class="sxs-lookup"><span data-stu-id="5e307-160">Step 1: Open the HTTP port to allow web access</span></span>
<span data-ttu-id="5e307-161">Slutpunkter i Azure består av ett TCP- eller UDP-protokoll, tillsammans med en offentlig och privat port.</span><span class="sxs-lookup"><span data-stu-id="5e307-161">Endpoints in Azure consist of a TCP or UDP protocol, along with a public and private port.</span></span> <span data-ttu-id="5e307-162">Den privata porten är den port som tjänsten lyssnar på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-162">The private port is the port that the service is listening to on the virtual machine.</span></span> <span data-ttu-id="5e307-163">Den offentliga porten är den port som Azure-Molntjänsten lyssnar på externt för inkommande, Internet-baserade trafik.</span><span class="sxs-lookup"><span data-stu-id="5e307-163">The public port is the port that the Azure cloud service listens to externally for incoming, Internet-based traffic.</span></span>  

<span data-ttu-id="5e307-164">TCP-port 8080 är standardportnumret som Tomcat använder för att lyssna.</span><span class="sxs-lookup"><span data-stu-id="5e307-164">TCP port 8080 is the default port number that Tomcat uses to listen.</span></span> <span data-ttu-id="5e307-165">Om den här porten är öppen med en Azure slutpunkt du och andra Internet-klienter kan komma åt Tomcat sidor.</span><span class="sxs-lookup"><span data-stu-id="5e307-165">If this port is opened with an Azure endpoint, you and other Internet clients can access Tomcat pages.</span></span>  

1. <span data-ttu-id="5e307-166">I portalen klickar du på **Bläddra** > **virtuella datorer**, och klicka sedan på den virtuella datorn som du skapade.</span><span class="sxs-lookup"><span data-stu-id="5e307-166">In the portal, click **Browse** > **Virtual machines**, and then click the virtual machine that you created.</span></span>  
   <span data-ttu-id="5e307-167">![Skärmbild av katalogen virtuella datorer][5]</span><span class="sxs-lookup"><span data-stu-id="5e307-167">![Screenshot of the Virtual machines directory][5]</span></span>
2. <span data-ttu-id="5e307-168">Om du vill lägga till en slutpunkt till den virtuella datorn klickar du på den **slutpunkter** rutan.</span><span class="sxs-lookup"><span data-stu-id="5e307-168">To add an endpoint to your virtual machine, click the **Endpoints** box.</span></span>
   <span data-ttu-id="5e307-169">![Skärmbild som visar rutan slutpunkter][6]</span><span class="sxs-lookup"><span data-stu-id="5e307-169">![Screenshot that shows the Endpoints box][6]</span></span>
3. <span data-ttu-id="5e307-170">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5e307-170">Click **Add**.</span></span>  

   1. <span data-ttu-id="5e307-171">Ange ett namn för slutpunkten i för slutpunkten, **Endpoint**, och ange sedan 80 i **offentlig Port**.</span><span class="sxs-lookup"><span data-stu-id="5e307-171">For the endpoint, enter a name for the endpoint in **Endpoint**, and then enter 80 in **Public Port**.</span></span>  

      <span data-ttu-id="5e307-172">Om du ställer in den 80, behöver du inte inkludera portnumret i den URL som används för att komma åt Tomcat.</span><span class="sxs-lookup"><span data-stu-id="5e307-172">If you set it to 80, you don’t need to include the port number in the URL that is used to access Tomcat.</span></span> <span data-ttu-id="5e307-173">Till exempel http://tomcatdemo.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="5e307-173">For example, http://tomcatdemo.cloudapp.net.</span></span>    

      <span data-ttu-id="5e307-174">Om du anger ett annat värde, till exempel 81, behöver du lägga till portnumret i URL: en för Tomcat.</span><span class="sxs-lookup"><span data-stu-id="5e307-174">If you set it to another value, such as 81, you need to add the port number to the URL to access Tomcat.</span></span> <span data-ttu-id="5e307-175">Till exempel http://tomcatdemo.cloudapp.net:81 /.</span><span class="sxs-lookup"><span data-stu-id="5e307-175">For example,  http://tomcatdemo.cloudapp.net:81/.</span></span>
   2. <span data-ttu-id="5e307-176">Ange 8080 i **privat Port**.</span><span class="sxs-lookup"><span data-stu-id="5e307-176">Enter 8080 in **Private Port**.</span></span> <span data-ttu-id="5e307-177">Som standard lyssnar Tomcat på TCP-port 8080.</span><span class="sxs-lookup"><span data-stu-id="5e307-177">By default, Tomcat listens on TCP port 8080.</span></span> <span data-ttu-id="5e307-178">Om du har ändrat standardinställningen lyssna port Tomcat, bör du uppdatera **privat Port** ska vara samma som Tomcat lyssningsport.</span><span class="sxs-lookup"><span data-stu-id="5e307-178">If you changed the default listen port of Tomcat, you should update **Private Port** to be the same as the Tomcat listen port.</span></span>  
      <span data-ttu-id="5e307-179">![Skärmbild av Användargränssnittet som visar Lägg till kommandot offentlig Port och privat Port][7]</span><span class="sxs-lookup"><span data-stu-id="5e307-179">![Screenshot of UI that shows Add command, Public Port, and Private Port][7]</span></span>
4. <span data-ttu-id="5e307-180">Klicka på **OK** att lägga till slutpunkten till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-180">Click **OK** to add the endpoint to your virtual machine.</span></span>

### <a name="step-2-connect-to-the-image-you-created"></a><span data-ttu-id="5e307-181">Steg 2: Anslut till den bild som du skapat</span><span class="sxs-lookup"><span data-stu-id="5e307-181">Step 2: Connect to the image you created</span></span>
<span data-ttu-id="5e307-182">Du kan välja ett SSH-verktyg för att ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-182">You can choose any SSH tool to connect to your virtual machine.</span></span> <span data-ttu-id="5e307-183">I det här exemplet använder vi PuTTY.</span><span class="sxs-lookup"><span data-stu-id="5e307-183">In this example, we use PuTTY.</span></span>  

1. <span data-ttu-id="5e307-184">Hämta DNS-namnet på den virtuella datorn från portalen.</span><span class="sxs-lookup"><span data-stu-id="5e307-184">Get the DNS name of your virtual machine from the portal.</span></span>
    1. <span data-ttu-id="5e307-185">Klicka på **Bläddra** > **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="5e307-185">Click **Browse** > **Virtual machines**.</span></span>
    2. <span data-ttu-id="5e307-186">Välj namnet på den virtuella datorn och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="5e307-186">Select the name of your virtual machine, and then click **Properties**.</span></span>
    3. <span data-ttu-id="5e307-187">I den **egenskaper** panelen, titta i den **domännamn** om du vill hämta DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="5e307-187">In the **Properties** tile, look in the **Domain Name** box to get the DNS name.</span></span>  

2. <span data-ttu-id="5e307-188">Hämta porten för SSH-anslutningar från den **SSH** rutan.</span><span class="sxs-lookup"><span data-stu-id="5e307-188">Get the port number for SSH connections from the **SSH** box.</span></span>  
<span data-ttu-id="5e307-189">![Skärmbild som visar portnumret för SSH-anslutning][8]</span><span class="sxs-lookup"><span data-stu-id="5e307-189">![Screenshot that shows the SSH connection port number][8]</span></span>

3. <span data-ttu-id="5e307-190">Hämta [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="5e307-190">Download [PuTTY](http://www.putty.org/).</span></span>  

4. <span data-ttu-id="5e307-191">Klicka på den körbara filen Putty.exe efter hämtningen.</span><span class="sxs-lookup"><span data-stu-id="5e307-191">After downloading, click the executable file Putty.exe.</span></span> <span data-ttu-id="5e307-192">Konfigurera alternativen för grundläggande med värdnamn i PuTTY-konfiguration och portnumret som hämtas från egenskaperna för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-192">In PuTTY configuration, configure the basic options with the host name and port number that is obtained from the properties of your virtual machine.</span></span>   
![Skärmbild som visar alternativ för namn på och portnummer för värden PuTTY-konfiguration][9]

5. <span data-ttu-id="5e307-194">I den vänstra rutan klickar du på **anslutning** > **SSH** > **Auth**, och klicka sedan på **Bläddra** att ange den platsen för filen privateKey.ppk.</span><span class="sxs-lookup"><span data-stu-id="5e307-194">In the left pane, click **Connection** > **SSH** > **Auth**, and then click **Browse** to specify the location of the privateKey.ppk file.</span></span> <span data-ttu-id="5e307-195">PrivateKey.ppk-filen innehåller den privata nyckeln som genereras av PuTTYgen tidigare i det ”fas 1: skapa en avbildning” i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5e307-195">The privateKey.ppk file contains the private key that is generated by PuTTYgen earlier in the "Phase 1: Create an image" section of this article.</span></span>  
<span data-ttu-id="5e307-196">![Skärmbild som visar anslutning directory-hierarkin och knappen Bläddra.][10]</span><span class="sxs-lookup"><span data-stu-id="5e307-196">![Screenshot that shows the Connection directory hierarchy and Browse button][10]</span></span>

6. <span data-ttu-id="5e307-197">Klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="5e307-197">Click **Open**.</span></span> <span data-ttu-id="5e307-198">Du kan bli aviserad genom en meddelanderuta.</span><span class="sxs-lookup"><span data-stu-id="5e307-198">You might be alerted by a message box.</span></span> <span data-ttu-id="5e307-199">Om du har konfigurerat DNS-namn och portnummer korrekt, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="5e307-199">If you have configured the DNS name and port number correctly, click **Yes**.</span></span>
<span data-ttu-id="5e307-200">![Skärmbild som visar meddelandet][11]</span><span class="sxs-lookup"><span data-stu-id="5e307-200">![Screenshot that shows the notification][11]</span></span>

7. <span data-ttu-id="5e307-201">Du uppmanas att ange ditt användarnamn.</span><span class="sxs-lookup"><span data-stu-id="5e307-201">You are prompted to enter your username.</span></span>  
![Skärmbild som visar var du vill ange ett användarnamn][12]

8. <span data-ttu-id="5e307-203">Ange det användarnamn som du använde för att skapa den virtuella datorn i den ”fas 1: skapa en avbildning” tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5e307-203">Enter the username that you used to create the virtual machine in the "Phase 1: Create an image" section earlier in this article.</span></span> <span data-ttu-id="5e307-204">Du ser något som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="5e307-204">You will see something like the following:</span></span>  
![Skärmbild som visar bekräftelse för autentisering][13]

## <a name="phase-3-install-software"></a><span data-ttu-id="5e307-206">Fas 3: Installera programvara</span><span class="sxs-lookup"><span data-stu-id="5e307-206">Phase 3: Install software</span></span>
<span data-ttu-id="5e307-207">I det här steget installera Java runtime environment, Tomcat7 och andra Tomcat7-komponenter.</span><span class="sxs-lookup"><span data-stu-id="5e307-207">In this phase, you install the Java runtime environment, Tomcat7, and other Tomcat7 components.</span></span>  

### <a name="java-runtime-environment"></a><span data-ttu-id="5e307-208">Java runtime environment</span><span class="sxs-lookup"><span data-stu-id="5e307-208">Java runtime environment</span></span>
<span data-ttu-id="5e307-209">Tomcat är skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="5e307-209">Tomcat is written in Java.</span></span> <span data-ttu-id="5e307-210">Det finns två typer av Java Development Kit (JDKs), OpenJDK och Oracle JDK.</span><span class="sxs-lookup"><span data-stu-id="5e307-210">There are two kinds of Java Development Kits (JDKs), OpenJDK and Oracle JDK.</span></span> <span data-ttu-id="5e307-211">Du kan välja det du vill använda.</span><span class="sxs-lookup"><span data-stu-id="5e307-211">You can choose the one you want.</span></span>  

> [!NOTE]
> <span data-ttu-id="5e307-212">Båda JDKs har nästan samma kod för klasser i Java-API, men koden för den virtuella datorn är olika.</span><span class="sxs-lookup"><span data-stu-id="5e307-212">Both JDKs have almost the same code for the classes in the Java API, but the code for the virtual machine is different.</span></span> <span data-ttu-id="5e307-213">OpenJDK tenderar att använda öppna bibliotek, medan Oracle JDK tenderar att använda stängd de.</span><span class="sxs-lookup"><span data-stu-id="5e307-213">OpenJDK tends to use open libraries, while Oracle JDK tends to use closed ones.</span></span> <span data-ttu-id="5e307-214">Oracle JDK har flera klasser och vissa fast buggar och Oracle JDK är mer stabilt än OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="5e307-214">Oracle JDK has more classes and some fixed bugs, and Oracle JDK is more stable than OpenJDK.</span></span>

#### <a name="install-openjdk"></a><span data-ttu-id="5e307-215">Installera OpenJDK</span><span class="sxs-lookup"><span data-stu-id="5e307-215">Install OpenJDK</span></span>  

<span data-ttu-id="5e307-216">Använd följande kommando för att hämta OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="5e307-216">Use the following command to download OpenJDK.</span></span>   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* <span data-ttu-id="5e307-217">Att skapa en katalog innehåller JDK-filer:</span><span class="sxs-lookup"><span data-stu-id="5e307-217">To create a directory to contain the JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="5e307-218">Extrahera JDK-filerna i katalogen/usr/lib/jvm:</span><span class="sxs-lookup"><span data-stu-id="5e307-218">To extract the JDK files into the /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a><span data-ttu-id="5e307-219">Installera Oracle JDK</span><span class="sxs-lookup"><span data-stu-id="5e307-219">Install Oracle JDK</span></span>


<span data-ttu-id="5e307-220">Använda följande kommando för att hämta Oracle JDK från Oracle-webbplats.</span><span class="sxs-lookup"><span data-stu-id="5e307-220">Use the following command to download Oracle JDK from the Oracle website.</span></span>  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* <span data-ttu-id="5e307-221">Att skapa en katalog innehåller JDK-filer:</span><span class="sxs-lookup"><span data-stu-id="5e307-221">To create a directory to contain the JDK files:</span></span>  

        sudo mkdir /usr/lib/jvm  
* <span data-ttu-id="5e307-222">Extrahera JDK-filerna i katalogen/usr/lib/jvm:</span><span class="sxs-lookup"><span data-stu-id="5e307-222">To extract the JDK files into the /usr/lib/jvm/ directory:</span></span>  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* <span data-ttu-id="5e307-223">Att ställa in Oracle JDK som standard Java virtual machine:</span><span class="sxs-lookup"><span data-stu-id="5e307-223">To set Oracle JDK as the default Java virtual machine:</span></span>  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a><span data-ttu-id="5e307-224">Bekräfta att Java-installationen har lyckats</span><span class="sxs-lookup"><span data-stu-id="5e307-224">Confirm that Java installation is successful</span></span>
<span data-ttu-id="5e307-225">Du kan använda ett kommando som följande för att testa om Java runtime environment har installerats på rätt sätt:</span><span class="sxs-lookup"><span data-stu-id="5e307-225">You can use a command like the following to test if the Java runtime environment is installed correctly:</span></span>  

    java -version  

<span data-ttu-id="5e307-226">Om du har installerat OpenJDK, bör du se följande meddelande: ![lyckade OpenJDK installationen visas][14]</span><span class="sxs-lookup"><span data-stu-id="5e307-226">If you installed OpenJDK, you should see a message like the following: ![Successful OpenJDK installation message][14]</span></span>

<span data-ttu-id="5e307-227">Om du har installerat Oracle JDK, bör du se följande meddelande: ![lyckade Oracle JDK installationen visas][15]</span><span class="sxs-lookup"><span data-stu-id="5e307-227">If you installed Oracle JDK, you should see a message like the following: ![Successful Oracle JDK installation message][15]</span></span>

### <a name="install-tomcat7"></a><span data-ttu-id="5e307-228">Installera Tomcat7</span><span class="sxs-lookup"><span data-stu-id="5e307-228">Install Tomcat7</span></span>
<span data-ttu-id="5e307-229">Använd följande kommando för att installera Tomcat7.</span><span class="sxs-lookup"><span data-stu-id="5e307-229">Use the following command to install Tomcat7.</span></span>  

    sudo apt-get install tomcat7  

<span data-ttu-id="5e307-230">Om du inte använder Tomcat7, använder du lämplig variation av det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="5e307-230">If you are not using Tomcat7, use the appropriate variation of this command.</span></span>  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a><span data-ttu-id="5e307-231">Bekräfta att Tomcat7 installationen har lyckats</span><span class="sxs-lookup"><span data-stu-id="5e307-231">Confirm that Tomcat7 installation is successful</span></span>
<span data-ttu-id="5e307-232">Bläddra till Tomcat-serverns DNS-namn om du vill kontrollera om Tomcat7 har installerats.</span><span class="sxs-lookup"><span data-stu-id="5e307-232">To check if Tomcat7 is successfully installed, browse to your Tomcat server’s DNS name.</span></span> <span data-ttu-id="5e307-233">Exempel-URL är http://tomcatexample.cloudapp.net/ i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5e307-233">In this article, the example URL is http://tomcatexample.cloudapp.net/.</span></span> <span data-ttu-id="5e307-234">Om du ser ett meddelande som följande Tomcat7 är korrekt installerad.</span><span class="sxs-lookup"><span data-stu-id="5e307-234">If you see a message like the following, Tomcat7 is installed correctly.</span></span>
<span data-ttu-id="5e307-235">![Lyckad Tomcat7 installationen visas][16]</span><span class="sxs-lookup"><span data-stu-id="5e307-235">![Successful Tomcat7 installation message][16]</span></span>

### <a name="install-other-tomcat7-components"></a><span data-ttu-id="5e307-236">Installera komponenter för andra Tomcat7</span><span class="sxs-lookup"><span data-stu-id="5e307-236">Install other Tomcat7 components</span></span>
<span data-ttu-id="5e307-237">Det finns andra valfria Tomcat-komponenter som du kan installera.</span><span class="sxs-lookup"><span data-stu-id="5e307-237">There are other optional Tomcat components that you can install.</span></span>  

<span data-ttu-id="5e307-238">Använd den **sudo lgh cache-sökning tomcat7** kommandot för att se alla tillgängliga komponenter.</span><span class="sxs-lookup"><span data-stu-id="5e307-238">Use the **sudo apt-cache search tomcat7** command to see all of the available components.</span></span> <span data-ttu-id="5e307-239">Använd följande kommandon för att installera vissa användbara komponenter.</span><span class="sxs-lookup"><span data-stu-id="5e307-239">Use the following commands to install some useful components.</span></span>  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a><span data-ttu-id="5e307-240">Steg 4: Konfigurera Tomcat7</span><span class="sxs-lookup"><span data-stu-id="5e307-240">Phase 4: Configure Tomcat7</span></span>
<span data-ttu-id="5e307-241">I det här steget kan du administrera Tomcat.</span><span class="sxs-lookup"><span data-stu-id="5e307-241">In this phase, you administer Tomcat.</span></span>

### <a name="start-and-stop-tomcat7"></a><span data-ttu-id="5e307-242">Starta och stoppa Tomcat7</span><span class="sxs-lookup"><span data-stu-id="5e307-242">Start and stop Tomcat7</span></span>
<span data-ttu-id="5e307-243">Tomcat7 servern startar automatiskt när du installerar den.</span><span class="sxs-lookup"><span data-stu-id="5e307-243">The Tomcat7 server automatically starts when you install it.</span></span> <span data-ttu-id="5e307-244">Du kan också starta den med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5e307-244">You can also start it with the following command:</span></span>   

    sudo /etc/init.d/tomcat7 start

<span data-ttu-id="5e307-245">Så här stoppar Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="5e307-245">To stop Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 stop

<span data-ttu-id="5e307-246">Visa status för Tomcat7:</span><span class="sxs-lookup"><span data-stu-id="5e307-246">To view the status of Tomcat7:</span></span>

    sudo /etc/init.d/tomcat7 status

<span data-ttu-id="5e307-247">Starta om Tomcat-tjänster:</span><span class="sxs-lookup"><span data-stu-id="5e307-247">To restart Tomcat services:</span></span> 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a><span data-ttu-id="5e307-248">Tomcat7 administration</span><span class="sxs-lookup"><span data-stu-id="5e307-248">Tomcat7 administration</span></span>
<span data-ttu-id="5e307-249">Du kan redigera konfigurationsfilen Tomcat användare att konfigurera dina administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="5e307-249">You can edit the Tomcat user configuration file to set up your admin credentials.</span></span> <span data-ttu-id="5e307-250">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5e307-250">Use the following command:</span></span>  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

<span data-ttu-id="5e307-251">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="5e307-251">Here is an example:</span></span>  
![Skärmbild som visar sudo vi kommandoutdata][17]  

> [!NOTE]
> <span data-ttu-id="5e307-253">Skapa ett starkt lösenord för administratörsanvändarnamnet.</span><span class="sxs-lookup"><span data-stu-id="5e307-253">Create a strong password for the admin username.</span></span>  

<span data-ttu-id="5e307-254">När du redigerar filen, bör du starta om Tomcat7 tjänster med följande kommando för att säkerställa att ändringarna börjar gälla:</span><span class="sxs-lookup"><span data-stu-id="5e307-254">After editing this file, you should restart Tomcat7 services with the following command to ensure that the changes take effect:</span></span>  

    sudo /etc/init.d/tomcat7 restart  

<span data-ttu-id="5e307-255">Öppna din webbläsare och ange **http://<your tomcat server DNS name>/manager/html** som URL.</span><span class="sxs-lookup"><span data-stu-id="5e307-255">Open your browser, and enter **http://<your tomcat server DNS name>/manager/html** as the URL.</span></span> <span data-ttu-id="5e307-256">Till exempel i den här artikeln är URL: en http://tomcatexample.cloudapp.net/manager/html.</span><span class="sxs-lookup"><span data-stu-id="5e307-256">For the example in this article, the URL is http://tomcatexample.cloudapp.net/manager/html.</span></span>  

<span data-ttu-id="5e307-257">Efter anslutning, bör du se något som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="5e307-257">After connecting, you should see something similar to the following:</span></span>  
![Skärmbild av Tomcat Web Application Manager][18]

## <a name="common-issues"></a><span data-ttu-id="5e307-259">Vanliga problem</span><span class="sxs-lookup"><span data-stu-id="5e307-259">Common issues</span></span>
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a><span data-ttu-id="5e307-260">Det går inte att komma åt den virtuella datorn med Tomcat och Moodle från Internet</span><span class="sxs-lookup"><span data-stu-id="5e307-260">Can't access the virtual machine with Tomcat and Moodle from the Internet</span></span>
#### <a name="symptom"></a><span data-ttu-id="5e307-261">Symtom</span><span class="sxs-lookup"><span data-stu-id="5e307-261">Symptom</span></span>  
  <span data-ttu-id="5e307-262">Tomcat körs men du kan inte se sidan Tomcat standard med din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="5e307-262">Tomcat is running but you can’t see the Tomcat default page with your browser.</span></span>
#### <a name="possible-root-cause"></a><span data-ttu-id="5e307-263">Möjliga underliggande orsaker</span><span class="sxs-lookup"><span data-stu-id="5e307-263">Possible root cause</span></span>   

  * <span data-ttu-id="5e307-264">Tomcat lyssningsporten är inte samma som den privata porten för den virtuella datorns slutpunkten för Tomcat-trafik.</span><span class="sxs-lookup"><span data-stu-id="5e307-264">The Tomcat listen port is not the same as the private port of your virtual machine's endpoint for Tomcat traffic.</span></span>  

     <span data-ttu-id="5e307-265">Kontrollera offentliga porten och privat port endpoint-inställningar och kontrollera att den privata porten är samma som Tomcat lyssningsport.</span><span class="sxs-lookup"><span data-stu-id="5e307-265">Check your public port and private port endpoint settings and make sure the private port is the same as the Tomcat listen port.</span></span> <span data-ttu-id="5e307-266">Se ”fas 1: skapa en avbildning” i den här artikeln för instruktioner om hur du konfigurerar slutpunkter för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-266">See "Phase 1: Create an image" section of this article for instructions on configuring endpoints for your virtual machine.</span></span>  

     <span data-ttu-id="5e307-267">Öppna /etc/httpd/conf/httpd.conf (Red Hat-version) eller /etc/tomcat7/server.xml (Debian-version) för att fastställa lyssningsporten för Tomcat.</span><span class="sxs-lookup"><span data-stu-id="5e307-267">To determine the Tomcat listen port, open /etc/httpd/conf/httpd.conf (Red Hat release), or /etc/tomcat7/server.xml (Debian release).</span></span> <span data-ttu-id="5e307-268">Som standard är Tomcat lyssna port 8080.</span><span class="sxs-lookup"><span data-stu-id="5e307-268">By default, the Tomcat listen port is 8080.</span></span> <span data-ttu-id="5e307-269">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="5e307-269">Here is an example:</span></span>  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     <span data-ttu-id="5e307-270">Om du använder en virtuell dator som Debian och Ubuntu och du vill ändra den standard port för Tomcat lyssna (till exempel 8081) kan öppna du också porten för operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="5e307-270">If you are using a virtual machine like Debian or Ubuntu and you want to change the default port of Tomcat Listen (for example 8081), you should also open the port for the operating system.</span></span> <span data-ttu-id="5e307-271">Först öppna profilen:</span><span class="sxs-lookup"><span data-stu-id="5e307-271">First, open the profile:</span></span>  

        sudo vi /etc/default/tomcat7  

     <span data-ttu-id="5e307-272">Sedan Avkommentera den sista raden och ändra ”Nej” till ”Ja”.</span><span class="sxs-lookup"><span data-stu-id="5e307-272">Then uncomment the last line and change “no” to “yes”.</span></span>  

        AUTHBIND=yes
  2. <span data-ttu-id="5e307-273">Brandväggen har inaktiverat lyssna port för Tomcat.</span><span class="sxs-lookup"><span data-stu-id="5e307-273">The firewall has disabled the listen port of Tomcat.</span></span>

     <span data-ttu-id="5e307-274">Du kan bara se sidan Tomcat från den lokala värden.</span><span class="sxs-lookup"><span data-stu-id="5e307-274">You can only see the Tomcat default page from the local host.</span></span> <span data-ttu-id="5e307-275">Problemet är sannolikt att den port som är öppna på Tomcat, blockeras av brandväggen.</span><span class="sxs-lookup"><span data-stu-id="5e307-275">The problem is most likely that the port, which is listened to by Tomcat, is blocked by the firewall.</span></span> <span data-ttu-id="5e307-276">Du kan använda verktyget w3m för att söka på webbsidan.</span><span class="sxs-lookup"><span data-stu-id="5e307-276">You can use the w3m tool to browse the webpage.</span></span> <span data-ttu-id="5e307-277">Följande kommandon installera w3m och gå till sidan med Tomcat:</span><span class="sxs-lookup"><span data-stu-id="5e307-277">The following commands install w3m and browse to the Tomcat default page:</span></span>  


        <span data-ttu-id="5e307-278">sudo yum installera w3m w3m-img</span><span class="sxs-lookup"><span data-stu-id="5e307-278">sudo yum  install w3m w3m-img</span></span>


        <span data-ttu-id="5e307-279">w3m http://localhost: 8080</span><span class="sxs-lookup"><span data-stu-id="5e307-279">w3m http://localhost:8080</span></span>  
#### <a name="solution"></a><span data-ttu-id="5e307-280">Lösning</span><span class="sxs-lookup"><span data-stu-id="5e307-280">Solution</span></span>

  * <span data-ttu-id="5e307-281">Om Tomcat lyssna porten är inte samma som den privata porten för slutpunkten för trafik till den virtuella datorn, behöver du ändra den privata porten för att vara samma som Tomcat lyssningsport.</span><span class="sxs-lookup"><span data-stu-id="5e307-281">If the Tomcat listen port is not the same as the private port of the endpoint for traffic to the virtual machine, you need change the private port to be the same as the Tomcat listen port.</span></span>   
  2. <span data-ttu-id="5e307-282">Om problemet orsakas av brandvägg/iptables, lägger du till följande rader /etc/sysconfig/iptables.</span><span class="sxs-lookup"><span data-stu-id="5e307-282">If the issue is caused by firewall/iptables, add the following lines to /etc/sysconfig/iptables.</span></span> <span data-ttu-id="5e307-283">Den andra raden krävs endast för https-trafik:</span><span class="sxs-lookup"><span data-stu-id="5e307-283">The second line is only needed for https traffic:</span></span>  

      <span data-ttu-id="5e307-284">-A -p tcp -m tcp--datorer 80 -j ACCEPTERA indata</span><span class="sxs-lookup"><span data-stu-id="5e307-284">-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT</span></span>

      <span data-ttu-id="5e307-285">-A -p tcp -m tcp--datorer 443 -j ACCEPTERA indata</span><span class="sxs-lookup"><span data-stu-id="5e307-285">-A INPUT -p tcp -m tcp --dport 443 -j ACCEPT</span></span>  

     > [!IMPORTANT]
     > <span data-ttu-id="5e307-286">Kontrollera att tidigare raderna placeras ovanför de rader som skulle globalt att begränsa åtkomsten till exempel följande: - A indata -j AVVISA--avvisa-med icmp värden förbjuden</span><span class="sxs-lookup"><span data-stu-id="5e307-286">Make sure the previous lines are positioned above any lines that would globally restrict access, such as the following: -A INPUT -j REJECT --reject-with icmp-host-prohibited</span></span>



<span data-ttu-id="5e307-287">Kör följande kommando för att läsa in iptables:</span><span class="sxs-lookup"><span data-stu-id="5e307-287">To reload the iptables, run the following command:</span></span>

    service iptables restart

<span data-ttu-id="5e307-288">Det här har testats på CentOS 6.3.</span><span class="sxs-lookup"><span data-stu-id="5e307-288">This has been tested on CentOS 6.3.</span></span>

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a><span data-ttu-id="5e307-289">Åtkomst nekad när du överför projektfilerna till /var/lib/tomcat7/webapps /</span><span class="sxs-lookup"><span data-stu-id="5e307-289">Permission denied when you upload project files to /var/lib/tomcat7/webapps/</span></span>
#### <a name="symptom"></a><span data-ttu-id="5e307-290">Symtom</span><span class="sxs-lookup"><span data-stu-id="5e307-290">Symptom</span></span>
  <span data-ttu-id="5e307-291">När du använder en SFTP-klient (till exempel FileZilla) att ansluta till den virtuella datorn och navigera till /var/lib/tomcat7/webapps/publicera webbplatsen kan få du ett felmeddelande liknar följande:</span><span class="sxs-lookup"><span data-stu-id="5e307-291">When you use an SFTP client (such as FileZilla) to connect to your virtual machine and navigate to /var/lib/tomcat7/webapps/ to publish your site, you get an error message similar to the following:</span></span>  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a><span data-ttu-id="5e307-292">Möjliga underliggande orsaker</span><span class="sxs-lookup"><span data-stu-id="5e307-292">Possible root cause</span></span>
  <span data-ttu-id="5e307-293">Du har inte åtkomstbehörighet till mappen /var/lib/tomcat7/webapps.</span><span class="sxs-lookup"><span data-stu-id="5e307-293">You have no permissions to access the /var/lib/tomcat7/webapps folder.</span></span>  
#### <a name="solution"></a><span data-ttu-id="5e307-294">Lösning</span><span class="sxs-lookup"><span data-stu-id="5e307-294">Solution</span></span>  
  <span data-ttu-id="5e307-295">Du behöver få behörighet från rotkontot.</span><span class="sxs-lookup"><span data-stu-id="5e307-295">You need to get permission from the root account.</span></span> <span data-ttu-id="5e307-296">Du kan ändra ägarskap för mappen från roten för användarnamnet som du använde när du har etablerat datorn.</span><span class="sxs-lookup"><span data-stu-id="5e307-296">You can change the ownership of that folder from root to the username you used when you provisioned the machine.</span></span> <span data-ttu-id="5e307-297">Här är ett exempel med azureuser kontonamn:</span><span class="sxs-lookup"><span data-stu-id="5e307-297">Here is an example with the azureuser account name:</span></span>  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  <span data-ttu-id="5e307-298">Använd alternativet -R för att tillämpa behörigheter för alla filer i en katalog för.</span><span class="sxs-lookup"><span data-stu-id="5e307-298">Use the -R option to apply the permissions for all files inside of a directory too.</span></span>  

  <span data-ttu-id="5e307-299">Det här kommandot fungerar även för kataloger.</span><span class="sxs-lookup"><span data-stu-id="5e307-299">This command also works for directories.</span></span> <span data-ttu-id="5e307-300">Alternativet -R ändrar behörigheter för alla filer och kataloger i katalogen.</span><span class="sxs-lookup"><span data-stu-id="5e307-300">The -R option changes the permissions for all files and directories inside the directory.</span></span> <span data-ttu-id="5e307-301">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="5e307-301">Here is an example:</span></span>  

     sudo chown -R username:group directory  

  <span data-ttu-id="5e307-302">Det här kommandot ändrar ägare (både användare och grupper) för alla filer och mappar som finns i katalogen.</span><span class="sxs-lookup"><span data-stu-id="5e307-302">This command changes ownership (both user and group) for all files and directories that are inside the directory.</span></span>  

  <span data-ttu-id="5e307-303">Följande kommando ändrar bara behörigheten för katalogen som mappen.</span><span class="sxs-lookup"><span data-stu-id="5e307-303">The following command only changes the permission of the folder directory.</span></span> <span data-ttu-id="5e307-304">Filer och mappar i katalogen ändras inte.</span><span class="sxs-lookup"><span data-stu-id="5e307-304">The files and folders inside the directory are not changed.</span></span>  

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
