---
title: "aaaUse PowerShell tooCreate en VM med ett enhetligt läge rapportserver | Microsoft Docs"
description: "Det här avsnittet beskriver och vägleder dig genom hello distributionen och konfigurationen av en rapportserver för SQL Server Reporting Services enhetligt läge i en virtuell dator i Azure. "
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: e7791199c87dff106132f1535da12de40a8dbc9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-vm-with-a-native-mode-report-server"></a><span data-ttu-id="639a4-103">Använd PowerShell tooCreate en Azure VM med ett enhetligt läge Report Server</span><span class="sxs-lookup"><span data-stu-id="639a4-103">Use PowerShell tooCreate an Azure VM With a Native Mode Report Server</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="639a4-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="639a4-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="639a4-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="639a4-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="639a4-106">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="639a4-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="639a4-107">Det här avsnittet beskriver och vägleder dig genom hello distributionen och konfigurationen av en rapportserver för SQL Server Reporting Services enhetligt läge i en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="639a4-107">This topic describes and walks you through hello deployment and configuration of a SQL Server Reporting Services native mode report server in an Azure Virtual Machine.</span></span> <span data-ttu-id="639a4-108">hello stegen i det här dokumentet används en kombination av manuella steg toocreate hello virtuell dator och en Windows PowerShell-skript tooconfigure Reporting Services på hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-108">hello steps in this document use a combination of manual steps toocreate hello virtual machine and a Windows PowerShell script tooconfigure Reporting Services on hello VM.</span></span> <span data-ttu-id="639a4-109">hello konfigurationsskript innehåller öppnar en brandväggsport för HTTP eller HTTPs.</span><span class="sxs-lookup"><span data-stu-id="639a4-109">hello configuration script includes opening a firewall port for HTTP or HTTPs.</span></span>

> [!NOTE]
> <span data-ttu-id="639a4-110">Om du inte behöver **HTTPS** på hello rapportserver **hoppa över steg 2**.</span><span class="sxs-lookup"><span data-stu-id="639a4-110">If you do not require **HTTPS** on hello report server, **skip step 2**.</span></span>
> 
> <span data-ttu-id="639a4-111">När du har skapat hello VM i steg 1, gå toohello avsnittet Använd skriptet tooconfigure hello report server- och HTTP.</span><span class="sxs-lookup"><span data-stu-id="639a4-111">After creating hello VM in step 1, go toohello section Use script tooconfigure hello report server and HTTP.</span></span> <span data-ttu-id="639a4-112">När du har kört skriptet hello är hello rapportservern klar toouse.</span><span class="sxs-lookup"><span data-stu-id="639a4-112">After you run hello script, hello report server is ready toouse.</span></span>

