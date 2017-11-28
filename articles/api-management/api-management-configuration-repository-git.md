---
title: "aaaConfigure din API Management-tjänsten med Git - Azure | Microsoft Docs"
description: "Lär dig hur toosave och konfigurera din API Management service configuration med Git."
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
ms.openlocfilehash: ef7d4c18f2ea3f5c9b86403349a83aef240f979b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosave-and-configure-your-api-management-service-configuration-using-git"></a><span data-ttu-id="0d429-103">Hur toosave och konfigurera din API Management service configuration med Git</span><span class="sxs-lookup"><span data-stu-id="0d429-103">How toosave and configure your API Management service configuration using Git</span></span>
> 
> 

<span data-ttu-id="0d429-104">Varje instans för API Management-tjänsten har en databas som innehåller information om hello konfiguration och metadata för hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="0d429-104">Each API Management service instance maintains a configuration database that contains information about hello configuration and metadata for hello service instance.</span></span> <span data-ttu-id="0d429-105">Ändringar kan göras toohello service-instans genom att ändra en inställning i hello publisher portal, med hjälp av en PowerShell-cmdlet eller göra ett REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="0d429-105">Changes can be made toohello service instance by changing a setting in hello publisher portal, using a PowerShell cmdlet, or making a REST API call.</span></span> <span data-ttu-id="0d429-106">Dessutom toothese metoder du kan också hantera konfigurationen för service-instans med Git, aktivera tjänsten hanteringsscenarier som:</span><span class="sxs-lookup"><span data-stu-id="0d429-106">In addition toothese methods, you can also manage your service instance configuration using Git, enabling service management scenarios such as:</span></span>

* <span data-ttu-id="0d429-107">Konfigurationen versionshantering - ladda ned och lagra olika versioner av tjänstkonfigurationen av</span><span class="sxs-lookup"><span data-stu-id="0d429-107">Configuration versioning - download and store different versions of your service configuration</span></span>
* <span data-ttu-id="0d429-108">Massredigera konfigurationsändringar – ändrar toomultiple delar av din tjänstkonfiguration i din lokala databas och integrera hello ändringar tillbaka toohello server med en enda åtgärd</span><span class="sxs-lookup"><span data-stu-id="0d429-108">Bulk configuration changes - make changes toomultiple parts of your service configuration in your local repository and integrate hello changes back toohello server with a single operation</span></span>
* <span data-ttu-id="0d429-109">Välkända Git toolchain och arbetsflöde – använda hello Git verktygsuppsättning och arbetsflöden som du redan är bekant med</span><span class="sxs-lookup"><span data-stu-id="0d429-109">Familiar Git toolchain and workflow - use hello Git tooling and workflows that you are already familiar with</span></span>

<span data-ttu-id="0d429-110">hello följande diagram visar en översikt över hello olika sätt tooconfigure din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="0d429-110">hello following diagram shows an overview of hello different ways tooconfigure your API Management service instance.</span></span>

![Konfigurera Git][api-management-git-configure]

<span data-ttu-id="0d429-112">När du gör ändringar tooyour tjänst med hello publisher portal, PowerShell-cmdlets eller hello REST API du hanterar din konfigurationsdatabas med hello `https://{name}.management.azure-api.net` slutpunkt, enligt hello höger på hello diagram.</span><span class="sxs-lookup"><span data-stu-id="0d429-112">When you make changes tooyour service using hello publisher portal, PowerShell cmdlets, or hello REST API, you are managing your service configuration database using hello `https://{name}.management.azure-api.net` endpoint, as shown on hello right side of hello diagram.</span></span> <span data-ttu-id="0d429-113">hello vänster sida av hello diagram illustrerar hur du kan hantera tjänstkonfigurationen av med Git och Git-lagringsplats för tjänsten finns på `https://{name}.scm.azure-api.net`.</span><span class="sxs-lookup"><span data-stu-id="0d429-113">hello left side of hello diagram illustrates how you can manage your service configuration using Git and Git repository for your service located at `https://{name}.scm.azure-api.net`.</span></span>

<span data-ttu-id="0d429-114">hello följande ger en översikt över hantering av din API Management service-instans med Git.</span><span class="sxs-lookup"><span data-stu-id="0d429-114">hello following steps provide an overview of managing your API Management service instance using Git.</span></span>

1. <span data-ttu-id="0d429-115">Konfiguration av Git i din tjänst</span><span class="sxs-lookup"><span data-stu-id="0d429-115">Access Git configuration in your service</span></span>
2. <span data-ttu-id="0d429-116">Spara service configuration database tooyour Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="0d429-116">Save your service configuration database tooyour Git repository</span></span>
3. <span data-ttu-id="0d429-117">Klona hello Git repo tooyour lokal dator</span><span class="sxs-lookup"><span data-stu-id="0d429-117">Clone hello Git repo tooyour local machine</span></span>
4. <span data-ttu-id="0d429-118">Hämta hello senaste lagringsplatsen ned tooyour lokala datorn och genomförande och push ändringar tillbaka tooyour lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="0d429-118">Pull hello latest repo down tooyour local machine, and commit and push changes back tooyour repo</span></span>
5. <span data-ttu-id="0d429-119">Distribuera hello ändringar från din lagringsplats i din konfigurationsdatabas</span><span class="sxs-lookup"><span data-stu-id="0d429-119">Deploy hello changes from your repo into your service configuration database</span></span>

