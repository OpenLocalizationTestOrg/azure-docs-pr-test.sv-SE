---
title: "aaaConnect tooSQL databasen med hjälp av SQL Server Management Studio i Azure RemoteApp | Microsoft Docs"
description: "Använd den här självstudiekursen toolearn hur toouse SQL Server Management Studio i Azure RemoteApp för säkerhet och prestanda när du ansluter tooSQL databas"
services: sql-database
documentationcenter: 
author: adhurwit
manager: jhubbard
ms.assetid: 1052c83c-e7f5-4736-922f-216194d8874b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: adhurwit
ms.openlocfilehash: 73994f9a1eb3e48efa5d7c4f976b00cfcbc88d75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-tooconnect-toosql-database"></a><span data-ttu-id="e40c7-103">Använda SQL Server Management Studio i Azure RemoteApp tooconnect tooSQL databas</span><span class="sxs-lookup"><span data-stu-id="e40c7-103">Use SQL Server Management Studio in Azure RemoteApp tooconnect tooSQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e40c7-104">Azure RemoteApp upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="e40c7-104">Azure RemoteApp is being discontinued.</span></span> <span data-ttu-id="e40c7-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="e40c7-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
>

## <a name="introduction"></a><span data-ttu-id="e40c7-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="e40c7-106">Introduction</span></span>
<span data-ttu-id="e40c7-107">De här självstudierna visar hur toouse SQL Server Management Studio (SSMS) i Azure RemoteApp tooconnect tooSQL databas.</span><span class="sxs-lookup"><span data-stu-id="e40c7-107">This tutorial shows you how toouse SQL Server Management Studio (SSMS) in Azure RemoteApp tooconnect tooSQL Database.</span></span> <span data-ttu-id="e40c7-108">Vägleder dig genom processen hello för att konfigurera SQL Server Management Studio i Azure RemoteApp, förklarar hello fördelar och visar säkerhetsfunktioner som du kan använda i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e40c7-108">It walks you through hello process of setting up SQL Server Management Studio in Azure RemoteApp, explains hello benefits, and shows security features that you can use in Azure Active Directory.</span></span>

<span data-ttu-id="e40c7-109">**Uppskattad tid toocomplete:** 45 minuter</span><span class="sxs-lookup"><span data-stu-id="e40c7-109">**Estimated time toocomplete:** 45 minutes</span></span>