## <a name="prerequisites-and-assumptions"></a><span data-ttu-id="639a4-113">Krav och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="639a4-113">Prerequisites and Assumptions</span></span>
* <span data-ttu-id="639a4-114">**Azure-prenumeration**: Kontrollera hello antal kärnor som är tillgängliga i Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="639a4-114">**Azure Subscription**: Verify hello number of cores available in your Azure Subscription.</span></span> <span data-ttu-id="639a4-115">Om du skapar hello rekommenderas VM-storlek för **A3**, behöver du **4** tillgängliga kärnor.</span><span class="sxs-lookup"><span data-stu-id="639a4-115">If you create hello recommended VM size of **A3**, you need **4** available cores.</span></span> <span data-ttu-id="639a4-116">Om du använder en VM-storlek för **A2**, behöver du **2** tillgängliga kärnor.</span><span class="sxs-lookup"><span data-stu-id="639a4-116">If you use a VM size of **A2**, you need **2** available cores.</span></span>
  
  * <span data-ttu-id="639a4-117">tooverify hello core gränsen för prenumerationen i hello klassiska Azure-portalen, klicka på inställningar i hello till vänster och sedan klicka på användning i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="639a4-117">tooverify hello core limit of your subscription, in hello Azure classic portal, click SETTINGS in hello left pane and then Click USAGE in hello top menu.</span></span>
  * <span data-ttu-id="639a4-118">tooincrease hello kärnkvot, kontakta [Azure-supporten](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="639a4-118">tooincrease hello core quota, contact [Azure Support](https://azure.microsoft.com/support/options/).</span></span> <span data-ttu-id="639a4-119">Virtuell Storleksinformation finns i [storlekar för virtuella datorer för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="639a4-119">For VM size information, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="639a4-120">**Windows PowerShell-skript**: hello avsnittet förutsätter att du har grundläggande kunskaper om Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="639a4-120">**Windows PowerShell Scripting**: hello topic assumes that you have a basic working knowledge of Windows PowerShell.</span></span> <span data-ttu-id="639a4-121">Mer information om hur du använder Windows PowerShell finns i hello följande:</span><span class="sxs-lookup"><span data-stu-id="639a4-121">For more information about using Windows PowerShell, see hello following:</span></span>
  
  * [<span data-ttu-id="639a4-122">Starta Windows PowerShell på Windows Server</span><span class="sxs-lookup"><span data-stu-id="639a4-122">Starting Windows PowerShell on Windows Server</span></span>](https://technet.microsoft.com/library/hh847814.aspx)
  * [<span data-ttu-id="639a4-123">Komma igång med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="639a4-123">Getting Started with Windows PowerShell</span></span>](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a><span data-ttu-id="639a4-124">Steg 1: Etablera en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="639a4-124">Step 1: Provision an Azure Virtual Machine</span></span>
1. <span data-ttu-id="639a4-125">Bläddra toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="639a4-125">Browse toohello Azure classic portal.</span></span>
2. <span data-ttu-id="639a4-126">Klicka på **virtuella datorer** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="639a4-126">Click **Virtual Machines** in hello left pane.</span></span>
   
    ![Microsoft azure virtuella datorer](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. <span data-ttu-id="639a4-128">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="639a4-128">Click **New**.</span></span>
   
    ![Knappen Nytt](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. <span data-ttu-id="639a4-130">Klicka på **från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="639a4-130">Click **From Gallery**.</span></span>
   
    ![ny virtuell dator från galleriet](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. <span data-ttu-id="639a4-132">Klicka på **SQL Server 2014 RTM Standard-Windows Server 2012 R2** och klicka sedan på hello pilen toocontinue.</span><span class="sxs-lookup"><span data-stu-id="639a4-132">Click **SQL Server 2014 RTM Standard – Windows Server 2012 R2** and then click hello arrow toocontinue.</span></span>
   
    ![nästa](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    <span data-ttu-id="639a4-134">Om du behöver hello Reporting Services datadrivna prenumerationer funktionen väljer **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**.</span><span class="sxs-lookup"><span data-stu-id="639a4-134">If you need hello Reporting Services data driven subscriptions feature, choose **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**.</span></span> <span data-ttu-id="639a4-135">Mer information om SQL Server-utgåvorna och funktioner som stöds finns i [funktioner som stöds av hello utgåvor av SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span><span class="sxs-lookup"><span data-stu-id="639a4-135">For more information on SQL Server editions and feature support, see [Features Supported by hello Editions of SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span></span>
6. <span data-ttu-id="639a4-136">På hello **konfiguration av virtuell dator** , redigera hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="639a4-136">On hello **Virtual machine configuration** page, edit hello following fields:</span></span>
   
   * <span data-ttu-id="639a4-137">Om det finns fler än en **VERSION LANSERINGSDATUMET**, Välj hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="639a4-137">If there is more than one **VERSION RELEASE DATE**, select hello most recent version.</span></span>
   * <span data-ttu-id="639a4-138">**Namn på virtuell dator**: hello datornamnet används också på konfigurationssidan för hello nästa som hello standard Cloud Service DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="639a4-138">**Virtual Machine Name**: hello machine name is also used on hello next configuration page as hello default Cloud Service DNS name.</span></span> <span data-ttu-id="639a4-139">hello DNS-namn måste vara unikt inom hello Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="639a4-139">hello DNS name must be unique across hello Azure service.</span></span> <span data-ttu-id="639a4-140">Överväg att konfigurera hello VM med ett datornamn som beskriver vilka hello VM används för.</span><span class="sxs-lookup"><span data-stu-id="639a4-140">Consider configuring hello VM with a computer name that describes what hello VM is used for.</span></span> <span data-ttu-id="639a4-141">Till exempel ssrsnativecloud.</span><span class="sxs-lookup"><span data-stu-id="639a4-141">For example ssrsnativecloud.</span></span>
   * <span data-ttu-id="639a4-142">**Nivån**: Standard</span><span class="sxs-lookup"><span data-stu-id="639a4-142">**Tier**: Standard</span></span>
   * <span data-ttu-id="639a4-143">**Storlek: A3** rekommenderas hello VM-storlek för SQL Server-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="639a4-143">**Size:A3** is hello recommended VM size for SQL Server workloads.</span></span> <span data-ttu-id="639a4-144">Om en virtuell dator används endast som en rapportserver, räcker en VM-storlek för A2 om hello rapportservern påträffar en stor belastning.</span><span class="sxs-lookup"><span data-stu-id="639a4-144">If a VM is only used as a report server, a VM size of A2 is sufficient unless hello report server experiences a large workload.</span></span> <span data-ttu-id="639a4-145">VM information om priser finns i [prissättning för Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="639a4-145">For VM pricing information, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>
   * <span data-ttu-id="639a4-146">**Nytt användarnamn**: hello namn skapas som en administratör på hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-146">**New User Name**: hello name you provide is created as an administrator on hello VM.</span></span>
   * <span data-ttu-id="639a4-147">**Nytt lösenord** och **Bekräfta**.</span><span class="sxs-lookup"><span data-stu-id="639a4-147">**New Password** and **confirm**.</span></span> <span data-ttu-id="639a4-148">Detta lösenord används för hello nya administratörskontot och det rekommenderas att du använder ett starkt lösenord.</span><span class="sxs-lookup"><span data-stu-id="639a4-148">This password is used for hello new administrator account and it is recommended you use a strong password.</span></span>
   * <span data-ttu-id="639a4-149">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="639a4-149">Click **Next**.</span></span> <span data-ttu-id="639a4-150">![Nästa](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span><span class="sxs-lookup"><span data-stu-id="639a4-150">![next](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span></span>
7. <span data-ttu-id="639a4-151">På nästa sida hello redigera hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="639a4-151">On hello next page, edit hello following fields:</span></span>
   
   * <span data-ttu-id="639a4-152">**Molntjänsten**: Välj **skapa en ny molntjänst**.</span><span class="sxs-lookup"><span data-stu-id="639a4-152">**Cloud Service**: select **Create a new Cloud Service**.</span></span>
   * <span data-ttu-id="639a4-153">**Cloud Service DNS-namnet**: är hello offentliga DNS-namnet på hello tjänst i molnet som är associerad med hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-153">**Cloud Service DNS Name**: This is hello public DNS name of hello Cloud Service that is associated with hello VM.</span></span> <span data-ttu-id="639a4-154">hello standardnamnet är hello namnet du angav i hello VM-namn.</span><span class="sxs-lookup"><span data-stu-id="639a4-154">hello default name is hello name you typed in for hello VM name.</span></span> <span data-ttu-id="639a4-155">Om i senare steg i hello avsnittet skapar du ett betrott certifikat för SSL och sedan hello DNS-namn används för hello värdet för hello ”**utfärdat till**” hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="639a4-155">If in later steps of hello topic, you create a trusted SSL certificate and then hello DNS name is used for hello value of hello “**Issued to**” of hello certificate.</span></span>
   * <span data-ttu-id="639a4-156">**Region/tillhörighet grupp/virtuellt nätverk**: Välj hello region närmaste tooyour slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="639a4-156">**Region/Affinity Group/Virtual Network**: Choose hello region closest tooyour end users.</span></span>
   * <span data-ttu-id="639a4-157">**Lagringskontot**: Använd ett lagringskonto som skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="639a4-157">**Storage Account**: Use an automatically generated storage account.</span></span>
   * <span data-ttu-id="639a4-158">**Tillgänglighetsuppsättningen**: Ingen.</span><span class="sxs-lookup"><span data-stu-id="639a4-158">**Availability Set**: None.</span></span>
   * <span data-ttu-id="639a4-159">**SLUTPUNKTER** Behåll hello **fjärrskrivbord** och **PowerShell** slutpunkter och Lägg sedan till en HTTP- eller HTTPS-slutpunkt, beroende på din miljö.</span><span class="sxs-lookup"><span data-stu-id="639a4-159">**ENDPOINTS** Keep hello **Remote Desktop** and **PowerShell** endpoints and then add either an HTTP or HTTPS endpoint, depending on your environment.</span></span>
     
     * <span data-ttu-id="639a4-160">**HTTP**: hello standard offentliga och privata portar är **80**.</span><span class="sxs-lookup"><span data-stu-id="639a4-160">**HTTP**: hello default public and private ports are **80**.</span></span> <span data-ttu-id="639a4-161">Observera att om du använder ett privat port än 80, ändra **$HTTPport = 80** i hello http-skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-161">Note that if you use a private port other than 80, modify **$HTTPport = 80** in hello http script.</span></span>
     * <span data-ttu-id="639a4-162">**HTTPS**: hello standard offentliga och privata portar är **443**.</span><span class="sxs-lookup"><span data-stu-id="639a4-162">**HTTPS**: hello default public and private ports are **443**.</span></span> <span data-ttu-id="639a4-163">Av säkerhetsskäl är toochange hello privat port och konfigurera din brandvägg och hello report server toouse hello privat port.</span><span class="sxs-lookup"><span data-stu-id="639a4-163">A security best practice is toochange hello private port and configure your firewall and hello report server toouse hello private port.</span></span> <span data-ttu-id="639a4-164">Läs mer på slutpunkter [hur tooSet in kommunikation med en virtuell dator](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="639a4-164">For more information on endpoints, see [How tooSet Up Communication with a Virtual Machine](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="639a4-165">Observera att om du använder en annan port än 443, ändra hello parametern **$HTTPsport = 443** i hello HTTPS-skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-165">Note that if you use a port other than 443, change hello parameter **$HTTPsport = 443** in hello HTTPS script.</span></span>
   * <span data-ttu-id="639a4-166">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="639a4-166">Click next .</span></span> ![nästa](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. <span data-ttu-id="639a4-168">På hello sista sidan i guiden hello, behåller du standardvärdet för hello **installera hello VM agent** valda.</span><span class="sxs-lookup"><span data-stu-id="639a4-168">On hello last page of hello wizard, keep hello default **Install hello VM agent** selected.</span></span> <span data-ttu-id="639a4-169">hello stegen i det här avsnittet gör du genom att använda hello VM agent men om du planerar tookeep på den här virtuella datorn hello VM agent och tillägg tillåter du tooenhance han CM.</span><span class="sxs-lookup"><span data-stu-id="639a4-169">hello steps in this topic do not utilize hello VM agent but if you plan tookeep this VM, hello VM agent and extensions will allow you tooenhance he CM.</span></span>  <span data-ttu-id="639a4-170">Mer information om hello VM-agenten finns [VM-Agent och tillägg – del 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span><span class="sxs-lookup"><span data-stu-id="639a4-170">For more information on hello VM agent, see [VM Agent and Extensions – Part 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span></span> <span data-ttu-id="639a4-171">En hello standard tillägg installerade ad kör hello ”BGINFO”-tillägget som visar hello Virtuella skrivbordet, Systeminformation, till exempel intern IP-adress och ledigt enhet utrymme.</span><span class="sxs-lookup"><span data-stu-id="639a4-171">One of hello default extensions installed ad running is hello “BGINFO” extension that displays on hello VM desktop, system information such as internal IP and free drive space.</span></span>
9. <span data-ttu-id="639a4-172">Klicka på Slutför.</span><span class="sxs-lookup"><span data-stu-id="639a4-172">Click complete .</span></span> ![Okej](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. <span data-ttu-id="639a4-174">Hej **Status** av hello VM visas som **start (etablering)** under hello etablera processen och sedan visar som **kör** när hello VM är allokerade och redo toouse.</span><span class="sxs-lookup"><span data-stu-id="639a4-174">hello **Status** of hello VM displays as **Starting (Provisioning)** during hello provision process and then displays as **Running** when hello VM is provisioned and ready toouse.</span></span>

## <a name="step-2-create-a-server-certificate"></a><span data-ttu-id="639a4-175">Steg 2: Skapa ett servercertifikat</span><span class="sxs-lookup"><span data-stu-id="639a4-175">Step 2: Create a Server Certificate</span></span>
> [!NOTE]
> <span data-ttu-id="639a4-176">Om du inte behöver HTTPS på hello rapportserver, kan du **hoppa över steg 2** och gå toohello avsnittet **använda skriptet tooconfigure hello report server- och HTTP-**.</span><span class="sxs-lookup"><span data-stu-id="639a4-176">If you do not require HTTPS on hello report server, you can **skip step 2** and go toohello section **Use script tooconfigure hello report server and HTTP**.</span></span> <span data-ttu-id="639a4-177">Använd hello HTTP skriptet tooquickly konfigurera hello rapportservern och hello report server kommer att vara klar toouse.</span><span class="sxs-lookup"><span data-stu-id="639a4-177">Use hello HTTP script tooquickly configure hello report server and hello report server will be ready toouse.</span></span>

<span data-ttu-id="639a4-178">I ordning toouse HTTPS på hello VM behöver du ett betrott certifikat för SSL.</span><span class="sxs-lookup"><span data-stu-id="639a4-178">In order toouse HTTPS on hello VM, you need a trusted SSL certificate.</span></span> <span data-ttu-id="639a4-179">Du kan använda något av följande två metoder hello beroende på ditt scenario:</span><span class="sxs-lookup"><span data-stu-id="639a4-179">Depending on your scenario, you can use one of hello following two methods:</span></span>

* <span data-ttu-id="639a4-180">Ett giltigt SSL-certifikat utfärdat av en certifikatutfärdare (CA) och betrodd av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="639a4-180">A valid SSL certificate issued by a Certification Authority (CA) and trusted by Microsoft.</span></span> <span data-ttu-id="639a4-181">hello Certifikatutfärdarens rotcertifikat är obligatoriska toobe distribueras via hello Microsoft Root Certificate Program.</span><span class="sxs-lookup"><span data-stu-id="639a4-181">hello CA root certificates are required toobe distributed via hello Microsoft Root Certificate Program.</span></span> <span data-ttu-id="639a4-182">Mer information om det här programmet finns [Windows och Windows Phone 8 SSL Root Certificate Program (medlem CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) och [introduktion toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-182">For more information about this program, see [Windows and Windows Phone 8 SSL Root Certificate Program (Member CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) and [Introduction toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span></span>
* <span data-ttu-id="639a4-183">Ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="639a4-183">A self-signed certificate.</span></span> <span data-ttu-id="639a4-184">Självsignerade certifikat rekommenderas inte för produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="639a4-184">Self-signed certificates are not recommended for production environments.</span></span>

### <a name="toouse-a-certificate-created-by-a-trusted-certificate-authority-ca"></a><span data-ttu-id="639a4-185">toouse ett certifikat som skapas av en betrodd certifikatutfärdare (CA)</span><span class="sxs-lookup"><span data-stu-id="639a4-185">toouse a certificate created by a trusted Certificate Authority (CA)</span></span>
1. <span data-ttu-id="639a4-186">**Begära ett certifikat för hello webbplats från en certifikatutfärdare**.</span><span class="sxs-lookup"><span data-stu-id="639a4-186">**Request a server certificate for hello website from a certification authority**.</span></span> 
   
    <span data-ttu-id="639a4-187">Du kan använda antingen toogenerate en fil med certifikatbegäran (Certreq.txt) att du skickar tooa certifikatutfärdare eller toogenerate en begäran om en onlinecertifikatutfärdare hello guiden Servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="639a4-187">You can use hello Web Server Certificate Wizard either toogenerate a certificate request file (Certreq.txt) that you send tooa certification authority, or toogenerate a request for an online certification authority.</span></span> <span data-ttu-id="639a4-188">Till exempel Microsoft Certificate Services i Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="639a4-188">For example, Microsoft Certificate Services in Windows Server 2012.</span></span> <span data-ttu-id="639a4-189">Beroende på hello säkerhetsnivå identifiering erbjuds av servercertifikatet, det är flera dagar tooseveral månader för hello certification authority tooapprove din begäran och skicka en certifikatfil.</span><span class="sxs-lookup"><span data-stu-id="639a4-189">Depending on hello level of identification assurance offered by your server certificate, it is several days tooseveral months for hello certification authority tooapprove your request and send you a certificate file.</span></span> 
   
    <span data-ttu-id="639a4-190">Mer information om hur du begär finns ett servercertifikat hello följande:</span><span class="sxs-lookup"><span data-stu-id="639a4-190">For more information about requesting a server certificates, see hello following:</span></span> 
   
   * <span data-ttu-id="639a4-191">Använd [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-191">Use [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span></span>
   * <span data-ttu-id="639a4-192">Security Tools tooAdminister Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="639a4-192">Security Tools tooAdminister Windows Server 2012.</span></span>
     
     [<span data-ttu-id="639a4-193">Security Tools tooAdminister Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="639a4-193">Security Tools tooAdminister Windows Server 2012</span></span>](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > <span data-ttu-id="639a4-194">hello **utfärdat till** för hello betrodda SSL-certifikat bör vara hello samma som hello **DNS MOLNTJÄNSTNAMNET** som du använde för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="639a4-194">hello **issued to** field of hello trusted SSL certificate should be hello same as hello **Cloud Service DNS NAME** you used for hello new VM.</span></span>

2. <span data-ttu-id="639a4-195">**Installera hello servercertifikat på hello webbserver**.</span><span class="sxs-lookup"><span data-stu-id="639a4-195">**Install hello server certificate on hello Web server**.</span></span> <span data-ttu-id="639a4-196">hello webbservern är i det här fallet hello VM att värdar hello rapportservern och hello webbplatsen har skapats i senare steg när du konfigurerar Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="639a4-196">hello Web server in this case is hello VM that hosts hello report server and hello website is created in later steps when you configure Reporting Services.</span></span> <span data-ttu-id="639a4-197">Mer information om hur du installerar servercertifikatet hello på hello webbserver med hjälp av hello MMC snapin-modulen för certifikat finns [installera ett servercertifikat](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="639a4-197">For more information about installing hello server certificate on hello Web server by using hello Certificate MMC snap-in, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
    <span data-ttu-id="639a4-198">Om du vill toouse hello skript som ingår i det här avsnittet tooconfigure hello rapportservern, hello värdet för hello certifikat **tumavtrycket** krävs som en parameter för hello skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-198">If you want toouse hello script included with this topic, tooconfigure hello report server, hello value of hello certificates **Thumbprint** is required as a parameter of hello script.</span></span> <span data-ttu-id="639a4-199">Avsnittet hello nästa information om hur tooobtain hello hello certifikatets tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="639a4-199">See hello next section for details on how tooobtain hello thumbprint of hello certificate.</span></span>
3. <span data-ttu-id="639a4-200">Tilldela hello server certifikat toohello-rapportservern.</span><span class="sxs-lookup"><span data-stu-id="639a4-200">Assign hello server certificate toohello report server.</span></span> <span data-ttu-id="639a4-201">hello tilldelningen har slutförts i nästa avsnitt om hello när du konfigurerar hello rapportservern.</span><span class="sxs-lookup"><span data-stu-id="639a4-201">hello assignment is completed in hello next section when you configure hello report server.</span></span>

### <a name="toouse-hello-virtual-machines-self-signed-certificate"></a><span data-ttu-id="639a4-202">toouse hello ett självsignerat certifikat för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="639a4-202">toouse hello Virtual Machines Self-signed Certificate</span></span>
<span data-ttu-id="639a4-203">Ett självsignerat certifikat har skapats på hello VM när hello VM har etablerats.</span><span class="sxs-lookup"><span data-stu-id="639a4-203">A self-signed certificate was created on hello VM when hello VM was provisioned.</span></span> <span data-ttu-id="639a4-204">hello certifikatet har hello samma namn som hello VM DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="639a4-204">hello certificate has hello same name as hello VM DNS name.</span></span> <span data-ttu-id="639a4-205">I ordning tooavoid certifikatfel förekommer det krävs att hello-certifikatet är betrott på hello Virtuella datorn och också av alla användare av hello plats.</span><span class="sxs-lookup"><span data-stu-id="639a4-205">In order tooavoid certificate errors, it is required that hello certificate is trusted on hello VM itself, and also by all users of hello site.</span></span>

1. <span data-ttu-id="639a4-206">tootrust hello rotcertifikatutfärdaren för hello certifikatet på hello lokala VM lägga till hello certifikat toohello **betrodda rotcertifikatutfärdare**.</span><span class="sxs-lookup"><span data-stu-id="639a4-206">tootrust hello root CA of hello certificate on hello Local VM, add hello certificate toohello **Trusted Root Certification Authorities**.</span></span> <span data-ttu-id="639a4-207">hello följer en sammanfattning av hello steg som krävs.</span><span class="sxs-lookup"><span data-stu-id="639a4-207">hello following is a summary of hello steps required.</span></span> <span data-ttu-id="639a4-208">Detaljerade anvisningar om hur tootrust hello Certifikatutfärdaren finns [installera ett servercertifikat](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="639a4-208">For detailed steps on how tootrust hello CA, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
   1. <span data-ttu-id="639a4-209">Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut.</span><span class="sxs-lookup"><span data-stu-id="639a4-209">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="639a4-210">Beroende på din webbläsarkonfiguration du tillfrågas toosave en RDP-fil för att ansluta toohello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-210">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
      
       ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="639a4-212">Använd hello VM användarnamn, användarnamn och lösenord som du konfigurerade när du har skapat hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-212">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
      
       <span data-ttu-id="639a4-213">Till exempel i följande bild hello, hello VM är namnet **ssrsnativecloud** och hello användarnamnet är **testuser**.</span><span class="sxs-lookup"><span data-stu-id="639a4-213">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
      
       ![inloggningsnamnet innehåller vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. <span data-ttu-id="639a4-215">Kör mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="639a4-215">Run mmc.exe.</span></span> <span data-ttu-id="639a4-216">Mer information finns i [så här: Visa certifikat med hello MMC-snapin-modulen](https://msdn.microsoft.com/library/ms788967.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-216">For more information, see [How to: View Certificates with hello MMC Snap-in](https://msdn.microsoft.com/library/ms788967.aspx).</span></span>
   3. <span data-ttu-id="639a4-217">I hello konsolprogram **filen** menyn Lägg till hello **certifikat** snapin-modulen, Välj **datorkonto** när en fråga och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="639a4-217">In hello console application **File** menu, add hello **Certificates** snap-in, select **Computer Account** when prompted, and then click **Next**.</span></span>
   4. <span data-ttu-id="639a4-218">Välj **lokal dator** toomanage och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="639a4-218">Select **Local Computer** toomanage and then click **Finish**.</span></span>
   5. <span data-ttu-id="639a4-219">Klicka på **Ok** och expandera sedan hello **certifikat - personliga** noder och klicka sedan på **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="639a4-219">Click **Ok** and then expand hello **Certificates -Personal** nodes and then click **Certificates**.</span></span> <span data-ttu-id="639a4-220">hello certifikat efter hello DNS-namnet på hello VM och slutar med **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="639a4-220">hello certificate is named after hello DNS name of hello VM and ends with **cloudapp.net**.</span></span> <span data-ttu-id="639a4-221">Högerklicka på hello certifikatets namn och klicka på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="639a4-221">Right-click hello certificate name and click **Copy**.</span></span>
   6. <span data-ttu-id="639a4-222">Expandera hello **betrodda rotcertifikatutfärdare** nod och högerklicka sedan **certifikat** och klicka sedan på **klistra in**.</span><span class="sxs-lookup"><span data-stu-id="639a4-222">Expand hello **Trusted Root Certification Authorities** node and then right-click **Certificates** and then click **Paste**.</span></span>
   7. <span data-ttu-id="639a4-223">toovalidate, dubbla klickar du på hello certifikatnamn under **betrodda rotcertifikatutfärdare** och verifiera att det inte finns några fel och du ser ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="639a4-223">toovalidate, double click on hello certificate name under **Trusted Root Certification Authorities** and verify that there are no errors and you see your certificate.</span></span> <span data-ttu-id="639a4-224">Om du vill toouse hello HTTPS skriptet ingår i det här avsnittet tooconfigure hello rapportservern, hello värdet för hello certifikat **tumavtrycket** krävs som en parameter för hello skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-224">If you want toouse hello HTTPS script included with this topic, tooconfigure hello report server, hello value of hello certificates **Thumbprint** is required as a parameter of hello script.</span></span> <span data-ttu-id="639a4-225">**tooget hello tumavtrycksvärde**, Slutför följande hello.</span><span class="sxs-lookup"><span data-stu-id="639a4-225">**tooget hello thumbprint value**, complete hello following.</span></span> <span data-ttu-id="639a4-226">Det finns också ett PowerShell-exempel tooretrieve hello tumavtryck i avsnittet [använda skriptet tooconfigure hello report server- och HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).</span><span class="sxs-lookup"><span data-stu-id="639a4-226">There is also a PowerShell sample tooretrieve hello thumbprint in section [Use script tooconfigure hello report server and HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).</span></span>
      
      1. <span data-ttu-id="639a4-227">Dubbelklicka på hello namnet på hello certifikat, till exempel ssrsnativecloud.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="639a4-227">Double-click hello name of hello certificate, for example ssrsnativecloud.cloudapp.net.</span></span>
      2. <span data-ttu-id="639a4-228">Klicka på hello **information** fliken.</span><span class="sxs-lookup"><span data-stu-id="639a4-228">Click hello **Details** tab.</span></span>
      3. <span data-ttu-id="639a4-229">Klicka på **tumavtrycket**.</span><span class="sxs-lookup"><span data-stu-id="639a4-229">Click **Thumbprint**.</span></span> <span data-ttu-id="639a4-230">hello-värdet för hello tumavtrycket visas i Informationsfältet hello, till exempel a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.</span><span class="sxs-lookup"><span data-stu-id="639a4-230">hello value of hello thumbprint is displayed in hello details field, for example ‎a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.</span></span>
      4. <span data-ttu-id="639a4-231">Kopiera hello tumavtrycket och spara hello värdet för senare eller redigera hello skript nu.</span><span class="sxs-lookup"><span data-stu-id="639a4-231">Copy hello thumbprint and save hello value for later or edit hello script now.</span></span>
      5. <span data-ttu-id="639a4-232">(*) Ta bort hello blanksteg mellan hello värdepar innan du kör hello skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-232">(*) Before you run hello script, remove hello spaces in between hello pairs of values.</span></span> <span data-ttu-id="639a4-233">Exempel är nu hello tumavtrycket anges innan a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span><span class="sxs-lookup"><span data-stu-id="639a4-233">For example hello thumbprint noted before would now be ‎a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span></span>
      6. <span data-ttu-id="639a4-234">Tilldela hello server certifikat toohello-rapportservern.</span><span class="sxs-lookup"><span data-stu-id="639a4-234">Assign hello server certificate toohello report server.</span></span> <span data-ttu-id="639a4-235">hello tilldelningen har slutförts i nästa avsnitt om hello när du konfigurerar hello rapportservern.</span><span class="sxs-lookup"><span data-stu-id="639a4-235">hello assignment is completed in hello next section when you configure hello report server.</span></span>

<span data-ttu-id="639a4-236">Om du använder ett självsignerat SSL-certifikat matchar hello namnet på hello certifikatet redan hello värdnamn hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-236">If you are using a self-signed SSL certificate, hello name on hello certificate already matches hello hostname of hello VM.</span></span> <span data-ttu-id="639a4-237">Därför är redan registrerad globalt hello DNS för hello dator och kan nås från alla klienter.</span><span class="sxs-lookup"><span data-stu-id="639a4-237">Therefore, hello DNS of hello machine is already registered globally and can be accessed from any client.</span></span>

## <a name="step-3-configure-hello-report-server"></a><span data-ttu-id="639a4-238">Steg 3: Konfigurera hello Report Server</span><span class="sxs-lookup"><span data-stu-id="639a4-238">Step 3: Configure hello Report Server</span></span>
<span data-ttu-id="639a4-239">Det här avsnittet beskriver hur du konfigurerar hello VM som en rapportserver för Reporting Services-enhetligt läge.</span><span class="sxs-lookup"><span data-stu-id="639a4-239">This section walks you through configuring hello VM as a Reporting Services native mode report server.</span></span> <span data-ttu-id="639a4-240">Du kan använda någon av följande metoder tooconfigure hello rapportservern hello:</span><span class="sxs-lookup"><span data-stu-id="639a4-240">You can use one of hello following methods tooconfigure hello report server:</span></span>

* <span data-ttu-id="639a4-241">Använd hello skriptet tooconfigure hello report server</span><span class="sxs-lookup"><span data-stu-id="639a4-241">Use hello script tooconfigure hello report server</span></span>
* <span data-ttu-id="639a4-242">Använd Configuration Manager tooConfigure hello Report Server.</span><span class="sxs-lookup"><span data-stu-id="639a4-242">Use Configuration Manager tooConfigure hello Report Server.</span></span>

<span data-ttu-id="639a4-243">Mer detaljerad stegen i avsnittet hello [Anslut toohello virtuella datorn och starta hello Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="639a4-243">For more detailed steps, see hello section [Connect toohello Virtual Machine and Start hello Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span></span>

<span data-ttu-id="639a4-244">**Autentisering Obs:** Windows-autentisering är hello rekommenderad autentiseringsmetod och hello standardautentisering Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="639a4-244">**Authentication Note:** Windows authentication is hello recommended authentication method and it is hello default Reporting Services authentication.</span></span> <span data-ttu-id="639a4-245">Endast användare som har konfigurerats på hello VM har åtkomst till Reporting Services och tilldelade tooReporting Services-roller.</span><span class="sxs-lookup"><span data-stu-id="639a4-245">Only users that are configured on hello VM can access Reporting Services and assigned tooReporting Services roles.</span></span>

### <a name="use-script-tooconfigure-hello-report-server-and-http"></a><span data-ttu-id="639a4-246">Använd skriptet tooconfigure hello report server- och HTTP</span><span class="sxs-lookup"><span data-stu-id="639a4-246">Use script tooconfigure hello report server and HTTP</span></span>
<span data-ttu-id="639a4-247">toouse hello Windows PowerShell-skript tooconfigure hello rapportservern, fullständig hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="639a4-247">toouse hello Windows PowerShell script tooconfigure hello report server, complete hello following steps.</span></span> <span data-ttu-id="639a4-248">hello konfigurationen omfattar HTTP, HTTPS inte:</span><span class="sxs-lookup"><span data-stu-id="639a4-248">hello configuration includes HTTP, not HTTPS:</span></span>

1. <span data-ttu-id="639a4-249">Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut.</span><span class="sxs-lookup"><span data-stu-id="639a4-249">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="639a4-250">Beroende på din webbläsarkonfiguration du tillfrågas toosave en RDP-fil för att ansluta toohello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-250">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
   
    ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="639a4-252">Använd hello VM användarnamn, användarnamn och lösenord som du konfigurerade när du har skapat hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-252">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
   
    <span data-ttu-id="639a4-253">Till exempel i följande bild hello, hello VM är namnet **ssrsnativecloud** och hello användarnamnet är **testuser**.</span><span class="sxs-lookup"><span data-stu-id="639a4-253">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
   
    ![inloggningsnamnet innehåller vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="639a4-255">Öppna på hello VM, **Windows PowerShell ISE** med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="639a4-255">On hello VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="639a4-256">hello PowerShell ISE installeras som standard i Windows server 2012.</span><span class="sxs-lookup"><span data-stu-id="639a4-256">hello PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="639a4-257">Det rekommenderas att du använder hello ISE i stället för standard Windows PowerShell-fönstret så att du kan klistra in hello skript i hello ISE, ändra hello skriptet och kör sedan hello skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-257">It is recommended you use hello ISE instead of a standard Windows PowerShell window so that you can paste hello script into hello ISE, modify hello script, and then run hello script.</span></span>
3. <span data-ttu-id="639a4-258">Klicka på hello i Windows PowerShell ISE **visa** -menyn och klicka sedan på **visa Skriptfönster**.</span><span class="sxs-lookup"><span data-stu-id="639a4-258">In Windows PowerShell ISE, click hello **View** menu and then click **Show Script Pane**.</span></span>
4. <span data-ttu-id="639a4-259">Kopiera hello följande skript och klistra in hello skript i hello Skriptfönster för Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="639a4-259">Copy hello following script, and paste hello script into hello Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change hello value if you used a different port for hello private HTTP endpoint when hello VM was created.
   
        ## Set PowerShell execution policy toobe able toorun scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. <span data-ttu-id="639a4-260">Om du har skapat hello VM med en HTTP-port än 80 ändra hello parametern $HTTPport = 80.</span><span class="sxs-lookup"><span data-stu-id="639a4-260">If you created hello VM with an HTTP port other than 80, modify hello parameter $HTTPport = 80.</span></span>
6. <span data-ttu-id="639a4-261">hello skript är konfigurerad för Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="639a4-261">hello script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="639a4-262">Om du vill toorun hello skript för Reporting Services, ändra hello version del av hello sökvägen toohello namnområdet för ”v11” på hello Get-WmiObject-instruktion.</span><span class="sxs-lookup"><span data-stu-id="639a4-262">If you want toorun hello script for  Reporting Services, modify hello version portion of hello path toohello namespace too“v11”, on hello Get-WmiObject statement.</span></span>
7. <span data-ttu-id="639a4-263">Kör skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="639a4-263">Run hello script.</span></span>

<span data-ttu-id="639a4-264">**Verifieringen**: tooverify hello grundläggande report server fungerar, se hello [Kontrollera hello configuration](#verify-the-configuration) senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="639a4-264">**Validation**: tooverify that hello basic report server functionality is working, see hello [Verify hello configuration](#verify-the-configuration) section later in this topic.</span></span>

### <a name="use-script-tooconfigure-hello-report-server-and-https"></a><span data-ttu-id="639a4-265">Använd skriptet tooconfigure hello report server- och HTTPS</span><span class="sxs-lookup"><span data-stu-id="639a4-265">Use script tooconfigure hello report server and HTTPS</span></span>
<span data-ttu-id="639a4-266">toouse Windows PowerShell tooconfigure hello rapportservern, fullständig hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="639a4-266">toouse Windows PowerShell tooconfigure hello report server, complete hello following steps.</span></span> <span data-ttu-id="639a4-267">hello konfigurationen innefattar HTTPS, inte HTTP.</span><span class="sxs-lookup"><span data-stu-id="639a4-267">hello configuration includes HTTPS, not HTTP.</span></span>

1. <span data-ttu-id="639a4-268">Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut.</span><span class="sxs-lookup"><span data-stu-id="639a4-268">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="639a4-269">Beroende på din webbläsarkonfiguration du tillfrågas toosave en RDP-fil för att ansluta toohello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-269">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
   
    ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="639a4-271">Använd hello VM användarnamn, användarnamn och lösenord som du konfigurerade när du har skapat hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-271">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
   
    <span data-ttu-id="639a4-272">Till exempel i följande bild hello, hello VM är namnet **ssrsnativecloud** och hello användarnamnet är **testuser**.</span><span class="sxs-lookup"><span data-stu-id="639a4-272">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
   
    ![inloggningsnamnet innehåller vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="639a4-274">Öppna på hello VM, **Windows PowerShell ISE** med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="639a4-274">On hello VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="639a4-275">hello PowerShell ISE installeras som standard i Windows server 2012.</span><span class="sxs-lookup"><span data-stu-id="639a4-275">hello PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="639a4-276">Det rekommenderas att du använder hello ISE i stället för standard Windows PowerShell-fönstret så att du kan klistra in hello skript i hello ISE, ändra hello skriptet och kör sedan hello skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-276">It is recommended you use hello ISE instead of a standard Windows PowerShell window so that you can paste hello script into hello ISE, modify hello script, and then run hello script.</span></span>
3. <span data-ttu-id="639a4-277">tooenable köra skript, kör hello följande Windows PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="639a4-277">tooenable running scripts, run hello following Windows PowerShell command:</span></span>
   
        Set-ExecutionPolicy RemoteSigned
   
    <span data-ttu-id="639a4-278">Du kan sedan köra hello följande tooverify hello princip:</span><span class="sxs-lookup"><span data-stu-id="639a4-278">You can then run hello following tooverify hello policy:</span></span>
   
        Get-ExecutionPolicy
4. <span data-ttu-id="639a4-279">I **Windows PowerShell ISE**, klicka på hello **visa** -menyn och klicka sedan på **visa Skriptfönster**.</span><span class="sxs-lookup"><span data-stu-id="639a4-279">In **Windows PowerShell ISE**, click hello **View** menu and then click **Show Script Pane**.</span></span>
5. <span data-ttu-id="639a4-280">Kopiera hello följande skript och klistra in den i hello Skriptfönster för Windows PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="639a4-280">Copy hello following script and paste it into hello Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures hello report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when hello HTTPS endpoint was created.
   
        # You can run hello following command tooget (.cloudapp.net certificates) so you can copy hello thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # hello certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # hello certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If hello certificate is not a wildcard certificate, comment out hello following line, and enable hello full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "hello script will use $DNSNameAndPort as hello DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. <span data-ttu-id="639a4-281">Ändra hello **$certificatehash** parameter i hello skript:</span><span class="sxs-lookup"><span data-stu-id="639a4-281">Modify hello **$certificatehash** parameter in hello script:</span></span>
   
   * <span data-ttu-id="639a4-282">Det här är en **krävs** parameter.</span><span class="sxs-lookup"><span data-stu-id="639a4-282">This is a **required** parameter.</span></span> <span data-ttu-id="639a4-283">Om du inte sparade hello certifikatvärdet från hello föregående steg, med någon av följande metoder toocopy hello certifikatets hash-värde från hello certifikat-tumavtrycket hello.:</span><span class="sxs-lookup"><span data-stu-id="639a4-283">If you did not save hello certificate value from hello previous steps, use one of hello following methods toocopy hello certificate hash value from hello certificates thumbprint.:</span></span>
     
       <span data-ttu-id="639a4-284">Öppna Windows PowerShell ISE på hello VM, och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="639a4-284">On hello VM, open Windows PowerShell ISE and run hello following command:</span></span>
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       <span data-ttu-id="639a4-285">hello utdata ser liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="639a4-285">hello output will look similar toohello following.</span></span> <span data-ttu-id="639a4-286">Om hello skriptet returnerar en tom rad, hello VM inte har ett certifikat som konfigurerats i till exempel, finns i avsnittet hello [toouse hello virtuella datorer ett självsignerat certifikat](#to-use-the-virtual-machines-self-signed-certificate).</span><span class="sxs-lookup"><span data-stu-id="639a4-286">If hello script returns a blank line, hello VM does not have a certificate configured for example, see hello section [toouse hello Virtual Machines Self-signed Certificate](#to-use-the-virtual-machines-self-signed-certificate).</span></span>
     
     <span data-ttu-id="639a4-287">ELLER</span><span class="sxs-lookup"><span data-stu-id="639a4-287">OR</span></span>
   * <span data-ttu-id="639a4-288">Hello VM kör mmc.exe och Lägg sedan till hello **certifikat** snapin-modulen.</span><span class="sxs-lookup"><span data-stu-id="639a4-288">On hello VM Run mmc.exe and then add hello **Certificates** snap-in.</span></span>
   * <span data-ttu-id="639a4-289">Under hello **betrodda rotcertifikatutfärdare** nod Dubbelklicka på certifikatets namn.</span><span class="sxs-lookup"><span data-stu-id="639a4-289">Under hello **Trusted Root Certificate Authorities** node double-click your certificate name.</span></span> <span data-ttu-id="639a4-290">Om du använder hello självsignerat certifikat för hello VM hello certifikat efter hello DNS-namnet på hello VM och slutar med **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="639a4-290">If you are using hello self-signed certificate of hello VM, hello certificate is named after hello DNS name of hello VM and ends with **cloudapp.net**.</span></span>
   * <span data-ttu-id="639a4-291">Klicka på hello **information** fliken.</span><span class="sxs-lookup"><span data-stu-id="639a4-291">Click hello **Details** tab.</span></span>
   * <span data-ttu-id="639a4-292">Klicka på **tumavtrycket**.</span><span class="sxs-lookup"><span data-stu-id="639a4-292">Click **Thumbprint**.</span></span> <span data-ttu-id="639a4-293">hello-värdet för hello tumavtrycket visas i Informationsfältet hello, till exempel af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span><span class="sxs-lookup"><span data-stu-id="639a4-293">hello value of hello thumbprint is displayed in hello details field, for example af 11 60 b6 4b 28 8d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span></span>
   * <span data-ttu-id="639a4-294">**Innan du kör skriptet hello**, ta bort hello blanksteg mellan hello värdepar.</span><span class="sxs-lookup"><span data-stu-id="639a4-294">**Before you run hello script**, remove hello spaces in between hello pairs of values.</span></span> <span data-ttu-id="639a4-295">Till exempel af1160b64b288d890a8212ff6ba9c3664f319048</span><span class="sxs-lookup"><span data-stu-id="639a4-295">For example af1160b64b288d890a8212ff6ba9c3664f319048</span></span>
7. <span data-ttu-id="639a4-296">Ändra hello **$httpsport** parameter:</span><span class="sxs-lookup"><span data-stu-id="639a4-296">Modify hello **$httpsport** parameter:</span></span> 
   
   * <span data-ttu-id="639a4-297">Om du använder port 443 för HTTPS-slutpunkt för hello sedan behöver du inte tooupdate den här parametern i hello skript.</span><span class="sxs-lookup"><span data-stu-id="639a4-297">If you used port 443 for hello HTTPS endpoint, then you do not need tooupdate this parameter in hello script.</span></span> <span data-ttu-id="639a4-298">Annars använda hello portvärde som du valde när du har konfigurerat privata hello HTTPS-slutpunkt på hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-298">Otherwise use hello port value you selected when you configured hello HTTPS private endpoint on hello VM.</span></span>
8. <span data-ttu-id="639a4-299">Ändra hello **$DNSName** parameter:</span><span class="sxs-lookup"><span data-stu-id="639a4-299">Modify hello **$DNSName** parameter:</span></span> 
   
   * <span data-ttu-id="639a4-300">hello skript har konfigurerats för ett certifikat med jokertecken $DNSName = ”+”.</span><span class="sxs-lookup"><span data-stu-id="639a4-300">hello script is configured for a wild card certificate $DNSName="+".</span></span> <span data-ttu-id="639a4-301">Om du gör inga vill tooconfigure för en bindning för certifikat med jokertecken kommentera ut $DNSName = ”+” och aktivera följande rad, hello fullständig $DNSNAme referens, ## $DNSName="$server.cloudapp.net hello”.</span><span class="sxs-lookup"><span data-stu-id="639a4-301">If you do no want tooconfigure for a wildcard certificate binding, comment out $DNSName="+" and enable hello following line, hello full $DNSNAme reference, ##$DNSName="$server.cloudapp.net".</span></span>
     
       <span data-ttu-id="639a4-302">Ändra hello $DNSName värde om du inte vill toouse hello virtuella datorns DNS-namn för Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="639a4-302">Change hello $DNSName value if you do not want toouse hello virtual machine’s DNS name for Reporting Services.</span></span> <span data-ttu-id="639a4-303">Om du använder hello parametern hello certifikatet måste också använda det här namnet och du registrera hello globalt på en DNS-server.</span><span class="sxs-lookup"><span data-stu-id="639a4-303">If you use hello parameter, hello certificate must also use this name and you register hello name globally on a DNS server.</span></span>
9. <span data-ttu-id="639a4-304">hello skript är konfigurerad för Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="639a4-304">hello script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="639a4-305">Om du vill toorun hello skript för Reporting Services, ändra hello version del av hello sökvägen toohello namnområdet för ”v11” på hello Get-WmiObject-instruktion.</span><span class="sxs-lookup"><span data-stu-id="639a4-305">If you want toorun hello script for  Reporting Services, modify hello version portion of hello path toohello namespace too“v11”, on hello Get-WmiObject statement.</span></span>
10. <span data-ttu-id="639a4-306">Kör skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="639a4-306">Run hello script.</span></span>

<span data-ttu-id="639a4-307">**Verifieringen**: tooverify hello grundläggande report server fungerar, se hello [Kontrollera hello configuration](#verify-the-connection) senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="639a4-307">**Validation**: tooverify that hello basic report server functionality is working, see hello [Verify hello configuration](#verify-the-connection) section later in this topic.</span></span> <span data-ttu-id="639a4-308">tooverify hello certifikat-bindning öppna en kommandotolk med administratörsbehörighet och kör sedan följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="639a4-308">tooverify hello certificate binding open a command prompt with administrative privileges, and then run hello following command:</span></span>

    netsh http show sslcert

<span data-ttu-id="639a4-309">hello resultatet innehåller hello följande:</span><span class="sxs-lookup"><span data-stu-id="639a4-309">hello result will include hello following:</span></span>

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-tooconfigure-hello-report-server"></a><span data-ttu-id="639a4-310">Använd Configuration Manager tooConfigure hello Report Server</span><span class="sxs-lookup"><span data-stu-id="639a4-310">Use Configuration Manager tooConfigure hello Report Server</span></span>
<span data-ttu-id="639a4-311">Om du inte vill att toorun hello PowerShell-skriptet tooconfigure hello rapportservern åtgärderna hello i det här avsnittet toouse hello Reporting Services enhetligt läge configuration manager tooconfigure hello-rapportservern.</span><span class="sxs-lookup"><span data-stu-id="639a4-311">If you do not want toorun hello PowerShell script tooconfigure hello report server, follow hello steps in this section toouse hello Reporting Services native mode configuration manager tooconfigure hello report server.</span></span>

1. <span data-ttu-id="639a4-312">Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut.</span><span class="sxs-lookup"><span data-stu-id="639a4-312">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="639a4-313">Använd hello användarnamn och lösenord som du konfigurerade när du har skapat hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-313">Use hello user name and password you configured when you created hello VM.</span></span>
   
    ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. <span data-ttu-id="639a4-315">Kör Windows update och installera uppdateringar toohello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-315">Run Windows update and install updates toohello VM.</span></span> <span data-ttu-id="639a4-316">Om det krävs en omstart av hello VM, starta om hello VM och återansluta toohello VM från hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="639a4-316">If a restart of hello VM is required, restart hello VM and reconnect toohello VM from hello Azure classic portal.</span></span>
3. <span data-ttu-id="639a4-317">Hello Start-menyn på hello VM skriver **Reporting Services** och öppna **Reporting Services Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="639a4-317">From hello Start menu on hello VM, type **Reporting Services** and open **Reporting Services Configuration Manager**.</span></span>
4. <span data-ttu-id="639a4-318">Lämna hello standardvärden för **servernamn** och **rapportserverinstansen**.</span><span class="sxs-lookup"><span data-stu-id="639a4-318">Leave hello default values for **Server Name** and **Report Server Instance**.</span></span> <span data-ttu-id="639a4-319">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="639a4-319">Click **Connect**.</span></span>
5. <span data-ttu-id="639a4-320">Hello vänster klickar du på **URL för webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="639a4-320">In hello left pane, click **Web Service URL**.</span></span>
6. <span data-ttu-id="639a4-321">Som standard konfigureras RS för HTTP-port 80 med IP-”alla tilldelade”.</span><span class="sxs-lookup"><span data-stu-id="639a4-321">By default, RS is configured for HTTP port 80 with IP “All Assigned”.</span></span> <span data-ttu-id="639a4-322">tooadd HTTPS:</span><span class="sxs-lookup"><span data-stu-id="639a4-322">tooadd HTTPS:</span></span>
   
   1. <span data-ttu-id="639a4-323">I **SSL-certifikat**: Välj hello-certifikat som du vill toouse, till exempel [VM name]. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="639a4-323">In **SSL Certificate**: select hello certificate you want toouse, for example [VM name].cloudapp.net.</span></span> <span data-ttu-id="639a4-324">Om inga certifikat visas i avsnittet hello **steg 2: skapa ett servercertifikat** information om hur tooinstall och förtroende hello certifikatet på hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-324">If no certificates are listed, see hello section **Step 2: Create a Server Certificate** for information on how tooinstall and trust hello certificate on hello VM.</span></span>
   2. <span data-ttu-id="639a4-325">Under **SSL-Port**: Välj 443.</span><span class="sxs-lookup"><span data-stu-id="639a4-325">Under **SSL Port**: choose 443.</span></span> <span data-ttu-id="639a4-326">Om du har konfigurerat privata hello HTTPS-slutpunkt i hello VM med en annan privat port använda värdet här.</span><span class="sxs-lookup"><span data-stu-id="639a4-326">If you configured hello HTTPS private endpoint in hello VM with a different private port, use that value here.</span></span>
   3. <span data-ttu-id="639a4-327">Klicka på **tillämpa** och vänta tills hello åtgärden toocomplete.</span><span class="sxs-lookup"><span data-stu-id="639a4-327">Click **Apply** and wait for hello operation toocomplete.</span></span>
7. <span data-ttu-id="639a4-328">Hello vänster klickar du på **databasen**.</span><span class="sxs-lookup"><span data-stu-id="639a4-328">In hello left pane, click **Database**.</span></span>
   
   1. <span data-ttu-id="639a4-329">Klicka på **ändra Databas**e.</span><span class="sxs-lookup"><span data-stu-id="639a4-329">Click **Change Databas**e.</span></span>
   2. <span data-ttu-id="639a4-330">Klicka på **skapa en ny rapportserverdatabas** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="639a4-330">Click **Create a new report server database** and then click **Next**.</span></span>
   3. <span data-ttu-id="639a4-331">Lämna hello standard **servernamn**: som hello VM namn och lämna hello standard **autentiseringstyp** som **aktuell användare** – **-integreradsäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="639a4-331">Leave hello default **Server Name**: as hello VM name and leave hello default **Authentication Type** as **Current User** – **Integrated Security**.</span></span> <span data-ttu-id="639a4-332">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="639a4-332">Click **Next**.</span></span>
   4. <span data-ttu-id="639a4-333">Lämna hello standard **databasnamnet** som **ReportServer** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="639a4-333">Leave hello default **Database Name** as **ReportServer** and click **Next**.</span></span>
   5. <span data-ttu-id="639a4-334">Lämna hello standard **autentiseringstyp** som **Tjänstereferenser** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="639a4-334">Leave hello default **Authentication Type** as **Service Credentials** and click **Next**.</span></span>
   6. <span data-ttu-id="639a4-335">Klicka på **nästa** på hello **sammanfattning** sidan.</span><span class="sxs-lookup"><span data-stu-id="639a4-335">Click **Next** on hello **Summary** page.</span></span>
   7. <span data-ttu-id="639a4-336">När hello konfigurationen är klar klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="639a4-336">When hello configuration is complete, click **Finish**.</span></span>
8. <span data-ttu-id="639a4-337">Hello vänster klickar du på **URL för Report Manager**.</span><span class="sxs-lookup"><span data-stu-id="639a4-337">In hello left pane, click **Report Manager URL**.</span></span> <span data-ttu-id="639a4-338">Lämna hello standard **virtuell katalog** som **rapporter** och på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="639a4-338">Leave hello default **Virtual Directory** as **Reports** and click **Apply**.</span></span>
9. <span data-ttu-id="639a4-339">Klicka på **avsluta** tooclose hello Reporting Services Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="639a4-339">Click **Exit** tooclose hello Reporting Services Configuration Manager.</span></span>

## <a name="step-4-open-windows-firewall-port"></a><span data-ttu-id="639a4-340">Steg 4: Öppna Windows-brandväggen porten</span><span class="sxs-lookup"><span data-stu-id="639a4-340">Step 4: Open Windows Firewall Port</span></span>
> [!NOTE]
> <span data-ttu-id="639a4-341">Om du använde en rapportserver för hello skript tooconfigure hello kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="639a4-341">If you used one of hello scripts tooconfigure hello report server, you can skip this section.</span></span> <span data-ttu-id="639a4-342">hello skript med en steg tooopen hello brandväggsport.</span><span class="sxs-lookup"><span data-stu-id="639a4-342">hello script included a step tooopen hello firewall port.</span></span> <span data-ttu-id="639a4-343">hello standard var port 80 för HTTP och port 443 för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="639a4-343">hello default was port 80 for HTTP and port 443 for HTTPS.</span></span>
> 
> 

<span data-ttu-id="639a4-344">tooconnect via fjärranslutning tooReport Manager eller hello Report Server på hello virtuell dator, en TCP-slutpunkt krävs på hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-344">tooconnect remotely tooReport Manager or hello Report Server on hello virtual machine, a TCP Endpoint is required on hello VM.</span></span> <span data-ttu-id="639a4-345">Det är obligatoriskt tooopen hello samma port i brandväggen hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-345">It is required tooopen hello same port in hello VM’s firewall.</span></span> <span data-ttu-id="639a4-346">hello-slutpunkt skapas när hello VM har etablerats.</span><span class="sxs-lookup"><span data-stu-id="639a4-346">hello endpoint was created when hello VM was provisioned.</span></span>

<span data-ttu-id="639a4-347">Det här avsnittet innehåller grundläggande information om hur tooopen hello brandväggsport.</span><span class="sxs-lookup"><span data-stu-id="639a4-347">This section provides basic information on how tooopen hello firewall port.</span></span> <span data-ttu-id="639a4-348">Mer information finns i [konfigurera en brandvägg för Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)</span><span class="sxs-lookup"><span data-stu-id="639a4-348">For more information, see [Configure a Firewall for Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)</span></span>

> [!NOTE]
> <span data-ttu-id="639a4-349">Om du använde hello skriptet tooconfigure hello rapportservern kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="639a4-349">If you used hello script tooconfigure hello report server, you can skip this section.</span></span> <span data-ttu-id="639a4-350">hello skript med en steg tooopen hello brandväggsport.</span><span class="sxs-lookup"><span data-stu-id="639a4-350">hello script included a step tooopen hello firewall port.</span></span>
> 
> 

<span data-ttu-id="639a4-351">Om du har konfigurerat en privat port för HTTPS än 443, ändra hello följande skript på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="639a4-351">If you configured a private port for HTTPS other than 443, modify hello following script appropriately.</span></span> <span data-ttu-id="639a4-352">tooopen port **443** på hello Windows-brandväggen, utföra hello följande:</span><span class="sxs-lookup"><span data-stu-id="639a4-352">tooopen port **443** on hello Windows Firewall, complete hello following:</span></span>

1. <span data-ttu-id="639a4-353">Öppna en Windows PowerShell-fönster med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="639a4-353">Open a Windows PowerShell window with administrative privileges.</span></span>
2. <span data-ttu-id="639a4-354">Om du använder en annan port än 443 när du har konfigurerat hello HTTPS-slutpunkt på hello VM, uppdatera hello port i hello följande kommando och kör sedan hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="639a4-354">If you used a port other than 443 when you configured hello HTTPS endpoint on hello VM, update hello port in hello following command and then run hello command:</span></span>
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. <span data-ttu-id="639a4-355">När hello-kommandot har slutförts **Ok** visas i hello kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="639a4-355">When hello command completes, **Ok** is displayed in hello command prompt.</span></span>

<span data-ttu-id="639a4-356">tooverify att hello porten är öppen, öppna ett Windows PowerShell-fönster och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="639a4-356">tooverify that hello port is opened, open a Windows PowerShell window and run hello following command:</span></span>

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-hello-configuration"></a><span data-ttu-id="639a4-357">Verifiera konfigurationen av hello</span><span class="sxs-lookup"><span data-stu-id="639a4-357">Verify hello configuration</span></span>
<span data-ttu-id="639a4-358">tooverify hello grundläggande report server nu fungerar, öppna webbläsaren med administratörsbehörighet och sedan bläddra toohello följande report server ad report manager URL: er:</span><span class="sxs-lookup"><span data-stu-id="639a4-358">tooverify that hello basic report server functionality is now working, open your browser with administrative privileges and then browse toohello following report server ad report manager URLS:</span></span>

* <span data-ttu-id="639a4-359">På hello VM, bläddrar du toohello rapportserverns URL:</span><span class="sxs-lookup"><span data-stu-id="639a4-359">On hello VM, browse toohello report server URL:</span></span>
  
        http://localhost/reportserver
* <span data-ttu-id="639a4-360">Bläddra toohello report Manager URL: en på hello VM:</span><span class="sxs-lookup"><span data-stu-id="639a4-360">On hello VM, browse toohello report manger URL:</span></span>
  
        http://localhost/Reports
* <span data-ttu-id="639a4-361">Från den lokala datorn, bläddra toohello **remote** report Manager på hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-361">From your local computer, browse toohello **remote** report Manager on hello VM.</span></span> <span data-ttu-id="639a4-362">Uppdatera hello DNS-namnet i hello följande exempel efter behov.</span><span class="sxs-lookup"><span data-stu-id="639a4-362">Update hello DNS name in hello following example as appropriate.</span></span> <span data-ttu-id="639a4-363">När du uppmanas att ange ett lösenord, Använd hello administratörsautentiseringsuppgifter som du skapade när hello VM har etablerats.</span><span class="sxs-lookup"><span data-stu-id="639a4-363">When prompted for a password, use hello administrator credentials you created when hello VM was provisioned.</span></span> <span data-ttu-id="639a4-364">hello användarnamnet är i hello [domän]\[användarnamn] format, där hello domän är datornamnet för hello VM, till exempel ssrsnativecloud\testuser.</span><span class="sxs-lookup"><span data-stu-id="639a4-364">hello user name is in hello [Domain]\[user name] format, where hello domain is hello VM computer name, for example ssrsnativecloud\testuser.</span></span> <span data-ttu-id="639a4-365">Om du inte använder HTTP**S**, ta bort hello **s** i hello-URL.</span><span class="sxs-lookup"><span data-stu-id="639a4-365">If you are not using HTTP**S**, remove hello **s** in hello URL.</span></span> <span data-ttu-id="639a4-366">Hello nästa avsnittet finns information om hur du skapar ytterligare användare på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="639a4-366">See hello next section for information on creating additional users on VM.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/Reports
* <span data-ttu-id="639a4-367">Bläddra toohello remote rapportserverns URL från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="639a4-367">From your local computer, browse toohello remote report server URL.</span></span> <span data-ttu-id="639a4-368">Uppdatera hello DNS-namnet i hello följande exempel efter behov.</span><span class="sxs-lookup"><span data-stu-id="639a4-368">Update hello DNS name in hello following example as appropriate.</span></span> <span data-ttu-id="639a4-369">Ta bort hello s i hello URL Om du inte använder HTTPS.</span><span class="sxs-lookup"><span data-stu-id="639a4-369">If you are not using HTTPS, remove hello s in hello URL.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a><span data-ttu-id="639a4-370">Skapa användare och tilldela roller</span><span class="sxs-lookup"><span data-stu-id="639a4-370">Create Users and Assign Roles</span></span>
<span data-ttu-id="639a4-371">När konfigurera och verifiera hello rapportservern, en vanliga administrativa uppgifter är toocreate en eller flera användare och tilldela användare tooReporting Services-roller.</span><span class="sxs-lookup"><span data-stu-id="639a4-371">After configuring and verifying hello report server, a common administrative task is toocreate one or more users and assign users tooReporting Services roles.</span></span> <span data-ttu-id="639a4-372">Mer information finns i hello följande:</span><span class="sxs-lookup"><span data-stu-id="639a4-372">For more information, see hello following:</span></span>

* [<span data-ttu-id="639a4-373">Skapa ett lokalt användarkonto</span><span class="sxs-lookup"><span data-stu-id="639a4-373">Create a local user account</span></span>](https://technet.microsoft.com/library/cc770642.aspx)
* <span data-ttu-id="639a4-374">[Bevilja användaråtkomst tooa Report Server (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))</span><span class="sxs-lookup"><span data-stu-id="639a4-374">[Grant User Access tooa Report Server (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))</span></span>
* [<span data-ttu-id="639a4-375">Skapa och hantera rolltilldelningar</span><span class="sxs-lookup"><span data-stu-id="639a4-375">Create and Manage Role Assignments</span></span>](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a><span data-ttu-id="639a4-376">tooCreate och publicera rapporter toohello Azure-dator</span><span class="sxs-lookup"><span data-stu-id="639a4-376">tooCreate and Publish Reports toohello Azure Virtual Machine</span></span>
<span data-ttu-id="639a4-377">hello följande tabell visas några av hello alternativ tillgängliga toopublish befintliga rapporter från en lokal dator toohello rapportservern finns på hello Microsoft Azure-dator:</span><span class="sxs-lookup"><span data-stu-id="639a4-377">hello following table summarizes some of hello options available toopublish existing reports from an on-premises computer toohello report server hosted on hello Microsoft Azure Virtual Machine:</span></span>

* <span data-ttu-id="639a4-378">**RS.exe skript**: Använd RS.exe skriptet toocopy rapportobjekt från och befintliga report server tooyour Microsoft Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="639a4-378">**RS.exe script**: Use RS.exe script toocopy report items from and existing report server tooyour Microsoft Azure Virtual Machine.</span></span> <span data-ttu-id="639a4-379">Mer information finns i avsnittet hello ”enhetligt läge tooNative läge – Microsoft Azure-dator” i [exempel Reporting Services rs.exe skriptet tooMigrate innehåll mellan rapportservrar](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-379">For more information, see hello section “Native mode tooNative Mode – Microsoft Azure Virtual Machine” in [Sample Reporting Services rs.exe Script tooMigrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>
* <span data-ttu-id="639a4-380">**Report Builder**: hello virtuell dator innehåller hello klicka – en gång version av Microsoft SQL Server Report Builder.</span><span class="sxs-lookup"><span data-stu-id="639a4-380">**Report Builder**: hello virtual machine includes hello click-once version of Microsoft SQL Server Report Builder.</span></span> <span data-ttu-id="639a4-381">toostart Report builder hello första gången på hello virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="639a4-381">toostart Report builder hello first time on hello virtual machine:</span></span>
  
  1. <span data-ttu-id="639a4-382">Starta din webbläsare med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="639a4-382">Start your browser with administrative privileges.</span></span>
  2. <span data-ttu-id="639a4-383">Bläddra tooreport manager på hello virtuella datorn och klicka på **Report Builder** hello menyfliken.</span><span class="sxs-lookup"><span data-stu-id="639a4-383">Browse tooreport manager on hello virtual machine and click **Report Builder**  in hello ribbon.</span></span>
     
     <span data-ttu-id="639a4-384">Mer information finns i [installerar och avinstallerar stöder Report Builder](https://technet.microsoft.com/library/dd207038.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-384">For more information, see [Installing, Uninstalling, and Supporting Report Builder](https://technet.microsoft.com/library/dd207038.aspx).</span></span>
* <span data-ttu-id="639a4-385">**SQL Server Data Tools: VM**: Om du har skapat hello virtuell dator med SQL Server 2012 och SQL Server Data Tools är installerat på hello virtuell dator och kan vara används toocreate **Report Server-projekt** och rapporter om hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="639a4-385">**SQL Server Data Tools: VM**:  If you created hello VM with SQL Server 2012, then SQL Server Data Tools is installed on hello virtual machine and can be used toocreate **Report Server Projects** and reports on hello virtual machine.</span></span> <span data-ttu-id="639a4-386">SQL Server Data Tools kan publicera rapporter hello toohello rapportservern på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="639a4-386">SQL Server Data Tools can publish hello reports toohello report server on hello virtual machine.</span></span>
  
    <span data-ttu-id="639a4-387">Om du har skapat hello virtuell dator med SQLServer 2014 kan du installera SQL Server Data Tools - BI för visual Studio.</span><span class="sxs-lookup"><span data-stu-id="639a4-387">If you created hello VM with SQL server 2014, you can install SQL Server Data Tools- BI for visual Studio.</span></span> <span data-ttu-id="639a4-388">Mer information finns i hello följande:</span><span class="sxs-lookup"><span data-stu-id="639a4-388">For more information, see hello following:</span></span>
  
  * [<span data-ttu-id="639a4-389">Microsoft SQL Server Data Tools - Business Intelligence för Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="639a4-389">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2013</span></span>](https://www.microsoft.com/download/details.aspx?id=42313)
  * [<span data-ttu-id="639a4-390">Microsoft SQL Server Data Tools - Business Intelligence för Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="639a4-390">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2012</span></span>](https://www.microsoft.com/download/details.aspx?id=36843)
  * [<span data-ttu-id="639a4-391">SQL Server Data Tools och SQL Server Business Intelligence (SSDT BI)</span><span class="sxs-lookup"><span data-stu-id="639a4-391">SQL Server Data Tools and SQL Server Business Intelligence (SSDT-BI)</span></span>](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* <span data-ttu-id="639a4-392">**SQL Server Data Tools: Remote**: skapa en Reporting Services-projekt i SQL Server Data Tools som innehåller Reporting Services-rapporter på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="639a4-392">**SQL Server Data Tools: Remote**:  On your local computer, create a Reporting Services project in SQL Server Data Tools that contains Reporting Services reports.</span></span> <span data-ttu-id="639a4-393">Konfigurera hello projektet tooconnect toohello webbtjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="639a4-393">Configure hello project tooconnect toohello web service URL.</span></span>
  
    ![SSDT projektegenskaperna för SSRS-projekt](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* <span data-ttu-id="639a4-395">**Använda skript**: använda skriptet toocopy report serverinnehåll.</span><span class="sxs-lookup"><span data-stu-id="639a4-395">**Use script**: Use script toocopy report server content.</span></span> <span data-ttu-id="639a4-396">Mer information finns i [exempel Reporting Services rs.exe skriptet tooMigrate innehåll mellan rapportservrar](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-396">For more information, see [Sample Reporting Services rs.exe Script tooMigrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>

## <a name="minimize-cost-if-you-are-not-using-hello-vm"></a><span data-ttu-id="639a4-397">Minimera kostnaderna om du inte använder hello VM</span><span class="sxs-lookup"><span data-stu-id="639a4-397">Minimize cost if you are not using hello VM</span></span>
> [!NOTE]
> <span data-ttu-id="639a4-398">toominimize avgifter för dina Azure virtuella datorer som, Stäng hello VM från hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="639a4-398">toominimize charges for your Azure Virtual Machines when not in use, shut down hello VM from hello Azure classic portal.</span></span> <span data-ttu-id="639a4-399">Om du använder hello Windows Energialternativ inuti en VM tooshut ned hello VM fortfarande debiteras du hello samma belopp för hello VM.</span><span class="sxs-lookup"><span data-stu-id="639a4-399">If you use hello Windows power options inside a VM tooshut down hello VM, you are still charged hello same amount for hello VM.</span></span> <span data-ttu-id="639a4-400">tooreduce debiterar måste tooshut ned hello VM i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="639a4-400">tooreduce charges, you need tooshut down hello VM in hello Azure classic portal.</span></span> <span data-ttu-id="639a4-401">Om du inte längre behöver hello VM Kom ihåg toodelete hello VM och hello associerade VHD-filer tooavoid lagringskostnader för volymer. Mer information finns i avsnittet hello vanliga frågor och svar på [prisinformation för virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="639a4-401">If you no longer need hello VM, remember toodelete hello VM and hello associated .vhd files tooavoid storage charges.For more information, see hello FAQ section at [Virtual Machines Pricing Details](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>

## <a name="more-information"></a><span data-ttu-id="639a4-402">Mer information</span><span class="sxs-lookup"><span data-stu-id="639a4-402">More Information</span></span>
### <a name="resources"></a><span data-ttu-id="639a4-403">Resurser</span><span class="sxs-lookup"><span data-stu-id="639a4-403">Resources</span></span>
* <span data-ttu-id="639a4-404">För liknande innehåll relaterade tooa distribution på enskild server finns i SQL Server Business Intelligence och SharePoint 2013 [använda Windows PowerShell tooCreate en Azure VM med SQL Server BI och SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-404">For similar content related tooa single server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Use Windows PowerShell tooCreate an Azure VM With SQL Server BI and SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span></span>
* <span data-ttu-id="639a4-405">Liknande innehåll relaterade tooa finns flera server-distributionen av SQL Server Business Intelligence och SharePoint 2013 [distribuera SQL Server Business Intelligence i Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).</span><span class="sxs-lookup"><span data-stu-id="639a4-405">For similar content related tooa multi-server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Deploy SQL Server Business Intelligence in Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).</span></span>
* <span data-ttu-id="639a4-406">Allmän information finns i Närliggande toodeployments av SQL Server Business Intelligence i Azure Virtual Machines [SQL Server Business Intelligence i Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).</span><span class="sxs-lookup"><span data-stu-id="639a4-406">For General information related toodeployments of SQL Server Business Intelligence in Azure Virtual Machines, see [SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).</span></span>
* <span data-ttu-id="639a4-407">Mer information om hello kostnad Azure datorkostnader finns hello virtuella datorer på fliken i [Azure prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="639a4-407">For more information about hello cost of Azure compute charges, see hello Virtual Machines tab of [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span></span>

### <a name="community-content"></a><span data-ttu-id="639a4-408">Community-innehåll</span><span class="sxs-lookup"><span data-stu-id="639a4-408">Community Content</span></span>
* <span data-ttu-id="639a4-409">Steg-för-steg-anvisningar om hur toocreate en Reporting Services enhetligt läge rapporterar server utan att använda skriptet, se [värd för SQL Reporting Service på Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span><span class="sxs-lookup"><span data-stu-id="639a4-409">For step by step instructions on how toocreate a Reporting Services Native mode report server without using script, see [Hosting SQL Reporting Service on Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span></span>

### <a name="links-tooother-resources-for-sql-server-in-azure-vms"></a><span data-ttu-id="639a4-410">Länkar tooother resurser för SQL Server i virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="639a4-410">Links tooother resources for SQL Server in Azure VMs</span></span>
[<span data-ttu-id="639a4-411">SQLServer på Azure virtuella datorer – översikt</span><span class="sxs-lookup"><span data-stu-id="639a4-411">SQL Server on Azure Virtual Machines Overview</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