<span data-ttu-id="0d429-120">Den här artikeln beskriver hur tooenable använder Git toomanage tjänstkonfigurationen av och innehåller en referens för hello filer och mappar i hello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="0d429-120">This article describes how tooenable and use Git toomanage your service configuration and provides a reference for hello files and folders in hello Git repository.</span></span>

## <a name="access-git-configuration-in-your-service"></a><span data-ttu-id="0d429-121">Konfiguration av Git i din tjänst</span><span class="sxs-lookup"><span data-stu-id="0d429-121">Access Git configuration in your service</span></span>
<span data-ttu-id="0d429-122">Du kan snabbt visa hello status för Git-konfiguration genom att visa hello Git ikon i hello övre högra hörnet av hello publisher portal.</span><span class="sxs-lookup"><span data-stu-id="0d429-122">You can quickly view hello status of your Git configuration by viewing hello Git icon in hello upper-right corner of hello publisher portal.</span></span> <span data-ttu-id="0d429-123">I det här exemplet hello statusmeddelande som anger att det finns ändringar som inte sparats toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="0d429-123">In this example, hello status message indicates that there are unsaved changes toohello repository.</span></span> <span data-ttu-id="0d429-124">Det beror på att konfigurationsdatabasen för hello API Management-tjänsten inte har ännu har sparats toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="0d429-124">This is because hello API Management service configuration database has not yet been saved toohello repository.</span></span>

![Git-status][api-management-git-icon-enable]

<span data-ttu-id="0d429-126">tooview och konfigurera inställningarna i Git, kan du antingen klicka hello Git ikonen eller klicka på hello **säkerhet** menyn och navigera toohello **Configuration databasen** fliken.</span><span class="sxs-lookup"><span data-stu-id="0d429-126">tooview and configure your Git configuration settings, you can either click hello Git icon, or click hello **Security** menu and navigate toohello **Configuration repository** tab.</span></span>

![Aktivera GIT][api-management-enable-git]

> [!IMPORTANT]
> <span data-ttu-id="0d429-128">Alla hemligheter som inte har definierats som egenskaper kommer att lagras i databasen hello och kommer att finnas kvar i historiken tills du inaktivera och aktivera Git-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0d429-128">Any secrets that are not defined as properties will be stored in hello repository and will remain in its history until you disable and re-enable Git access.</span></span> <span data-ttu-id="0d429-129">Egenskaper som innehåller en säker plats toomanage konstanta strängvärden, inklusive hemligheter, för alla API-konfiguration och principer, så du inte behöver toostore dem direkt i din principrapporter.</span><span class="sxs-lookup"><span data-stu-id="0d429-129">Properties provide a secure place toomanage constant string values, including secrets, across all API configuration and policies, so you don't have toostore them directly in your policy statements.</span></span> <span data-ttu-id="0d429-130">Mer information finns i [hur toouse egenskaper i Azure API Management-principer](api-management-howto-properties.md).</span><span class="sxs-lookup"><span data-stu-id="0d429-130">For more information, see [How toouse properties in Azure API Management policies](api-management-howto-properties.md).</span></span>
> 
> 

