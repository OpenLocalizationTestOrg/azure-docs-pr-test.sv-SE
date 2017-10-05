---
title: "Konfigurera din API Management-tjänsten med Git - Azure | Microsoft Docs"
description: "Lär dig mer om att spara och konfigurera din API Management service configuration med Git."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: mattfarm
ms.assetid: 364cd53e-88fb-4301-a093-f132fa1f88f5
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: f5d6bb7ccbf15424e9940ccda2fac668a2af5a57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a><span data-ttu-id="2686f-103">Spara och konfigurera din API Management service configuration med Git</span><span class="sxs-lookup"><span data-stu-id="2686f-103">How to save and configure your API Management service configuration using Git</span></span>
> 
> 

<span data-ttu-id="2686f-104">Varje instans för API Management-tjänsten har en databas som innehåller information om konfiguration och metadata för tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="2686f-104">Each API Management service instance maintains a configuration database that contains information about the configuration and metadata for the service instance.</span></span> <span data-ttu-id="2686f-105">Ändringar kan göras i tjänstinstansen genom att ändra en inställning i portalen utgivare, med hjälp av en PowerShell-cmdlet eller göra ett REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="2686f-105">Changes can be made to the service instance by changing a setting in the publisher portal, using a PowerShell cmdlet, or making a REST API call.</span></span> <span data-ttu-id="2686f-106">Du kan också hantera konfigurationen för service-instans med Git, aktivera tjänsten hanteringsscenarier som förutom dessa metoder:</span><span class="sxs-lookup"><span data-stu-id="2686f-106">In addition to these methods, you can also manage your service instance configuration using Git, enabling service management scenarios such as:</span></span>

* <span data-ttu-id="2686f-107">Konfigurationen versionshantering - ladda ned och lagra olika versioner av tjänstkonfigurationen av</span><span class="sxs-lookup"><span data-stu-id="2686f-107">Configuration versioning - download and store different versions of your service configuration</span></span>
* <span data-ttu-id="2686f-108">Massredigera konfigurationsändringar – göra ändringar i flera delar av din tjänstkonfiguration i lokala databasen och integrera ändringarna tillbaka till servern med en enda åtgärd</span><span class="sxs-lookup"><span data-stu-id="2686f-108">Bulk configuration changes - make changes to multiple parts of your service configuration in your local repository and integrate the changes back to the server with a single operation</span></span>
* <span data-ttu-id="2686f-109">Bekant Git toolchain och arbetsflöde – använda Git verktygsuppsättning och arbetsflöden som du redan är bekant med</span><span class="sxs-lookup"><span data-stu-id="2686f-109">Familiar Git toolchain and workflow - use the Git tooling and workflows that you are already familiar with</span></span>

<span data-ttu-id="2686f-110">Följande diagram visar en översikt över olika sätt att konfigurera din instans för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2686f-110">The following diagram shows an overview of the different ways to configure your API Management service instance.</span></span>

![Konfigurera Git][api-management-git-configure]

<span data-ttu-id="2686f-112">När du gör ändringar i din tjänst med hjälp av publisher-portalen, PowerShell-cmdlets eller REST API du hanterar din service configuration databas med hjälp av den `https://{name}.management.azure-api.net` slutpunkt, som visas till höger i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="2686f-112">When you make changes to your service using the publisher portal, PowerShell cmdlets, or the REST API, you are managing your service configuration database using the `https://{name}.management.azure-api.net` endpoint, as shown on the right side of the diagram.</span></span> <span data-ttu-id="2686f-113">Till vänster i diagrammet visar hur du kan hantera tjänstkonfigurationen av med Git och Git-lagringsplats för tjänsten finns på `https://{name}.scm.azure-api.net`.</span><span class="sxs-lookup"><span data-stu-id="2686f-113">The left side of the diagram illustrates how you can manage your service configuration using Git and Git repository for your service located at `https://{name}.scm.azure-api.net`.</span></span>

<span data-ttu-id="2686f-114">Följande steg ger en översikt över hantering av din API Management service-instans med Git.</span><span class="sxs-lookup"><span data-stu-id="2686f-114">The following steps provide an overview of managing your API Management service instance using Git.</span></span>

1. <span data-ttu-id="2686f-115">Konfiguration av Git i din tjänst</span><span class="sxs-lookup"><span data-stu-id="2686f-115">Access Git configuration in your service</span></span>
2. <span data-ttu-id="2686f-116">Spara dina konfigurationsdatabas Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="2686f-116">Save your service configuration database to your Git repository</span></span>
3. <span data-ttu-id="2686f-117">Klona Git-lagringsplatsen till den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="2686f-117">Clone the Git repo to your local machine</span></span>
4. <span data-ttu-id="2686f-118">Hämta senaste lagringsplatsen till den lokala datorn och genomförande och push ändringarna tillbaka till din lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="2686f-118">Pull the latest repo down to your local machine, and commit and push changes back to your repo</span></span>
5. <span data-ttu-id="2686f-119">Distribuera ändringarna från din lagringsplats i din konfigurationsdatabas</span><span class="sxs-lookup"><span data-stu-id="2686f-119">Deploy the changes from your repo into your service configuration database</span></span>