## <a name="ssms-in-azure-remoteapp"></a><span data-ttu-id="e40c7-110">SSMS i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="e40c7-110">SSMS in Azure RemoteApp</span></span>
<span data-ttu-id="e40c7-111">Azure RemoteApp är en RDS-tjänst i Azure som levererar program.</span><span class="sxs-lookup"><span data-stu-id="e40c7-111">Azure RemoteApp is an RDS service in Azure that delivers applications.</span></span> <span data-ttu-id="e40c7-112">Du kan lära dig mer om den här: [vad är RemoteApp?](../remoteapp/remoteapp-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="e40c7-112">You can learn more about it here: [What is RemoteApp?](../remoteapp/remoteapp-whatis.md)</span></span>

<span data-ttu-id="e40c7-113">SSMS som körs i Azure RemoteApp ger hello samma upplevelse som körs lokalt SSMS.</span><span class="sxs-lookup"><span data-stu-id="e40c7-113">SSMS running in Azure RemoteApp gives you hello same experience as running SSMS locally.</span></span>

![Skärmbild som visar SSMS som körs i Azure RemoteApp][1]

## <a name="benefits"></a><span data-ttu-id="e40c7-115">Fördelar</span><span class="sxs-lookup"><span data-stu-id="e40c7-115">Benefits</span></span>
<span data-ttu-id="e40c7-116">Det finns många fördelar toousing SSMS i Azure RemoteApp, inklusive:</span><span class="sxs-lookup"><span data-stu-id="e40c7-116">There are many benefits toousing SSMS in Azure RemoteApp, including:</span></span>

* <span data-ttu-id="e40c7-117">Port 1433 på Azure SQL server har inte toobe exponeras externt (utanför Azure).</span><span class="sxs-lookup"><span data-stu-id="e40c7-117">Port 1433 on Azure SQL server does not have toobe exposed externally (outside of Azure).</span></span>
* <span data-ttu-id="e40c7-118">Inga måste tookeep att lägga till och ta bort IP-adresser i brandväggen för hello Azure SQL server.</span><span class="sxs-lookup"><span data-stu-id="e40c7-118">No need tookeep adding and removing IP addresses in hello Azure SQL server firewall.</span></span>
* <span data-ttu-id="e40c7-119">Alla Azure RemoteApp-anslutningar som sker över HTTPS med port 443 krypterade Remote Desktop protocol</span><span class="sxs-lookup"><span data-stu-id="e40c7-119">All Azure RemoteApp connections occur over HTTPS on port 443 using encrypted Remote Desktop protocol</span></span>
* <span data-ttu-id="e40c7-120">Det är flera användare och kan skalas.</span><span class="sxs-lookup"><span data-stu-id="e40c7-120">It is multi-user and can scale.</span></span>
* <span data-ttu-id="e40c7-121">Finns det prestandafördelar från med SSMS i hello samma region som hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e40c7-121">There is a performance gain from having SSMS in hello same region as hello SQL Database.</span></span>
* <span data-ttu-id="e40c7-122">Du kan granska användning av Azure RemoteApp med hello Premium edition av Azure Active Directory som har aktivitetsrapporter för användaren.</span><span class="sxs-lookup"><span data-stu-id="e40c7-122">You can audit use of Azure RemoteApp with hello Premium edition of Azure Active Directory which has user activity reports.</span></span>
* <span data-ttu-id="e40c7-123">Du kan aktivera multifaktorautentisering (MFA).</span><span class="sxs-lookup"><span data-stu-id="e40c7-123">You can enable multi-factor authentication (MFA).</span></span>
* <span data-ttu-id="e40c7-124">Åtkomst SSMS var som helst när du använder någon av hello stöds Azure RemoteApp-klienter som innehåller iOS, Android, Mac, Windows Phone och Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="e40c7-124">Access SSMS anywhere when using any of hello supported Azure RemoteApp clients which includes iOS, Android, Mac, Windows Phone, and Windows PC’s.</span></span>

## <a name="create-hello-azure-remoteapp-collection"></a><span data-ttu-id="e40c7-125">Skapa hello Azure RemoteApp-samling</span><span class="sxs-lookup"><span data-stu-id="e40c7-125">Create hello Azure RemoteApp collection</span></span>
<span data-ttu-id="e40c7-126">Här följer hello steg toocreate Azure RemoteApp-samlingen med SSMS:</span><span class="sxs-lookup"><span data-stu-id="e40c7-126">Here are hello steps toocreate your Azure RemoteApp collection with SSMS:</span></span>

### <a name="1-create-a-new-windows-vm-from-image"></a><span data-ttu-id="e40c7-127">1. Skapa en ny Windows virtuell dator från bild</span><span class="sxs-lookup"><span data-stu-id="e40c7-127">1. Create a new Windows VM from Image</span></span>
<span data-ttu-id="e40c7-128">Använd hello ”Windows Server Remote Desktop Session Host Windows Server 2012 R2” bilden från hello galleriet toomake den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e40c7-128">Use hello "Windows Server Remote Desktop Session Host Windows Server 2012 R2" Image from hello Gallery toomake your new VM.</span></span>

### <a name="2-install-ssms-from-sql-express"></a><span data-ttu-id="e40c7-129">2. Installera SSMS från SQL Express</span><span class="sxs-lookup"><span data-stu-id="e40c7-129">2. Install SSMS from SQL Express</span></span>
<span data-ttu-id="e40c7-130">Gå till hello ny virtuell dator och navigera toothis hämtningssidan: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span><span class="sxs-lookup"><span data-stu-id="e40c7-130">Go onto hello new VM and navigate toothis download page: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span></span>

<span data-ttu-id="e40c7-131">Det finns en alternativet tooonly nedladdning SSMS.</span><span class="sxs-lookup"><span data-stu-id="e40c7-131">There is an option tooonly download SSMS.</span></span> <span data-ttu-id="e40c7-132">Gå till installationskatalogen för hello efter hämtning, och kör installationsprogrammet tooinstall SSMS.</span><span class="sxs-lookup"><span data-stu-id="e40c7-132">After download, go into hello install directory and run Setup tooinstall SSMS.</span></span>

<span data-ttu-id="e40c7-133">Du måste också tooinstall SQL Server 2014 Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="e40c7-133">You also need tooinstall SQL Server 2014 Service Pack 1.</span></span> <span data-ttu-id="e40c7-134">Du kan hämta: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span><span class="sxs-lookup"><span data-stu-id="e40c7-134">You can download it here: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span></span>

<span data-ttu-id="e40c7-135">SQL Server 2014 Service Pack 1 innehåller grundläggande funktioner för att arbeta med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e40c7-135">SQL Server 2014 Service Pack 1 includes essential functionality for working with Azure SQL Database.</span></span>

### <a name="3-run-validate-script-and-sysprep"></a><span data-ttu-id="e40c7-136">3. Kör Validera skript och Sysprep</span><span class="sxs-lookup"><span data-stu-id="e40c7-136">3. Run Validate script and Sysprep</span></span>
<span data-ttu-id="e40c7-137">På hello är hello Virtuella skrivbord ett PowerShell.skript som heter verifiera.</span><span class="sxs-lookup"><span data-stu-id="e40c7-137">On hello desktop of hello VM is a PowerShell script called Validate.</span></span> <span data-ttu-id="e40c7-138">Kör detta genom att dubbelklicka på.</span><span class="sxs-lookup"><span data-stu-id="e40c7-138">Run this by double-clicking.</span></span> <span data-ttu-id="e40c7-139">Den verifierar att hello VM är klar toobe som används för fjärråtkomst att vara värd för program.</span><span class="sxs-lookup"><span data-stu-id="e40c7-139">It will verify that hello VM is ready toobe used for remote hosting of applications.</span></span> <span data-ttu-id="e40c7-140">När verifieringen är klar uppmanas toorun sysprep - Välj toorun den.</span><span class="sxs-lookup"><span data-stu-id="e40c7-140">When verification is complete, it will ask toorun sysprep - choose toorun it.</span></span>

<span data-ttu-id="e40c7-141">När sysprep har slutförts stängs hello VM.</span><span class="sxs-lookup"><span data-stu-id="e40c7-141">When sysprep completes, it will shut down hello VM.</span></span>

<span data-ttu-id="e40c7-142">toolearn mer om hur du skapar en Azure RemoteApp-avbildning, se: [hur toocreate en mall för RemoteApp-avbildning i Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="e40c7-142">toolearn more about creating a Azure RemoteApp image, see: [How toocreate a RemoteApp template image in Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span></span>

### <a name="4-capture-image"></a><span data-ttu-id="e40c7-143">4. Avbildning</span><span class="sxs-lookup"><span data-stu-id="e40c7-143">4. Capture image</span></span>
<span data-ttu-id="e40c7-144">När hello VM har avbrutits, hitta i aktuella hello-portalen och fånga den.</span><span class="sxs-lookup"><span data-stu-id="e40c7-144">When hello VM has stopped running, find it in hello current portal and capture it.</span></span>

<span data-ttu-id="e40c7-145">toolearn mer information om en avbildning finns [en avbildning av en virtuell dator för Windows Azure som skapats med hello klassiska distributionsmodellen](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="e40c7-145">toolearn more about capturing an image, see [Capture an image of an Azure Windows virtual machine created with hello classic deployment model](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

### <a name="5-add-tooazure-remoteapp-template-images"></a><span data-ttu-id="e40c7-146">5. Lägg till tooAzure RemoteApp-mallavbildningar</span><span class="sxs-lookup"><span data-stu-id="e40c7-146">5. Add tooAzure RemoteApp Template images</span></span>
<span data-ttu-id="e40c7-147">Gå toohello Mallavbildningarna fliken hello Azure RemoteApp-avsnittet i aktuella hello-portalen, och klicka på Lägg till.</span><span class="sxs-lookup"><span data-stu-id="e40c7-147">In hello Azure RemoteApp section of hello current portal, go toohello Template Images tab and click Add.</span></span> <span data-ttu-id="e40c7-148">Markera ”Importera en bild från biblioteket för virtuella datorer” i hello popup-ruta och välj sedan hello bilden som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="e40c7-148">In hello pop-up box, select "Import an image from your Virtual Machines library" and then choose hello Image that you just created.</span></span>

### <a name="6-create-cloud-collection"></a><span data-ttu-id="e40c7-149">6. Skapa molnsamling</span><span class="sxs-lookup"><span data-stu-id="e40c7-149">6. Create cloud collection</span></span>
<span data-ttu-id="e40c7-150">Skapa en ny Azure RemoteApp-Molnsamling i aktuella hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e40c7-150">In hello current portal, create a new Azure RemoteApp Cloud Collection.</span></span> <span data-ttu-id="e40c7-151">Välj hello-Mallavbildningen som du nyss importerade med SSMS som är installerade på den.</span><span class="sxs-lookup"><span data-stu-id="e40c7-151">Choose hello Template Image that you just imported with SSMS installed on it.</span></span>

![Skapa ny molnsamling][2]

### <a name="7-publish-ssms"></a><span data-ttu-id="e40c7-153">7. Publicera SSMS</span><span class="sxs-lookup"><span data-stu-id="e40c7-153">7. Publish SSMS</span></span>
<span data-ttu-id="e40c7-154">Hej på hello publicering för den nya samlingen i molnet, välja publicera ett program från Start-menyn och välj sedan SSMS hello-listan.</span><span class="sxs-lookup"><span data-stu-id="e40c7-154">On hello Publishing tab of your new cloud collection, select Publish an application from hello Start Menu and then choose SSMS from hello list.</span></span>

![Publicera appen][5]

### <a name="8-add-users"></a><span data-ttu-id="e40c7-156">8. Lägga till användare</span><span class="sxs-lookup"><span data-stu-id="e40c7-156">8. Add users</span></span>
<span data-ttu-id="e40c7-157">Du kan välja hello-användare som ska ha åtkomst toothis Azure RemoteApp-samling som bara innehåller SSMS hello användaråtkomst på fliken.</span><span class="sxs-lookup"><span data-stu-id="e40c7-157">On hello User Access tab you can select hello users that will have access toothis Azure RemoteApp collection which only includes SSMS.</span></span>

![Lägg till användare][6]

### <a name="9-install-hello-azure-remoteapp-client-application"></a><span data-ttu-id="e40c7-159">9. Installera klientprogrammet för hello Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="e40c7-159">9. Install hello Azure RemoteApp client application</span></span>
<span data-ttu-id="e40c7-160">Du kan hämta och installera en Azure RemoteApp-klient här: [hämta | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span><span class="sxs-lookup"><span data-stu-id="e40c7-160">You can download and install a Azure RemoteApp client here: [Download | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span></span>

## <a name="configure-azure-sql-server"></a><span data-ttu-id="e40c7-161">Konfigurera Azure SQL-server</span><span class="sxs-lookup"><span data-stu-id="e40c7-161">Configure Azure SQL server</span></span>
<span data-ttu-id="e40c7-162">hello har konfiguration krävs är tooensure Azure Services aktiverats för hello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e40c7-162">hello only configuration needed is tooensure that Azure Services is enabled for hello firewall.</span></span> <span data-ttu-id="e40c7-163">Om du använder den här lösningen kan sedan behöver du inte tooadd alla IP-adresser tooopen hello brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e40c7-163">If you use this solution, then you do not need tooadd any IP addresses tooopen hello firewall.</span></span> <span data-ttu-id="e40c7-164">hello trafik som tillåts toohello SQL Server är från andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e40c7-164">hello network traffic that is allowed toohello SQL Server is from other Azure services.</span></span>

![Tillåt Azure][4]

## <a name="multi-factor-authentication-mfa"></a><span data-ttu-id="e40c7-166">Multifaktorautentisering (MFA)</span><span class="sxs-lookup"><span data-stu-id="e40c7-166">Multi-Factor Authentication (MFA)</span></span>
<span data-ttu-id="e40c7-167">MFA kan aktiveras för det här programmet specifikt.</span><span class="sxs-lookup"><span data-stu-id="e40c7-167">MFA can be enabled for this application specifically.</span></span> <span data-ttu-id="e40c7-168">Gå toohello program fliken i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e40c7-168">Go toohello Applications tab of your Azure Active Directory.</span></span> <span data-ttu-id="e40c7-169">Du hittar en post för Microsoft Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="e40c7-169">You will find an entry for Microsoft Azure RemoteApp.</span></span> <span data-ttu-id="e40c7-170">Om du klickar på programmet och sedan konfigurera visas hello sida där du kan aktivera MFA för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="e40c7-170">If you click that application and then configure, you will see hello page below where you can enable MFA for this application.</span></span>

![Aktivera MFA][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a><span data-ttu-id="e40c7-172">Gransknings-och användaraktivitet med Azure Active Directory Premium</span><span class="sxs-lookup"><span data-stu-id="e40c7-172">Audit user activity with Azure Active Directory Premium</span></span>
<span data-ttu-id="e40c7-173">Om du inte har Azure AD Premium kan du ha tooturn hello den på i avsnittet licenser i din katalog.</span><span class="sxs-lookup"><span data-stu-id="e40c7-173">If you do not have Azure AD Premium, then you have tooturn it on in hello Licenses section of your directory.</span></span> <span data-ttu-id="e40c7-174">Med Premium aktiverat, kan du tilldela användare toohello Premium-nivå.</span><span class="sxs-lookup"><span data-stu-id="e40c7-174">With Premium enabled, you can assign users toohello Premium level.</span></span>

<span data-ttu-id="e40c7-175">När du går tooa användare i Azure Active Directory går du sedan toohello aktivitet fliken toosee inloggningen information tooAzure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="e40c7-175">When you go tooa user in your Azure Active Directory, you can then go toohello Activity tab toosee login information tooAzure RemoteApp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e40c7-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e40c7-176">Next steps</span></span>
<span data-ttu-id="e40c7-177">När du har slutfört alla hello senare steg du ska kunna toorun hello Azure RemoteApp-klienten och logga in med en tilldelad användare.</span><span class="sxs-lookup"><span data-stu-id="e40c7-177">After completing all hello above steps, you will be able toorun hello Azure RemoteApp client and log-in with an assigned user.</span></span> <span data-ttu-id="e40c7-178">Visas med SSMS som en av dina program och du kan köra den som om den har installerats på datorn med åtkomst tooAzure SQLServer.</span><span class="sxs-lookup"><span data-stu-id="e40c7-178">You will be presented with SSMS as one of your applications, and you can run it as you would if it were installed on your computer with access tooAzure SQL server.</span></span>

<span data-ttu-id="e40c7-179">Mer information om hur toomake hello anslutning tooSQL databasen finns [ansluta tooSQL databasen med SQL Server Management Studio och utföra en exempelfråga i T-SQL](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="e40c7-179">For more information on how toomake hello connection tooSQL Database, see [Connect tooSQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>

<span data-ttu-id="e40c7-180">Det är allt som för tillfället.</span><span class="sxs-lookup"><span data-stu-id="e40c7-180">That's everything for now.</span></span> <span data-ttu-id="e40c7-181">Ha det så kul!</span><span class="sxs-lookup"><span data-stu-id="e40c7-181">Enjoy!</span></span>

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png