<span data-ttu-id="0d429-131">Mer information om att aktivera eller inaktivera Git-åtkomst med hjälp av hello REST-API finns [aktivera eller inaktivera Git-åtkomst med hjälp av hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span><span class="sxs-lookup"><span data-stu-id="0d429-131">For information on enabling or disabling Git access using hello REST API, see [Enable or disable Git access using hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).</span></span>

## <a name="toosave-hello-service-configuration-toohello-git-repository"></a><span data-ttu-id="0d429-132">toosave hello service configuration toohello Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="0d429-132">toosave hello service configuration toohello Git repository</span></span>
<span data-ttu-id="0d429-133">hello första steget innan kloning hello databasen är toosave hello aktuell status för hello service configuration toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="0d429-133">hello first step before cloning hello repository is toosave hello current state of hello service configuration toohello repository.</span></span> <span data-ttu-id="0d429-134">Klicka på **spara konfigurationen toorepository**.</span><span class="sxs-lookup"><span data-stu-id="0d429-134">Click **Save configuration toorepository**.</span></span>

![Spara konfigurationen][api-management-save-configuration]

<span data-ttu-id="0d429-136">Gör eventuella ändringar på hello bekräftelseskärm och klicka på **Ok** toosave.</span><span class="sxs-lookup"><span data-stu-id="0d429-136">Make any desired changes on hello confirmation screen and click **Ok** toosave.</span></span>

![Spara konfigurationen][api-management-save-configuration-confirm]

<span data-ttu-id="0d429-138">Efter en liten stund hello konfiguration sparas och hello Konfigurationsstatus för hello lagringsplats visas, inklusive hello datum och tidpunkt för senaste ändring av hello och hello senaste synkronisering mellan hello tjänstkonfiguration och hello databasen.</span><span class="sxs-lookup"><span data-stu-id="0d429-138">After a few moments hello configuration is saved, and hello configuration status of hello repository is displayed, including hello date and time of hello last configuration change and hello last synchronization between hello service configuration and hello repository.</span></span>

![Konfigurationsstatus][api-management-configuration-status]

<span data-ttu-id="0d429-140">När hello konfiguration sparas toohello databasen kan klonas den.</span><span class="sxs-lookup"><span data-stu-id="0d429-140">Once hello configuration is saved toohello repository, it can be cloned.</span></span>

<span data-ttu-id="0d429-141">Information om hur du utför den här åtgärden med hjälp av hello REST-API finns [Commit-konfiguration med verktyget hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span><span class="sxs-lookup"><span data-stu-id="0d429-141">For information on performing this operation using hello REST API, see [Commit configuration snapshot using hello REST API](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).</span></span>

## <a name="tooclone-hello-repository-tooyour-local-machine"></a><span data-ttu-id="0d429-142">tooclone hello databasen tooyour lokal dator</span><span class="sxs-lookup"><span data-stu-id="0d429-142">tooclone hello repository tooyour local machine</span></span>
<span data-ttu-id="0d429-143">tooclone en databas måste hello URL tooyour databasen, ett användarnamn och ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="0d429-143">tooclone a repository, you need hello URL tooyour repository, a user name, and a password.</span></span> <span data-ttu-id="0d429-144">hello användarnamn och URL visas hello övre delen av hello **Configuration databasen** fliken.</span><span class="sxs-lookup"><span data-stu-id="0d429-144">hello user name and URL are displayed near hello top of hello **Configuration repository** tab.</span></span>

![Git-klonen][api-management-configuration-git-clone]

<span data-ttu-id="0d429-146">hello lösenord genereras längst hello hello **Configuration databasen** fliken.</span><span class="sxs-lookup"><span data-stu-id="0d429-146">hello password is generated at hello bottom of hello **Configuration repository** tab.</span></span>

![Generera lösenord][api-management-generate-password]

<span data-ttu-id="0d429-148">toogenerate ett lösenord, kontrollera först att hello **upphör att gälla** är korrekt toohello önskad utgångsdatum och utgångstid och klicka sedan på **generera Token**.</span><span class="sxs-lookup"><span data-stu-id="0d429-148">toogenerate a password, first ensure that hello **Expiry** is set toohello desired expiration date and time, and then click **Generate Token**.</span></span>

![Lösenord][api-management-password]

> [!IMPORTANT]
> <span data-ttu-id="0d429-150">Anteckna det här lösenordet.</span><span class="sxs-lookup"><span data-stu-id="0d429-150">Make a note of this password.</span></span> <span data-ttu-id="0d429-151">När du lämnar kommer den här sidan hello lösenord inte att visas igen.</span><span class="sxs-lookup"><span data-stu-id="0d429-151">Once you leave this page hello password will not be displayed again.</span></span>
> 
> 

<span data-ttu-id="0d429-152">följande exempel används hello Git Bash hello verktyget från [Git för Windows](http://www.git-scm.com/downloads) men du kan använda alla Git-verktyg som du är bekant med.</span><span class="sxs-lookup"><span data-stu-id="0d429-152">hello following examples use hello Git Bash tool from [Git for Windows](http://www.git-scm.com/downloads) but you can use any Git tool that you are familiar with.</span></span>

<span data-ttu-id="0d429-153">Öppna Git-verktyget i hello önskade mappen och kör hello efter kommandot tooclone hello git-lagringsplatsen tooyour lokala datorn, med hello-kommando som tillhandahålls av hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="0d429-153">Open your Git tool in hello desired folder and run hello following command tooclone hello git repository tooyour local machine, using hello command provided by hello publisher portal.</span></span>

```
git clone https://bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="0d429-154">Ange hello användarnamn och lösenord när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="0d429-154">Provide hello user name and password when prompted.</span></span>

<span data-ttu-id="0d429-155">Om du får inga fel, försök att ändra din `git clone` kommandot tooinclude hello användarnamn och lösenord, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="0d429-155">If you receive any errors, try modifying your `git clone` command tooinclude hello user name and password, as shown in hello following example.</span></span>

```
git clone https://username:password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="0d429-156">Försök URL kodning hello delar av hello kommandot lösenord om det innehåller ett fel.</span><span class="sxs-lookup"><span data-stu-id="0d429-156">If this provides an error, try URL encoding hello password portion of hello command.</span></span> <span data-ttu-id="0d429-157">Ett snabbt sätt toodo är tooopen Visual Studio och problemet hello följande kommando i hello **kommandofönstret**.</span><span class="sxs-lookup"><span data-stu-id="0d429-157">One quick way toodo this is tooopen Visual Studio, and issue hello following command in hello **Immediate Window**.</span></span> <span data-ttu-id="0d429-158">tooopen hello **kommandofönstret**, öppna någon lösning eller ett projekt i Visual Studio (eller skapa en ny tom konsolapp), och välj **Windows**, **Immediate** från Hej **felsöka** menyn.</span><span class="sxs-lookup"><span data-stu-id="0d429-158">tooopen hello **Immediate Window**, open any solution or project in Visual Studio (or create a new empty console application), and choose **Windows**, **Immediate** from hello **Debug** menu.</span></span>

```
?System.NetWebUtility.UrlEncode("password from publisher portal")
```

<span data-ttu-id="0d429-159">Använda hello kodade lösenordet tillsammans med ditt namn och databasen plats tooconstruct hello git användarkommando.</span><span class="sxs-lookup"><span data-stu-id="0d429-159">Use hello encoded password along with your user name and repository location tooconstruct hello git command.</span></span>

```
git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/
```

<span data-ttu-id="0d429-160">Du kan visa och arbeta med det i det lokala filsystemet när hello databasen klonas.</span><span class="sxs-lookup"><span data-stu-id="0d429-160">Once hello repository is cloned you can view and work with it in your local file system.</span></span> <span data-ttu-id="0d429-161">Mer information finns i [fil- och struktur referens för lokal Git-lagringsplats](#file-and-folder-structure-reference-of-local-git-repository).</span><span class="sxs-lookup"><span data-stu-id="0d429-161">For more information, see [File and folder structure reference of local Git repository](#file-and-folder-structure-reference-of-local-git-repository).</span></span>

## <a name="tooupdate-your-local-repository-with-hello-most-current-service-instance-configuration"></a><span data-ttu-id="0d429-162">tooupdate lokala databasen med hello aktuell instans tjänstkonfiguration</span><span class="sxs-lookup"><span data-stu-id="0d429-162">tooupdate your local repository with hello most current service instance configuration</span></span>
<span data-ttu-id="0d429-163">Om du gör ändringar tooyour API Management service-instans i hello publisher portalen eller med hjälp av hello REST-API, måste du spara dessa ändringar toohello databasen innan du kan uppdatera din lokal databas med hello senaste ändringarna.</span><span class="sxs-lookup"><span data-stu-id="0d429-163">If you make changes tooyour API Management service instance in hello publisher portal or using hello REST API, you must save these changes toohello repository before you can update your local repository with hello latest changes.</span></span> <span data-ttu-id="0d429-164">toodo, genom att välja **spara konfigurationen toorepository** på hello **Configuration databasen** i hello publisher portal och utfärda hello följande kommando i en lokal databas.</span><span class="sxs-lookup"><span data-stu-id="0d429-164">toodo this, click **Save configuration toorepository** on hello **Configuration repository** tab in hello publisher portal, and then issue hello following command in your local repository.</span></span>

```
git pull
```

<span data-ttu-id="0d429-165">Innan du kör `git pull` så att du är i hello-mappen för din lokala databas.</span><span class="sxs-lookup"><span data-stu-id="0d429-165">Before running `git pull` ensure that you are in hello folder for your local repository.</span></span> <span data-ttu-id="0d429-166">Om du har precis slutfört hello `git clone` kommandot, du måste ändra lagringsplatsen för hello directory tooyour genom att köra ett kommando som hello nedan.</span><span class="sxs-lookup"><span data-stu-id="0d429-166">If you have just completed hello `git clone` command, then you must change hello directory tooyour repo by running a command like hello following.</span></span>

```
cd bugbashdev4.scm.azure-api.net/
```

## <a name="toopush-changes-from-your-local-repo-toohello-server-repo"></a><span data-ttu-id="0d429-167">toopush ändras från din lokala lagringsplatsen toohello server lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="0d429-167">toopush changes from your local repo toohello server repo</span></span>
<span data-ttu-id="0d429-168">toopush ändras från din lokala toohello server databasen måste du spara ändringarna och sedan push-installera dem toohello server-databasen.</span><span class="sxs-lookup"><span data-stu-id="0d429-168">toopush changes from your local repository toohello server repository, you must commit your changes and then push them toohello server repository.</span></span> <span data-ttu-id="0d429-169">toocommit ändringarna, öppna ditt Git-kommandoradsverktyg, växel toohello katalog med din lokala databasen och problemet hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="0d429-169">toocommit your changes, open your Git command tool, switch toohello directory of your local repository, and issue hello following commands.</span></span>

```
git add --all
git commit -m "Description of your changes"
```

<span data-ttu-id="0d429-170">toopush alla hello genomför toohello servern, kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="0d429-170">toopush all of hello commits toohello server, run hello following command.</span></span>

```
git push
```

## <a name="toodeploy-any-service-configuration-changes-toohello-api-management-service-instance"></a><span data-ttu-id="0d429-171">toodeploy alla tjänstens konfiguration ändringar toohello API Management tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-171">toodeploy any service configuration changes toohello API Management service instance</span></span>
<span data-ttu-id="0d429-172">När din lokala ändringar är bekräftade och intryckt toohello server-databasen kan distribuera du dem tooyour API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="0d429-172">Once your local changes are committed and pushed toohello server repository, you can deploy them tooyour API Management service instance.</span></span>

![Distribuera][api-management-configuration-deploy]

<span data-ttu-id="0d429-174">Information om hur du utför den här åtgärden med hjälp av hello REST-API finns [distribuera Git ändras tooconfiguration databasen med hjälp av hello REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0d429-174">For information on performing this operation using hello REST API, see [Deploy Git changes tooconfiguration database using hello REST API](https://docs.microsoft.com/en-us/rest/api/apimanagement/tenantconfiguration).</span></span>

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a><span data-ttu-id="0d429-175">Fil- och structure-referens för lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="0d429-175">File and folder structure reference of local Git repository</span></span>
<span data-ttu-id="0d429-176">hello innehåller filer och mappar i hello lokal git-lagringsplatsen hello konfigurationsinformation om hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="0d429-176">hello files and folders in hello local git repository contain hello configuration information about hello service instance.</span></span>

| <span data-ttu-id="0d429-177">Objekt</span><span class="sxs-lookup"><span data-stu-id="0d429-177">Item</span></span> | <span data-ttu-id="0d429-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0d429-178">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0d429-179">Rotmapp för api management</span><span class="sxs-lookup"><span data-stu-id="0d429-179">root api-management folder</span></span> |<span data-ttu-id="0d429-180">Innehåller översta konfiguration för hello tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-180">Contains top-level configuration for hello service instance</span></span> |
| <span data-ttu-id="0d429-181">API: er för mappen</span><span class="sxs-lookup"><span data-stu-id="0d429-181">apis folder</span></span> |<span data-ttu-id="0d429-182">Innehåller hello konfiguration för hello API: er i hello tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-182">Contains hello configuration for hello apis in hello service instance</span></span> |
| <span data-ttu-id="0d429-183">mappen grupper</span><span class="sxs-lookup"><span data-stu-id="0d429-183">groups folder</span></span> |<span data-ttu-id="0d429-184">Innehåller hello konfiguration för hello grupper i hello tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-184">Contains hello configuration for hello groups in hello service instance</span></span> |
| <span data-ttu-id="0d429-185">för principmappen</span><span class="sxs-lookup"><span data-stu-id="0d429-185">policies folder</span></span> |<span data-ttu-id="0d429-186">Innehåller hello principer i hello tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-186">Contains hello policies in hello service instance</span></span> |
| <span data-ttu-id="0d429-187">portalStyles mapp</span><span class="sxs-lookup"><span data-stu-id="0d429-187">portalStyles folder</span></span> |<span data-ttu-id="0d429-188">Innehåller hello konfiguration för hello developer portal anpassningar i hello tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-188">Contains hello configuration for hello developer portal customizations in hello service instance</span></span> |
| <span data-ttu-id="0d429-189">produkter mapp</span><span class="sxs-lookup"><span data-stu-id="0d429-189">products folder</span></span> |<span data-ttu-id="0d429-190">Innehåller hello konfiguration för hello produkter i hello tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-190">Contains hello configuration for hello products in hello service instance</span></span> |
| <span data-ttu-id="0d429-191">mallmappen</span><span class="sxs-lookup"><span data-stu-id="0d429-191">templates folder</span></span> |<span data-ttu-id="0d429-192">Innehåller hello konfiguration för hello e-mallar i hello tjänstinstans</span><span class="sxs-lookup"><span data-stu-id="0d429-192">Contains hello configuration for hello email templates in hello service instance</span></span> |

<span data-ttu-id="0d429-193">Varje mapp kan innehålla en eller flera filer och i vissa fall en eller flera mappar, till exempel en mapp för varje API, produkt eller grupp.</span><span class="sxs-lookup"><span data-stu-id="0d429-193">Each folder can contain one or more files, and in some cases one or more folders, for example a folder for each API, product, or group.</span></span> <span data-ttu-id="0d429-194">hello-filer i mappen är specifika för hello entitetstypen beskrivs av hello mappnamn.</span><span class="sxs-lookup"><span data-stu-id="0d429-194">hello files within each folder are specific for hello entity type described by hello folder name.</span></span>

| <span data-ttu-id="0d429-195">Filtyp</span><span class="sxs-lookup"><span data-stu-id="0d429-195">File type</span></span> | <span data-ttu-id="0d429-196">Syfte</span><span class="sxs-lookup"><span data-stu-id="0d429-196">Purpose</span></span> |
| --- | --- |
| <span data-ttu-id="0d429-197">JSON</span><span class="sxs-lookup"><span data-stu-id="0d429-197">json</span></span> |<span data-ttu-id="0d429-198">Konfigurationsinformation om hello respektive enhet</span><span class="sxs-lookup"><span data-stu-id="0d429-198">Configuration information about hello respective entity</span></span> |
| <span data-ttu-id="0d429-199">HTML</span><span class="sxs-lookup"><span data-stu-id="0d429-199">html</span></span> |<span data-ttu-id="0d429-200">Beskrivningar om hello-enhet, ofta visas i hello developer-portalen</span><span class="sxs-lookup"><span data-stu-id="0d429-200">Descriptions about hello entity, often displayed in hello developer portal</span></span> |
| <span data-ttu-id="0d429-201">xml</span><span class="sxs-lookup"><span data-stu-id="0d429-201">xml</span></span> |<span data-ttu-id="0d429-202">Principrapporter</span><span class="sxs-lookup"><span data-stu-id="0d429-202">Policy statements</span></span> |
| <span data-ttu-id="0d429-203">CSS</span><span class="sxs-lookup"><span data-stu-id="0d429-203">css</span></span> |<span data-ttu-id="0d429-204">Formatmallar för developer portal anpassning</span><span class="sxs-lookup"><span data-stu-id="0d429-204">Style sheets for developer portal customization</span></span> |

<span data-ttu-id="0d429-205">Dessa filer kan skapas, tas bort, redigera och hanteras inte i det lokala filsystemet och hello ändringarna distribueras tillbaka toohello din API Management service-instans.</span><span class="sxs-lookup"><span data-stu-id="0d429-205">These files can be created, deleted, edited, and managed on your local file system, and hello changes deployed back toohello your API Management service instance.</span></span>

> [!NOTE]
> <span data-ttu-id="0d429-206">hello följande enheter finns inte i hello Git-lagringsplats och kan inte konfigureras med Git.</span><span class="sxs-lookup"><span data-stu-id="0d429-206">hello following entities are not contained in hello Git repository and cannot be configured using Git.</span></span>
> 
> * <span data-ttu-id="0d429-207">Användare</span><span class="sxs-lookup"><span data-stu-id="0d429-207">Users</span></span>
> * <span data-ttu-id="0d429-208">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0d429-208">Subscriptions</span></span>
> * <span data-ttu-id="0d429-209">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="0d429-209">Properties</span></span>
> * <span data-ttu-id="0d429-210">Developer portal enheter än format</span><span class="sxs-lookup"><span data-stu-id="0d429-210">Developer portal entities other than styles</span></span>
> 
> 

### <a name="root-api-management-folder"></a><span data-ttu-id="0d429-211">Rotmapp för api management</span><span class="sxs-lookup"><span data-stu-id="0d429-211">Root api-management folder</span></span>
<span data-ttu-id="0d429-212">hello rot `api-management` mappen innehåller en `configuration.json` -fil som innehåller översta information om hello tjänstinstansen i hello följande format.</span><span class="sxs-lookup"><span data-stu-id="0d429-212">hello root `api-management` folder contains a `configuration.json` file that contains top-level information about hello service instance in hello following format.</span></span>

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

<span data-ttu-id="0d429-213">Hej första fyra inställningar (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, och `UserRegistrationTermsConsentRequired`) mappa toohello följande inställningar på hello **identiteter** fliken i hello **säkerhet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0d429-213">hello first four settings (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, and `UserRegistrationTermsConsentRequired`) map toohello following settings on hello **Identities** tab in hello **Security** section.</span></span>

| <span data-ttu-id="0d429-214">Identity-inställning</span><span class="sxs-lookup"><span data-stu-id="0d429-214">Identity setting</span></span> | <span data-ttu-id="0d429-215">Mappar för</span><span class="sxs-lookup"><span data-stu-id="0d429-215">Maps too</span></span>|
| --- | --- |
| <span data-ttu-id="0d429-216">RegistrationEnabled</span><span class="sxs-lookup"><span data-stu-id="0d429-216">RegistrationEnabled</span></span> |<span data-ttu-id="0d429-217">**Omdirigeringssida anonyma användare toosign i** kryssruta</span><span class="sxs-lookup"><span data-stu-id="0d429-217">**Redirect anonymous users toosign-in page** checkbox</span></span> |
| <span data-ttu-id="0d429-218">UserRegistrationTerms</span><span class="sxs-lookup"><span data-stu-id="0d429-218">UserRegistrationTerms</span></span> |<span data-ttu-id="0d429-219">**Villkor för användning på användaren signup** textruta</span><span class="sxs-lookup"><span data-stu-id="0d429-219">**Terms of use on user signup** textbox</span></span> |
| <span data-ttu-id="0d429-220">UserRegistrationTermsEnabled</span><span class="sxs-lookup"><span data-stu-id="0d429-220">UserRegistrationTermsEnabled</span></span> |<span data-ttu-id="0d429-221">**Visa användningsvillkoren på inloggningssidan** kryssruta</span><span class="sxs-lookup"><span data-stu-id="0d429-221">**Show terms of use on signup page** checkbox</span></span> |
| <span data-ttu-id="0d429-222">UserRegistrationTermsConsentRequired</span><span class="sxs-lookup"><span data-stu-id="0d429-222">UserRegistrationTermsConsentRequired</span></span> |<span data-ttu-id="0d429-223">**Kräv godkännande** kryssruta</span><span class="sxs-lookup"><span data-stu-id="0d429-223">**Require consent** checkbox</span></span> |

![Identitetsinställningar][api-management-identity-settings]

<span data-ttu-id="0d429-225">Hej följande fyra inställningar (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, och `DelegationValidationKey`) mappa toohello följande inställningar på hello **delegering** fliken i hello **säkerhet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0d429-225">hello next four settings (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, and `DelegationValidationKey`) map toohello following settings on hello **Delegation** tab in hello **Security** section.</span></span>

| <span data-ttu-id="0d429-226">Delegeringsinställningen</span><span class="sxs-lookup"><span data-stu-id="0d429-226">Delegation setting</span></span> | <span data-ttu-id="0d429-227">Mappar för</span><span class="sxs-lookup"><span data-stu-id="0d429-227">Maps too</span></span>|
| --- | --- |
| <span data-ttu-id="0d429-228">DelegationEnabled</span><span class="sxs-lookup"><span data-stu-id="0d429-228">DelegationEnabled</span></span> |<span data-ttu-id="0d429-229">**Delegera inloggning & anmälan** kryssruta</span><span class="sxs-lookup"><span data-stu-id="0d429-229">**Delegate sign-in & sign-up** checkbox</span></span> |
| <span data-ttu-id="0d429-230">DelegationUrl</span><span class="sxs-lookup"><span data-stu-id="0d429-230">DelegationUrl</span></span> |<span data-ttu-id="0d429-231">**Delegering slutpunkts-URL** textruta</span><span class="sxs-lookup"><span data-stu-id="0d429-231">**Delegation endpoint URL** textbox</span></span> |
| <span data-ttu-id="0d429-232">DelegatedSubscriptionEnabled</span><span class="sxs-lookup"><span data-stu-id="0d429-232">DelegatedSubscriptionEnabled</span></span> |<span data-ttu-id="0d429-233">**Delegera produkten prenumeration** kryssruta</span><span class="sxs-lookup"><span data-stu-id="0d429-233">**Delegate product subscription** checkbox</span></span> |
| <span data-ttu-id="0d429-234">DelegationValidationKey</span><span class="sxs-lookup"><span data-stu-id="0d429-234">DelegationValidationKey</span></span> |<span data-ttu-id="0d429-235">**Delegera valideringsnyckel** textruta</span><span class="sxs-lookup"><span data-stu-id="0d429-235">**Delegate Validation Key** textbox</span></span> |

![Inställningarna för standarddelegering][api-management-delegation-settings]

<span data-ttu-id="0d429-237">Hej sista inställningen `$ref-policy`, mappar toohello globala instruktioner principfil för hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="0d429-237">hello final setting, `$ref-policy`, maps toohello global policy statements file for hello service instance.</span></span>

### <a name="apis-folder"></a><span data-ttu-id="0d429-238">API: er för mappen</span><span class="sxs-lookup"><span data-stu-id="0d429-238">apis folder</span></span>
<span data-ttu-id="0d429-239">Hej `apis` mappen innehåller en mapp för varje API i hello service-instans som innehåller hello följande objekt.</span><span class="sxs-lookup"><span data-stu-id="0d429-239">hello `apis` folder contains a folder for each API in hello service instance which contains hello following items.</span></span>

* <span data-ttu-id="0d429-240">`apis\<api name>\configuration.json`-Detta är hello konfigurationen för hello API och innehåller information om hello backend-tjänstens URL och hello åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0d429-240">`apis\<api name>\configuration.json` - this is hello configuration for hello API and contains information about hello backend service URL and hello operations.</span></span> <span data-ttu-id="0d429-241">Detta är hello samma information som returneras om du toocall [få en specifika API: et](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) med `export=true` i `application/json` format.</span><span class="sxs-lookup"><span data-stu-id="0d429-241">This is hello same information that would be returned if you were toocall [Get a specific API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) with `export=true` in `application/json` format.</span></span>
* <span data-ttu-id="0d429-242">`apis\<api name>\api.description.html`-Detta är hello beskrivning av hello API som motsvarar toohello `description` -egenskapen för hello [API entiteten](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="0d429-242">`apis\<api name>\api.description.html` - this is hello description of hello API and corresponds toohello `description` property of hello [API entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).</span></span>
* <span data-ttu-id="0d429-243">`apis\<api name>\operations\`-den här mappen innehåller `<operation name>.description.html` filer som mappas toohello åtgärder i hello API.</span><span class="sxs-lookup"><span data-stu-id="0d429-243">`apis\<api name>\operations\` - this folder contains `<operation name>.description.html` files that map toohello operations in hello API.</span></span> <span data-ttu-id="0d429-244">Varje fil innehåller hello beskrivning av en enda åtgärd i hello API som mappar toohello `description` -egenskapen för hello [åtgärden entiteten](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) i hello REST API.</span><span class="sxs-lookup"><span data-stu-id="0d429-244">Each file contains hello description of a single operation in hello API which maps toohello `description` property of hello [operation entity](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) in hello REST API.</span></span>

### <a name="groups-folder"></a><span data-ttu-id="0d429-245">mappen grupper</span><span class="sxs-lookup"><span data-stu-id="0d429-245">groups folder</span></span>
<span data-ttu-id="0d429-246">Hej `groups` mappen innehåller en mapp för varje grupp som definierats i hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="0d429-246">hello `groups` folder contains a folder for each group defined in hello service instance.</span></span>

* <span data-ttu-id="0d429-247">`groups\<group name>\configuration.json`-Detta är hello konfiguration för hello grupp.</span><span class="sxs-lookup"><span data-stu-id="0d429-247">`groups\<group name>\configuration.json` - this is hello configuration for hello group.</span></span> <span data-ttu-id="0d429-248">Detta är hello samma information som returneras om du toocall hello [få en specifik grupp](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) igen.</span><span class="sxs-lookup"><span data-stu-id="0d429-248">This is hello same information that would be returned if you were toocall hello [Get a specific group](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) operation.</span></span>
* <span data-ttu-id="0d429-249">`groups\<group name>\description.html`-Detta är hello beskrivning av hello grupp som motsvarar toohello `description` -egenskapen för hello [gruppen entiteten](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span><span class="sxs-lookup"><span data-stu-id="0d429-249">`groups\<group name>\description.html` - this is hello description of hello group and corresponds toohello `description` property of hello [group entity](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).</span></span>

### <a name="policies-folder"></a><span data-ttu-id="0d429-250">för principmappen</span><span class="sxs-lookup"><span data-stu-id="0d429-250">policies folder</span></span>
<span data-ttu-id="0d429-251">Hej `policies` mappen innehåller hello hanteringsprinciper för service-instans.</span><span class="sxs-lookup"><span data-stu-id="0d429-251">hello `policies` folder contains hello policy statements for your service instance.</span></span>

* <span data-ttu-id="0d429-252">`policies\global.xml`-innehåller principer som definierats vid globalt scope för service-instans.</span><span class="sxs-lookup"><span data-stu-id="0d429-252">`policies\global.xml` - contains policies defined at global scope for your service instance.</span></span>
* <span data-ttu-id="0d429-253">`policies\apis\<api name>\`-Om du har några definierade principer på API-scope, de ingår i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="0d429-253">`policies\apis\<api name>\` - if you have any policies defined at API scope, they are contained in this folder.</span></span>
* <span data-ttu-id="0d429-254">`policies\apis\<api name>\<operation name>\`mapp - om du har några definierade principer på åtgärdsomfånget de ingår i den här mappen i `<operation name>.xml` filer som mappas toohello principrapporter för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="0d429-254">`policies\apis\<api name>\<operation name>\` folder - if you have any policies defined at operation scope, they are contained in this folder in `<operation name>.xml` files that map toohello policy statements for each operation.</span></span>
* <span data-ttu-id="0d429-255">`policies\products\`-Om du har några principer som definierats i produktomfattningen de ingår i den här mappen som innehåller `<product name>.xml` filer som mappas toohello hanteringsprinciper för varje produkt.</span><span class="sxs-lookup"><span data-stu-id="0d429-255">`policies\products\` - if you have any policies defined at product scope, they are contained in this folder, which contains `<product name>.xml` files that map toohello policy statements for each product.</span></span>

### <a name="portalstyles-folder"></a><span data-ttu-id="0d429-256">portalStyles mapp</span><span class="sxs-lookup"><span data-stu-id="0d429-256">portalStyles folder</span></span>
<span data-ttu-id="0d429-257">Hej `portalStyles` mappen innehåller konfigurations- och blad för developer portal anpassningar för hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="0d429-257">hello `portalStyles` folder contains configuration and style sheets for developer portal customizations for hello service instance.</span></span>

* <span data-ttu-id="0d429-258">`portalStyles\configuration.json`-innehåller hello namnen på hello formatmallar som används av hello developer-portalen</span><span class="sxs-lookup"><span data-stu-id="0d429-258">`portalStyles\configuration.json` - contains hello names of hello style sheets used by hello developer portal</span></span>
* <span data-ttu-id="0d429-259">`portalStyles\<style name>.css`-varje `<style name>.css` filen innehåller format för hello developer-portalen (`Preview.css` och `Production.css` som standard).</span><span class="sxs-lookup"><span data-stu-id="0d429-259">`portalStyles\<style name>.css` - each `<style name>.css` file contains styles for hello developer portal (`Preview.css` and `Production.css` by default).</span></span>

### <a name="products-folder"></a><span data-ttu-id="0d429-260">produkter mapp</span><span class="sxs-lookup"><span data-stu-id="0d429-260">products folder</span></span>
<span data-ttu-id="0d429-261">Hej `products` mappen innehåller en mapp för varje produkt som definierats i hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="0d429-261">hello `products` folder contains a folder for each product defined in hello service instance.</span></span>

* <span data-ttu-id="0d429-262">`products\<product name>\configuration.json`-Detta är hello konfiguration för hello produkten.</span><span class="sxs-lookup"><span data-stu-id="0d429-262">`products\<product name>\configuration.json` - this is hello configuration for hello product.</span></span> <span data-ttu-id="0d429-263">Detta är hello samma information som returneras om du toocall hello [få en specifik produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) igen.</span><span class="sxs-lookup"><span data-stu-id="0d429-263">This is hello same information that would be returned if you were toocall hello [Get a specific product](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) operation.</span></span>
* <span data-ttu-id="0d429-264">`products\<product name>\product.description.html`-Detta är hello beskrivning av hello produkt som motsvarar toohello `description` -egenskapen för hello [entiteten för produkt](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) i hello REST API.</span><span class="sxs-lookup"><span data-stu-id="0d429-264">`products\<product name>\product.description.html` - this is hello description of hello product and corresponds toohello `description` property of hello [product entity](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) in hello REST API.</span></span>

### <a name="templates"></a><span data-ttu-id="0d429-265">mallar</span><span class="sxs-lookup"><span data-stu-id="0d429-265">templates</span></span>
<span data-ttu-id="0d429-266">Hej `templates` mappen innehåller konfiguration för hello [e-mallar](api-management-howto-configure-notifications.md) av hello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="0d429-266">hello `templates` folder contains configuration for hello [email templates](api-management-howto-configure-notifications.md) of hello service instance.</span></span>

* <span data-ttu-id="0d429-267">`<template name>\configuration.json`-Detta är hello konfiguration för hello e-postmall.</span><span class="sxs-lookup"><span data-stu-id="0d429-267">`<template name>\configuration.json` - this is hello configuration for hello email template.</span></span>
* <span data-ttu-id="0d429-268">`<template name>\body.html`-Detta är hello texten för hello e-postmallen.</span><span class="sxs-lookup"><span data-stu-id="0d429-268">`<template name>\body.html` - this is hello body of hello email template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d429-269">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d429-269">Next steps</span></span>
<span data-ttu-id="0d429-270">Mer information om andra sätt toomanage service-instans finns:</span><span class="sxs-lookup"><span data-stu-id="0d429-270">For information on other ways toomanage your service instance, see:</span></span>

* <span data-ttu-id="0d429-271">Hantera din service-instans med hello följande PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="0d429-271">Manage your service instance using hello following PowerShell cmdlets</span></span>
  * [<span data-ttu-id="0d429-272">Tjänstdistributionen PowerShell cmdlet-referens</span><span class="sxs-lookup"><span data-stu-id="0d429-272">Service deployment PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt619282.aspx)
  * [<span data-ttu-id="0d429-273">Service management PowerShell-cmdlet-referens</span><span class="sxs-lookup"><span data-stu-id="0d429-273">Service management PowerShell cmdlet reference</span></span>](https://msdn.microsoft.com/library/azure/mt613507.aspx)
* <span data-ttu-id="0d429-274">Hantera service-instans i hello publisher portal</span><span class="sxs-lookup"><span data-stu-id="0d429-274">Manage your service instance in hello publisher portal</span></span>
  * [<span data-ttu-id="0d429-275">Hantera ditt första API</span><span class="sxs-lookup"><span data-stu-id="0d429-275">Manage your first API</span></span>](api-management-get-started.md)
* <span data-ttu-id="0d429-276">Hantera din service-instans med hello REST API</span><span class="sxs-lookup"><span data-stu-id="0d429-276">Manage your service instance using hello REST API</span></span>
  * [<span data-ttu-id="0d429-277">API Management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="0d429-277">API Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="0d429-278">Titta på en videoöversikt</span><span class="sxs-lookup"><span data-stu-id="0d429-278">Watch a video overview</span></span>
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