<span data-ttu-id="2686f-120">Den här artikeln beskriver hur du aktiverar och använder Git för att hantera tjänstkonfigurationen av och innehåller en referens för filer och mappar i Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="2686f-120">This article describes how to enable and use Git to manage your service configuration and provides a reference for the files and folders in the Git repository.</span></span>

## <a name="access-git-configuration-in-your-service"></a><span data-ttu-id="2686f-121">Konfiguration av Git i din tjänst</span><span class="sxs-lookup"><span data-stu-id="2686f-121">Access Git configuration in your service</span></span>
<span data-ttu-id="2686f-122">Du kan snabbt visa status för Git-konfigurationen genom att visa Git-ikonen i det övre högra hörnet av portalen utgivare.</span><span class="sxs-lookup"><span data-stu-id="2686f-122">You can quickly view the status of your Git configuration by viewing the Git icon in the upper-right corner of the publisher portal.</span></span> <span data-ttu-id="2686f-123">I det här exemplet anger statusmeddelanden som det finns osparade ändringar i databasen.</span><span class="sxs-lookup"><span data-stu-id="2686f-123">In this example, the status message indicates that there are unsaved changes to the repository.</span></span> <span data-ttu-id="2686f-124">Det beror på att konfigurationsdatabasen för API Management-tjänsten inte har sparats i databasen.</span><span class="sxs-lookup"><span data-stu-id="2686f-124">This is because the API Management service configuration database has not yet been saved to the repository.</span></span>

![Git-status][api-management-git-icon-enable]

<span data-ttu-id="2686f-126">Om du vill visa och konfigurera inställningarna i Git, du kan antingen klicka på ikonen Git, eller klicka på den **säkerhet** menyn och navigera till den **Configuration databasen** fliken.</span><span class="sxs-lookup"><span data-stu-id="2686f-126">To view and configure your Git configuration settings, you can either click the Git icon, or click the **Security** menu and navigate to the **Configuration repository** tab.</span></span>

![Aktivera GIT][api-management-enable-git]

> [!IMPORTANT]
> <span data-ttu-id="2686f-128">Alla hemligheter som inte har definierats som egenskaper kommer att lagras i databasen och kommer att finnas kvar i historiken tills du inaktivera och aktivera Git-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2686f-128">Any secrets that are not defined as properties will be stored in the repository and will remain in its history until you disable and re-enable Git access.</span></span> <span data-ttu-id="2686f-129">Egenskaper innehåller en säker plats för att hantera konstanta strängvärden, inklusive hemligheter, för alla API-konfiguration och principer, så att du inte behöver lagra dem direkt i din principrapporter.</span><span class="sxs-lookup"><span data-stu-id="2686f-129">Properties provide a secure place to manage constant string values, including secrets, across all API configuration and policies, so you don't have to store them directly in your policy statements.</span></span> <span data-ttu-id="2686f-130">Mer information finns i [använda egenskaper i Azure API Management-principer](api-management-howto-properties.md).</span><span class="sxs-lookup"><span data-stu-id="2686f-130">For more information, see [How to use properties in Azure API Management policies](api-management-howto-properties.md).</span></span>
> 
> 

<span data-ttu-id="2686f-131">Mer information om att aktivera eller inaktivera Git-åtkomst med hjälp av REST-API finns [aktivera eller inaktivera Git-åtkomst med hjälp av REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span><span class="sxs-lookup"><span data-stu-id="2686f-131">For information on enabling or disabling Git access using the REST API, see [Enable or disable Git access using the REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span></span>

## <a name="to-save-the-service-configuration-to-the-git-repository"></a><span data-ttu-id="2686f-132">Spara konfigurationen för tjänsten i Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="2686f-132">To save the service configuration to the Git repository</span></span>
<span data-ttu-id="2686f-133">Det första steget innan kloning databasen är att spara det aktuella tillståndet för tjänstens konfiguration i databasen.</span><span class="sxs-lookup"><span data-stu-id="2686f-133">The first step before cloning the repository is to save the current state of the service configuration to the repository.</span></span> <span data-ttu-id="2686f-134">Klicka på **spara konfigurationen till databasen**.</span><span class="sxs-lookup"><span data-stu-id="2686f-134">Click **Save configuration to repository**.</span></span>

![Spara konfigurationen][api-management-save-configuration]

<span data-ttu-id="2686f-136">Gör eventuella ändringar på bekräftelsesidan och klicka på **Ok** att spara.</span><span class="sxs-lookup"><span data-stu-id="2686f-136">Make any desired changes on the confirmation screen and click **Ok** to save.</span></span>

![Spara konfigurationen][api-management-save-configuration-confirm]

<span data-ttu-id="2686f-138">Efter en liten stund konfigurationen sparas och visas status för konfiguration av databasen, inklusive datum och tid för senaste konfigurationsändringen och den senaste synkroniseringen mellan konfigurationen för tjänsten och -databasen.</span><span class="sxs-lookup"><span data-stu-id="2686f-138">After a few moments the configuration is saved, and the configuration status of the repository is displayed, including the date and time of the last configuration change and the last synchronization between the service configuration and the repository.</span></span>

![Konfigurationsstatus][api-management-configuration-status]

<span data-ttu-id="2686f-140">När konfigurationen har sparats i databasen, kan den klonas.</span><span class="sxs-lookup"><span data-stu-id="2686f-140">Once the configuration is saved to the repository, it can be cloned.</span></span>

<span data-ttu-id="2686f-141">Information om hur du utför den här åtgärden med hjälp av REST-API finns [Commit configuration ögonblicksbild med hjälp av REST-API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span><span class="sxs-lookup"><span data-stu-id="2686f-141">For information on performing this operation using the REST API, see [Commit configuration snapshot using the REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span></span>

## <a name="to-clone-the-repository-to-your-local-machine"></a><span data-ttu-id="2686f-142">Att klona databasen på den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="2686f-142">To clone the repository to your local machine</span></span>
<span data-ttu-id="2686f-143">Om du vill klona en databas måste URL: en till databasen, ett användarnamn och ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="2686f-143">To clone a repository, you need the URL to your repository, a user name, and a password.</span></span> <span data-ttu-id="2686f-144">Användarnamn och URL visas längst upp i den **Configuration databasen** fliken.</span><span class="sxs-lookup"><span data-stu-id="2686f-144">The user name and URL are displayed near the top of the **Configuration repository** tab.</span></span>

![Git-klonen][api-management-configuration-git-clone]

<span data-ttu-id="2686f-146">Lösenordet genereras längst ned i den **Configuration databasen** fliken.</span><span class="sxs-lookup"><span data-stu-id="2686f-146">The password is generated at the bottom of the **Configuration repository** tab.</span></span>

![Generera lösenord][api-management-generate-password]

<span data-ttu-id="2686f-148">För att generera ett lösenord måste du först kontrollera att den **upphör att gälla** är inställd på önskad utgångsdatum och utgångstid och klicka sedan på **generera Token**.</span><span class="sxs-lookup"><span data-stu-id="2686f-148">To generate a password, first ensure that the **Expiry** is set to the desired expiration date and time, and then click **Generate Token**.</span></span>

![Lösenord][api-management-password]

> [!IMPORTANT]
> <span data-ttu-id="2686f-150">Anteckna det här lösenordet.</span><span class="sxs-lookup"><span data-stu-id="2686f-150">Make a note of this password.</span></span> <span data-ttu-id="2686f-151">När du lämnar den här sidan visas inte lösenordet igen.</span><span class="sxs-lookup"><span data-stu-id="2686f-151">Once you leave this page the password will not be displayed again.</span></span>
> 
> 

<span data-ttu-id="2686f-152">I följande exempel används verktyget Git Bash från [Git för Windows](http://www.git-scm.com/downloads) men du kan använda alla Git-verktyg som du är bekant med.</span><span class="sxs-lookup"><span data-stu-id="2686f-152">The following examples use the Git Bash tool from [Git for Windows](http://www.git-scm.com/downloads) but you can use any Git tool that you are familiar with.</span></span>

<span data-ttu-id="2686f-153">Öppna Git-verktyget i mappen önskade och kör följande kommando för att klona git-lagringsplatsen till den lokala datorn med hjälp av kommandot som tillhandahålls av publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="2686f-153">Open your Git tool in the desired folder and run the following command to clone the git repository to your local machine, using the command provided by the publisher portal.</span></span>

```
git clone https://bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="2686f-154">Ange användarnamn och lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="2686f-154">Provide the user name and password when prompted.</span></span>

<span data-ttu-id="2686f-155">Om du får inga fel, försök att ändra din `git clone` kommando för att inkludera användarnamn och lösenord, som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="2686f-155">If you receive any errors, try modifying your `git clone` command to include the user name and password, as shown in the following example.</span></span>

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="2686f-156">Om detta ger ett fel kan du försöka URL kodning lösenord delen av kommandot.</span><span class="sxs-lookup"><span data-stu-id="2686f-156">If this provides an error, try URL encoding the password portion of the command.</span></span> <span data-ttu-id="2686f-157">Ett snabbt sätt att göra detta är att öppna Visual Studio och kör följande kommando i den **kommandofönstret**.</span><span class="sxs-lookup"><span data-stu-id="2686f-157">One quick way to do this is to open Visual Studio, and issue the following command in the **Immediate Window**.</span></span> <span data-ttu-id="2686f-158">Öppna den **kommandofönstret**, öppna någon lösning eller ett projekt i Visual Studio (eller skapa en ny tom konsolapp), och välj **Windows**, **Immediate** från den **Felsöka** menyn.</span><span class="sxs-lookup"><span data-stu-id="2686f-158">To open the **Immediate Window**, open any solution or project in Visual Studio (or create a new empty console application), and choose **Windows**, **Immediate** from the **Debug** menu.</span></span>

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

<span data-ttu-id="2686f-159">Använda kodade lösenordet tillsammans med din Användarplats för namn och databasen för att skapa git-kommandot.</span><span class="sxs-lookup"><span data-stu-id="2686f-159">Use the encoded password along with your user name and repository location to construct the git command.</span></span>

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="2686f-160">När databasen är klonad kan du visa och arbeta med det i det lokala filsystemet.</span><span class="sxs-lookup"><span data-stu-id="2686f-160">Once the repository is cloned you can view and work with it in your local file system.</span></span> <span data-ttu-id="2686f-161">Mer information finns i [fil- och struktur referens för lokal Git-lagringsplats](#file-and-folder-structure-reference-of-local-git-repository).</span><span class="sxs-lookup"><span data-stu-id="2686f-161">For more information, see [File and folder structure reference of local Git repository](#file-and-folder-structure-reference-of-local-git-repository).</span></span>

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a><span data-ttu-id="2686f-162">Uppdatera din lokal databas med aktuell instans konfigurationen för tjänsten</span><span class="sxs-lookup"><span data-stu-id="2686f-162">To update your local repository with the most current service instance configuration</span></span>
<span data-ttu-id="2686f-163">Om du gör ändringar i API Management service-instans i publisher-portalen eller med hjälp av REST-API, måste du spara dessa ändringar till databasen innan du kan uppdatera din lokala databasen med de senaste ändringarna.</span><span class="sxs-lookup"><span data-stu-id="2686f-163">If you make changes to your API Management service instance in the publisher portal or using the REST API, you must save these changes to the repository before you can update your local repository with the latest changes.</span></span> <span data-ttu-id="2686f-164">Gör detta genom att klicka på **spara konfigurationen till databasen** på den **Configuration databasen** i portalen publisher och sedan kör du följande kommando i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="2686f-164">To do this, click **Save configuration to repository** on the **Configuration repository** tab in the publisher portal, and then issue the following command in your local repository.</span></span>

```
git pull
```

<span data-ttu-id="2686f-165">Innan du kör `git pull` så att du är i mappen för din lokala databas.</span><span class="sxs-lookup"><span data-stu-id="2686f-165">Before running `git pull` ensure that you are in the folder for your local repository.</span></span> <span data-ttu-id="2686f-166">Om du precis har slutfört den `git clone` kommando, och sedan måste du ändra katalogen till din lagringsplats genom att köra ett kommando som följande.</span><span class="sxs-lookup"><span data-stu-id="2686f-166">If you have just completed the `git clone` command, then you must change the directory to your repo by running a command like the following.</span></span>

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a><span data-ttu-id="2686f-167">Att skicka ändringar från din lokala lagringsplatsen till lagringsplatsen för server</span><span class="sxs-lookup"><span data-stu-id="2686f-167">To push changes from your local repo to the server repo</span></span>
<span data-ttu-id="2686f-168">För att skicka ändringar från din lokala databasen till server-databasen, måste du genomför ändringarna och skicka dem till server-databasen.</span><span class="sxs-lookup"><span data-stu-id="2686f-168">To push changes from your local repository to the server repository, you must commit your changes and then push them to the server repository.</span></span> <span data-ttu-id="2686f-169">Öppna kommandoradsverktyg för Git för att genomföra ändringarna, växla till katalogen för lokala databasen och utfärda följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="2686f-169">To commit your changes, open your Git command tool, switch to the directory of your local repository, and issue the following commands.</span></span>

```
git add --all
git commit -m "Description of your changes"
```

<span data-ttu-id="2686f-170">För att vidarebefordra alla incheckningar till servern, kör du följande kommando.</span><span class="sxs-lookup"><span data-stu-id="2686f-170">To push all of the commits to the server, run the following command.</span></span>

```
git push
```

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a><span data-ttu-id="2686f-171">Att distribuera konfigurationsändringar service till instansen för API Management-tjänsten</span><span class="sxs-lookup"><span data-stu-id="2686f-171">To deploy any service configuration changes to the API Management service instance</span></span>
<span data-ttu-id="2686f-172">När din lokala ändringar allokerat och pushas till server-databasen, kan du distribuera dem till din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="2686f-172">Once your local changes are committed and pushed to the server repository, you can deploy them to your API Management service instance.</span></span>

![Distribuera][api-management-configuration-deploy]

<span data-ttu-id="2686f-174">Information om hur du utför den här åtgärden med hjälp av REST-API finns [distribuera Git ändras till konfigurationsdatabasen med hjälp av REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span><span class="sxs-lookup"><span data-stu-id="2686f-174">For information on performing this operation using the REST API, see [Deploy Git changes to configuration database using the REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span></span>

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a><span data-ttu-id="2686f-175">Fil- och structure-referens för lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="2686f-175">File and folder structure reference of local Git repository</span></span>
<span data-ttu-id="2686f-176">Filer och mappar i den lokala git-lagringsplatsen innehåller konfigurationsinformation om tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="2686f-176">The files and folders in the local git repository contain the configuration information about the service instance.</span></span>

| <span data-ttu-id="2686f-177">Objekt</span><span class="sxs-lookup"><span data-stu-id="2686f-177">Item</span></span> | <span data-ttu-id="2686f-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2686f-178">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2686f-179">Rotmapp för api management</span><span class="sxs-lookup"><span data-stu-id="2686f-179">root api-management folder</span></span> |<span data-ttu-id="2686f-180">Innehåller översta konfiguration för tjänstinstansen</span><span class="sxs-lookup"><span data-stu-id="2686f-180">Contains top-level configuration for the service instance</span></span> |
| <span data-ttu-id="2686f-181">API: er för mappen</span><span class="sxs-lookup"><span data-stu-id="2686f-181">apis folder</span></span> |<span data-ttu-id="2686f-182">Innehåller konfiguration för API: er i instansen för tjänsten</span><span class="sxs-lookup"><span data-stu-id="2686f-182">Contains the configuration for the apis in the service instance</span></span> |
| <span data-ttu-id="2686f-183">mappen grupper</span><span class="sxs-lookup"><span data-stu-id="2686f-183">groups folder</span></span> |<span data-ttu-id="2686f-184">Innehåller konfiguration för grupper i tjänstinstansen</span><span class="sxs-lookup"><span data-stu-id="2686f-184">Contains the configuration for the groups in the service instance</span></span> |
| <span data-ttu-id="2686f-185">för principmappen</span><span class="sxs-lookup"><span data-stu-id="2686f-185">policies folder</span></span> |<span data-ttu-id="2686f-186">Innehåller principer i tjänstinstansen</span><span class="sxs-lookup"><span data-stu-id="2686f-186">Contains the policies in the service instance</span></span> |
| <span data-ttu-id="2686f-187">portalStyles mapp</span><span class="sxs-lookup"><span data-stu-id="2686f-187">portalStyles folder</span></span> |<span data-ttu-id="2686f-188">Innehåller konfiguration för developer portal anpassningar i tjänstinstansen</span><span class="sxs-lookup"><span data-stu-id="2686f-188">Contains the configuration for the developer portal customizations in the service instance</span></span> |
| <span data-ttu-id="2686f-189">produkter mapp</span><span class="sxs-lookup"><span data-stu-id="2686f-189">products folder</span></span> |<span data-ttu-id="2686f-190">Innehåller konfiguration för produkter i tjänstinstansen</span><span class="sxs-lookup"><span data-stu-id="2686f-190">Contains the configuration for the products in the service instance</span></span> |
| <span data-ttu-id="2686f-191">mallmappen</span><span class="sxs-lookup"><span data-stu-id="2686f-191">templates folder</span></span> |<span data-ttu-id="2686f-192">Innehåller konfiguration för e-mallarna i tjänstinstansen</span><span class="sxs-lookup"><span data-stu-id="2686f-192">Contains the configuration for the email templates in the service instance</span></span> |

<span data-ttu-id="2686f-193">Varje mapp kan innehålla en eller flera filer och i vissa fall en eller flera mappar, till exempel en mapp för varje API, produkt eller grupp.</span><span class="sxs-lookup"><span data-stu-id="2686f-193">Each folder can contain one or more files, and in some cases one or more folders, for example a folder for each API, product, or group.</span></span> <span data-ttu-id="2686f-194">Filerna i mappen är specifika för entitetstypen beskrivs av mappnamnet.</span><span class="sxs-lookup"><span data-stu-id="2686f-194">The files within each folder are specific for the entity type described by the folder name.</span></span>

| <span data-ttu-id="2686f-195">Filtyp</span><span class="sxs-lookup"><span data-stu-id="2686f-195">File type</span></span> | <span data-ttu-id="2686f-196">Syfte</span><span class="sxs-lookup"><span data-stu-id="2686f-196">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="2686f-197">JSON</span><span class="sxs-lookup"><span data-stu-id="2686f-197">json</span></span> |<span data-ttu-id="2686f-198">Konfigurationsinformation om respektive enhet</span><span class="sxs-lookup"><span data-stu-id="2686f-198">Configuration information about the respective entity</span></span> |
| <span data-ttu-id="2686f-199">HTML</span><span class="sxs-lookup"><span data-stu-id="2686f-199">html</span></span> |<span data-ttu-id="2686f-200">Beskrivningar av entiteten ofta visas i developer-portalen</span><span class="sxs-lookup"><span data-stu-id="2686f-200">Descriptions about the entity, often displayed in the developer portal</span></span> |
| <span data-ttu-id="2686f-201">xml</span><span class="sxs-lookup"><span data-stu-id="2686f-201">xml</span></span> |<span data-ttu-id="2686f-202">Principrapporter</span><span class="sxs-lookup"><span data-stu-id="2686f-202">Policy statements</span></span> |
| <span data-ttu-id="2686f-203">CSS</span><span class="sxs-lookup"><span data-stu-id="2686f-203">css</span></span> |<span data-ttu-id="2686f-204">Formatmallar för developer portal anpassning</span><span class="sxs-lookup"><span data-stu-id="2686f-204">Style sheets for developer portal customization</span></span> |

<span data-ttu-id="2686f-205">Dessa filer kan skapas, tas bort, redigera och hanteras inte i det lokala filsystemet och ändringarna distribueras tillbaka till den din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="2686f-205">These files can be created, deleted, edited, and managed on your local file system, and the changes deployed back to the your API Management service instance.</span></span>

> [!NOTE]
> <span data-ttu-id="2686f-206">Följande enheter finns inte i Git-lagringsplats och kan inte konfigureras med Git.</span><span class="sxs-lookup"><span data-stu-id="2686f-206">The following entities are not contained in the Git repository and cannot be configured using Git.</span></span>
> 
> * <span data-ttu-id="2686f-207">Användare</span><span class="sxs-lookup"><span data-stu-id="2686f-207">Users</span></span>
> * <span data-ttu-id="2686f-208">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="2686f-208">Subscriptions</span></span>
> * <span data-ttu-id="2686f-209">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="2686f-209">Properties</span></span>
> * <span data-ttu-id="2686f-210">Developer portal enheter än format</span><span class="sxs-lookup"><span data-stu-id="2686f-210">Developer portal entities other than styles</span></span>
> 
> 

### <a name="root-api-management-folder"></a><span data-ttu-id="2686f-211">Rotmapp för api management</span><span class="sxs-lookup"><span data-stu-id="2686f-211">Root api-management folder</span></span>
<span data-ttu-id="2686f-212">Roten `api-management` mappen innehåller en `configuration.json` -fil som innehåller översta information om instansen för tjänsten i följande format.</span><span class="sxs-lookup"><span data-stu-id="2686f-212">The root `api-management` folder contains a `configuration.json` file that contains top-level information about the service instance in the following format.</span></span>

```json
{
  "settings": {
    "RegistrationEnabled": "True",
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": "False",
    "UserRegistrationTermsConsentRequired": "False",
    "DelegationEnabled": "False",
    "DelegationUrl": "",
    "DelegatedSubscriptionEnabled": "False",
    "DelegationValidationKey": ""
  },
  "$ref-policy": "api-management/policies/global.xml"
}
```

<span data-ttu-id="2686f-213">De fyra första inställningarna (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, och `UserRegistrationTermsConsentRequired`) mappas till följande inställningar på den **identiteter** fliken i den **säkerhet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2686f-213">The first four settings (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, and `UserRegistrationTermsConsentRequired`) map to the following settings on the **Identities** tab in the **Security** section.</span></span>

| <span data-ttu-id="2686f-214">Identity-inställning</span><span class="sxs-lookup"><span data-stu-id="2686f-214">Identity setting</span></span> | <span data-ttu-id="2686f-215">Mappas till</span><span class="sxs-lookup"><span data-stu-id="2686f-215">Maps to</span></span> |
| --- | --- |
| <span data-ttu-id="2686f-216">RegistrationEnabled</span><span class="sxs-lookup"><span data-stu-id="2686f-216">RegistrationEnabled</span></span> |<span data-ttu-id="2686f-217">**Omdirigera anonyma användare till inloggningssidan** kryssruta</span><span class="sxs-lookup"><span data-stu-id="2686f-217">**Redirect anonymous users to sign-in page** checkbox</span></span> |
| <span data-ttu-id="2686f-218">UserRegistrationTerms</span><span class="sxs-lookup"><span data-stu-id="2686f-218">UserRegistrationTerms</span></span> |<span data-ttu-id="2686f-219">**Villkor för användning på användaren signup** textruta</span><span class="sxs-lookup"><span data-stu-id="2686f-219">**Terms of use on user signup** textbox</span></span> |
| <span data-ttu-id="2686f-220">UserRegistrationTermsEnabled</span><span class="sxs-lookup"><span data-stu-id="2686f-220">UserRegistrationTermsEnabled</span></span> |<span data-ttu-id="2686f-221">**Visa användningsvillkoren på inloggningssidan** kryssruta</span><span class="sxs-lookup"><span data-stu-id="2686f-221">**Show terms of use on signup page** checkbox</span></span> |
| <span data-ttu-id="2686f-222">UserRegistrationTermsConsentRequired</span><span class="sxs-lookup"><span data-stu-id="2686f-222">UserRegistrationTermsConsentRequired</span></span> |<span data-ttu-id="2686f-223">**Kräv godkännande** kryssruta</span><span class="sxs-lookup"><span data-stu-id="2686f-223">**Require consent** checkbox</span></span> |

![Identitetsinställningar][api-management-identity-settings]

<span data-ttu-id="2686f-225">Inställningarna för följande fyra (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, och `DelegationValidationKey`) mappas till följande inställningar på den **delegering** fliken i den **säkerhet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2686f-225">The next four settings (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, and `DelegationValidationKey`) map to the following settings on the **Delegation** tab in the **Security** section.</span></span>

| <span data-ttu-id="2686f-226">Delegeringsinställningen</span><span class="sxs-lookup"><span data-stu-id="2686f-226">Delegation setting</span></span> | <span data-ttu-id="2686f-227">Mappas till</span><span class="sxs-lookup"><span data-stu-id="2686f-227">Maps to</span></span> |
| --- | --- |
| <span data-ttu-id="2686f-228">DelegationEnabled</span><span class="sxs-lookup"><span data-stu-id="2686f-228">DelegationEnabled</span></span> |<span data-ttu-id="2686f-229">**Delegera inloggning & anmälan** kryssruta</span><span class="sxs-lookup"><span data-stu-id="2686f-229">**Delegate sign-in & sign-up** checkbox</span></span> |
| <span data-ttu-id="2686f-230">DelegationUrl</span><span class="sxs-lookup"><span data-stu-id="2686f-230">DelegationUrl</span></span> |<span data-ttu-id="2686f-231">**Delegering slutpunkts-URL** textruta</span><span class="sxs-lookup"><span data-stu-id="2686f-231">**Delegation endpoint URL** textbox</span></span> |
| <span data-ttu-id="2686f-232">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="2686f-232">DelegatedSubscriptionEnabled</span></span> |<span data-ttu-id="2686f-233">**Delegera produkten prenumeration** kryssruta</span><span class="sxs-lookup"><span data-stu-id="2686f-233">**Delegate product subscription** checkbox</span></span> |
| <span data-ttu-id="2686f-234">DelegationValidationKey</span><span class="sxs-lookup"><span data-stu-id="2686f-234">DelegationValidationKey</span></span> |<span data-ttu-id="2686f-235">**Delegera valideringsnyckel** textruta</span><span class="sxs-lookup"><span data-stu-id="2686f-235">**Delegate Validation Key** textbox</span></span> |

![Inställningarna för standarddelegering][api-management-delegation-settings]

<span data-ttu-id="2686f-237">Den sista inställningen `$ref-policy`, mappar till den globala principfilen instruktioner för tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="2686f-237">The final setting, `$ref-policy`, maps to the global policy statements file for the service instance.</span></span>

### <a name="apis-folder"></a><span data-ttu-id="2686f-238">API: er för mappen</span><span class="sxs-lookup"><span data-stu-id="2686f-238">apis folder</span></span>
<span data-ttu-id="2686f-239">Den `apis` mappen innehåller en mapp för varje API i tjänstinstansen som innehåller följande objekt.</span><span class="sxs-lookup"><span data-stu-id="2686f-239">The `apis` folder contains a folder for each API in the service instance which contains the following items.</span></span>

* <span data-ttu-id="2686f-240">`apis\<api name>\configuration.json`-Detta är konfigurationen för API: et och innehåller information om URL: en för backend-tjänsten och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2686f-240">`apis\<api name>\configuration.json` - this is the configuration for the API and contains information about the backend service URL and the operations.</span></span> <span data-ttu-id="2686f-241">Det här är samma information som returneras om du anropa [få en specifika API: et](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) med `export=true` i `application/json` format.</span><span class="sxs-lookup"><span data-stu-id="2686f-241">This is the same information that would be returned if you were to call [Get a specific API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) with `export=true` in `application/json` format.</span></span>
* <span data-ttu-id="2686f-242">`apis\<api name>\api.description.html`-Detta är en beskrivning av API: et och motsvarar den `description` -egenskapen för den [API entiteten](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="2686f-242">`apis\<api name>\api.description.html` - this is the description of the API and corresponds to the `description` property of the [API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span></span>
* <span data-ttu-id="2686f-243">`apis\<api name>\operations\`-den här mappen innehåller `<operation name>.description.html` filer som mappar till åtgärder i API: et.</span><span class="sxs-lookup"><span data-stu-id="2686f-243">`apis\<api name>\operations\` - this folder contains `<operation name>.description.html` files that map to the operations in the API.</span></span> <span data-ttu-id="2686f-244">Varje fil som innehåller en beskrivning av en enda åtgärd i API som mappar till den `description` -egenskapen för den [åtgärden entiteten](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) i REST-API.</span><span class="sxs-lookup"><span data-stu-id="2686f-244">Each file contains the description of a single operation in the API which maps to the `description` property of the [operation entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in the REST API.</span></span>

### <a name="groups-folder"></a><span data-ttu-id="2686f-245">mappen grupper</span><span class="sxs-lookup"><span data-stu-id="2686f-245">groups folder</span></span>
<span data-ttu-id="2686f-246">Den `groups` mappen innehåller en mapp för varje grupp som definierats i tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="2686f-246">The `groups` folder contains a folder for each group defined in the service instance.</span></span>

* <span data-ttu-id="2686f-247">`groups\<group name>\configuration.json`-Detta är konfigurationen för gruppen.</span><span class="sxs-lookup"><span data-stu-id="2686f-247">`groups\<group name>\configuration.json` - this is the configuration for the group.</span></span> <span data-ttu-id="2686f-248">Det här är samma information som returneras om du anropar den [få en specifik grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) igen.</span><span class="sxs-lookup"><span data-stu-id="2686f-248">This is the same information that would be returned if you were to call the [Get a specific group](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operation.</span></span>
* <span data-ttu-id="2686f-249">`groups\<group name>\description.html`-Detta är en beskrivning av gruppen som motsvarar den `description` -egenskapen för den [gruppen entiteten](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="2686f-249">`groups\<group name>\description.html` - this is the description of the group and corresponds to the `description` property of the [group entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span></span>

### <a name="policies-folder"></a><span data-ttu-id="2686f-250">för principmappen</span><span class="sxs-lookup"><span data-stu-id="2686f-250">policies folder</span></span>
<span data-ttu-id="2686f-251">Den `policies` mappen innehåller hanteringsprinciper för service-instans.</span><span class="sxs-lookup"><span data-stu-id="2686f-251">The `policies` folder contains the policy statements for your service instance.</span></span>

* <span data-ttu-id="2686f-252">`policies\global.xml`-innehåller principer som definierats vid globalt scope för service-instans.</span><span class="sxs-lookup"><span data-stu-id="2686f-252">`policies\global.xml` - contains policies defined at global scope for your service instance.</span></span>
* <span data-ttu-id="2686f-253">`policies\apis\<api name>\`-Om du har några definierade principer på API-scope, de ingår i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="2686f-253">`policies\apis\<api name>\` - if you have any policies defined at API scope, they are contained in this folder.</span></span>
* <span data-ttu-id="2686f-254">`policies\apis\<api name>\<operation name>\`mapp - om du har några definierade principer på åtgärdsomfånget de ingår i den här mappen i `<operation name>.xml` filer som mappas till principrapporter för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2686f-254">`policies\apis\<api name>\<operation name>\` folder - if you have any policies defined at operation scope, they are contained in this folder in `<operation name>.xml` files that map to the policy statements for each operation.</span></span>
* <span data-ttu-id="2686f-255">`policies\products\`-Om du har några principer som definierats i produktomfattningen de ingår i den här mappen som innehåller `<product name>.xml` filer som mappas till principrapporter för varje produkt.</span><span class="sxs-lookup"><span data-stu-id="2686f-255">`policies\products\` - if you have any policies defined at product scope, they are contained in this folder, which contains `<product name>.xml` files that map to the policy statements for each product.</span></span>

### <a name="portalstyles-folder"></a><span data-ttu-id="2686f-256">portalStyles mapp</span><span class="sxs-lookup"><span data-stu-id="2686f-256">portalStyles folder</span></span>
<span data-ttu-id="2686f-257">Den `portalStyles` mappen innehåller konfiguration och style sheets developer portal anpassningar för tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="2686f-257">The `portalStyles` folder contains configuration and style sheets for developer portal customizations for the service instance.</span></span>

* <span data-ttu-id="2686f-258">`portalStyles\configuration.json`-innehåller namnen på de formatmallar som används av developer-portalen</span><span class="sxs-lookup"><span data-stu-id="2686f-258">`portalStyles\configuration.json` - contains the names of the style sheets used by the developer portal</span></span>
* <span data-ttu-id="2686f-259">`portalStyles\<style name>.css`-varje `<style name>.css` filen innehåller format för developer-portalen (`Preview.css` och `Production.css` som standard).</span><span class="sxs-lookup"><span data-stu-id="2686f-259">`portalStyles\<style name>.css` - each `<style name>.css` file contains styles for the developer portal (`Preview.css` and `Production.css` by default).</span></span>

### <a name="products-folder"></a><span data-ttu-id="2686f-260">produkter mapp</span><span class="sxs-lookup"><span data-stu-id="2686f-260">products folder</span></span>
<span data-ttu-id="2686f-261">Den `products` mappen innehåller en mapp för varje produkt som definierats i tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="2686f-261">The `products` folder contains a folder for each product defined in the service instance.</span></span>

* <span data-ttu-id="2686f-262">`products\<product name>\configuration.json`-Detta är konfigurationen för produkten.</span><span class="sxs-lookup"><span data-stu-id="2686f-262">`products\<product name>\configuration.json` - this is the configuration for the product.</span></span> <span data-ttu-id="2686f-263">Det här är samma information som returneras om du anropar den [få en specifik produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) igen.</span><span class="sxs-lookup"><span data-stu-id="2686f-263">This is the same information that would be returned if you were to call the [Get a specific product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operation.</span></span>
* <span data-ttu-id="2686f-264">`products\<product name>\product.description.html`-Detta är en beskrivning av produkten som motsvarar den `description` -egenskapen för den [entiteten för produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) i REST-API.</span><span class="sxs-lookup"><span data-stu-id="2686f-264">`products\<product name>\product.description.html` - this is the description of the product and corresponds to the `description` property of the [product entity](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in the REST API.</span></span>

### <a name="templates"></a><span data-ttu-id="2686f-265">mallar</span><span class="sxs-lookup"><span data-stu-id="2686f-265">templates</span></span>
<span data-ttu-id="2686f-266">Den `templates` mappen innehåller konfigurationen för den [e-mallar](api-management-howto-configure-notifications.md) i tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="2686f-266">The `templates` folder contains configuration for the [email templates](api-management-howto-configure-notifications.md) of the service instance.</span></span>

* <span data-ttu-id="2686f-267">`<template name>\configuration.json`-Detta är konfigurationen för e-postmallen.</span><span class="sxs-lookup"><span data-stu-id="2686f-267">`<template name>\configuration.json` - this is the configuration for the email template.</span></span>
* <span data-ttu-id="2686f-268">`<template name>\body.html`-Detta är innehållet i e-postmallen.</span><span class="sxs-lookup"><span data-stu-id="2686f-268">`<template name>\body.html` - this is the body of the email template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2686f-269">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2686f-269">Next steps</span></span>
<span data-ttu-id="2686f-270">Information om andra sätt att hantera din tjänstinstansen finns:</span><span class="sxs-lookup"><span data-stu-id="2686f-270">For information on other ways to manage your service instance, see:</span></span>

* <span data-ttu-id="2686f-271">Hantera din service-instans med hjälp av följande PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="2686f-271">Manage your service instance using the following PowerShell cmdlets</span></span>
  * [<span data-ttu-id="2686f-272">Tjänstdistributionen PowerShell cmdlet-referens</span><span class="sxs-lookup"><span data-stu-id="2686f-272">Service deployment PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [<span data-ttu-id="2686f-273">Service management PowerShell-cmdlet-referens</span><span class="sxs-lookup"><span data-stu-id="2686f-273">Service management PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* <span data-ttu-id="2686f-274">Hantera service-instans i publisher-portal</span><span class="sxs-lookup"><span data-stu-id="2686f-274">Manage your service instance in the publisher portal</span></span>
  * [<span data-ttu-id="2686f-275">Hantera ditt första API</span><span class="sxs-lookup"><span data-stu-id="2686f-275">Manage your first API</span></span>](api-management-get-started.md)
* <span data-ttu-id="2686f-276">Hantera din service-instans med hjälp av REST-API</span><span class="sxs-lookup"><span data-stu-id="2686f-276">Manage your service instance using the REST API</span></span>
  * [<span data-ttu-id="2686f-277">API Management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="2686f-277">API Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="2686f-278">Titta på en videoöversikt</span><span class="sxs-lookup"><span data-stu-id="2686f-278">Watch a video overview</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Configuration-over-Git/player]
> 
> 

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




