---
title: Samla in Linux programprestanda i OMS Log Analytics | Microsoft Docs
description: "Den här artikeln innehåller information för att konfigurera OMS-Agent för Linux för att samla in prestandaräknare för MySQL och Apache HTTP-servern."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 04ea6f728e59ec8b47e54fe45e1adc6cbbfb85ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-performance-counters-for-linux-applications-in-log-analytics"></a><span data-ttu-id="b219c-103">Samla in prestandaräknare för Linux-program i logganalys</span><span class="sxs-lookup"><span data-stu-id="b219c-103">Collect performance counters for Linux applications in Log Analytics</span></span> 
<span data-ttu-id="b219c-104">Den här artikeln innehåller information om hur du konfigurerar den [OMS-Agent för Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) att samla in prestandaräknare för specifika program.</span><span class="sxs-lookup"><span data-stu-id="b219c-104">This article provides details for configuring the [OMS Agent for Linux](https://github.com/Microsoft/OMS-Agent-for-Linux) to collect performance counters for specific applications.</span></span>  <span data-ttu-id="b219c-105">Program som ingår i den här artikeln är:</span><span class="sxs-lookup"><span data-stu-id="b219c-105">The applications included in this article are:</span></span>  

- [<span data-ttu-id="b219c-106">MySQL</span><span class="sxs-lookup"><span data-stu-id="b219c-106">MySQL</span></span>](#MySQL)
- [<span data-ttu-id="b219c-107">Apache HTTP-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-107">Apache HTTP Server</span></span>](#apache-http-server)

## <a name="mysql"></a><span data-ttu-id="b219c-108">MySQL</span><span class="sxs-lookup"><span data-stu-id="b219c-108">MySQL</span></span>
<span data-ttu-id="b219c-109">Om MySQL-Server eller MariaDB Server har identifierats på datorn när OMS-agenten är installerad, installeras en provider för MySQL-Server för prestandaövervakning automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b219c-109">If MySQL Server or MariaDB Server is detected on the computer when the OMS agent is installed, a performance monitoring provider for MySQL Server will be automatically installed.</span></span> <span data-ttu-id="b219c-110">Den här providern ansluter till den lokala MySQL/MariaDB servern att exponera prestandastatistik.</span><span class="sxs-lookup"><span data-stu-id="b219c-110">This provider connects to the local MySQL/MariaDB server to expose performance statistics.</span></span> <span data-ttu-id="b219c-111">MySQL-autentiseringsuppgifter måste konfigureras så att providern har åtkomst till MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b219c-111">MySQL user credentials must be configured so that the provider can access the MySQL Server.</span></span>

### <a name="configure-mysql-credentials"></a><span data-ttu-id="b219c-112">Konfigurera MySQL-autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="b219c-112">Configure MySQL credentials</span></span>
<span data-ttu-id="b219c-113">MySQL OMI-providern kräver att en förkonfigurerad MySQL-användare och installerat MySQL-klientbibliotek för att fråga efter prestanda och hälsoinformation från MySQL-instans.</span><span class="sxs-lookup"><span data-stu-id="b219c-113">The MySQL OMI provider requires a preconfigured MySQL user and installed MySQL client libraries in order to query the performance and health information from the MySQL instance.</span></span>  <span data-ttu-id="b219c-114">Dessa autentiseringsuppgifter lagras i en fil för autentisering som är lagrad på Linux-agenten.</span><span class="sxs-lookup"><span data-stu-id="b219c-114">These credentials are stored in an authentication file that's stored on the Linux agent.</span></span>  <span data-ttu-id="b219c-115">Autentiseringsfilen anger vilka bind-adress och port MySQL-instans lyssnar på och vilka autentiseringsuppgifter du använder för att samla in mått.</span><span class="sxs-lookup"><span data-stu-id="b219c-115">The authentication file specifies what bind-address and port the MySQL instance is listening on and what credentials to use to gather metrics.</span></span>  

<span data-ttu-id="b219c-116">Under installationen av OMS-Agent för Linux MySQL OMI providern genomsöker MySQL my.cnf configuration-filer (standardplatserna) för bind-adress och port och delvis anger MySQL OMI autentisering.</span><span class="sxs-lookup"><span data-stu-id="b219c-116">During installation of the OMS Agent for Linux the MySQL OMI provider will scan MySQL my.cnf configuration files (default locations) for bind-address and port and partially set the MySQL OMI authentication file.</span></span>

<span data-ttu-id="b219c-117">MySQL-autentisering lagras på `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.</span><span class="sxs-lookup"><span data-stu-id="b219c-117">The MySQL authentication file is stored at `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.</span></span>


### <a name="authentication-file-format"></a><span data-ttu-id="b219c-118">Filformatet för autentisering</span><span class="sxs-lookup"><span data-stu-id="b219c-118">Authentication file format</span></span>
<span data-ttu-id="b219c-119">Följande är formatet för filen MySQL OMI-autentisering</span><span class="sxs-lookup"><span data-stu-id="b219c-119">Following is the format for the MySQL OMI authentication file</span></span>

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

<span data-ttu-id="b219c-120">Poster i autentiseringsfilen beskrivs i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="b219c-120">The entries in the authentication file are described in the following table.</span></span>

| <span data-ttu-id="b219c-121">Egenskap</span><span class="sxs-lookup"><span data-stu-id="b219c-121">Property</span></span> | <span data-ttu-id="b219c-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b219c-122">Description</span></span> |
|:--|:--|
| <span data-ttu-id="b219c-123">Port</span><span class="sxs-lookup"><span data-stu-id="b219c-123">Port</span></span> | <span data-ttu-id="b219c-124">Representerar den aktuella porten MySQL-instans lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="b219c-124">Represents the current port the MySQL instance is listening on.</span></span> <span data-ttu-id="b219c-125">Port 0 anger att egenskaperna för följande används för standardinstansen.</span><span class="sxs-lookup"><span data-stu-id="b219c-125">Port 0 specifies that the properties following are used for default instance.</span></span> |
| <span data-ttu-id="b219c-126">Bind-adress</span><span class="sxs-lookup"><span data-stu-id="b219c-126">Bind-Address</span></span>| <span data-ttu-id="b219c-127">Aktuell MySQL bind-adress.</span><span class="sxs-lookup"><span data-stu-id="b219c-127">Current MySQL bind-address.</span></span> |
| <span data-ttu-id="b219c-128">användarnamn</span><span class="sxs-lookup"><span data-stu-id="b219c-128">username</span></span>| <span data-ttu-id="b219c-129">MySQL-användare som används för att använda för att övervaka MySQL-serverinstansen.</span><span class="sxs-lookup"><span data-stu-id="b219c-129">MySQL user used to use to monitor the MySQL server instance.</span></span> |
| <span data-ttu-id="b219c-130">Base64-kodat lösenord</span><span class="sxs-lookup"><span data-stu-id="b219c-130">Base64 encoded Password</span></span>| <span data-ttu-id="b219c-131">Lösenordet för MySQL övervakning användaren kodad i Base64.</span><span class="sxs-lookup"><span data-stu-id="b219c-131">Password of the MySQL monitoring user encoded in Base64.</span></span> |
| <span data-ttu-id="b219c-132">Automatisk uppdatering</span><span class="sxs-lookup"><span data-stu-id="b219c-132">AutoUpdate</span></span>| <span data-ttu-id="b219c-133">Anger om genomsökning efter ändringar i filen my.cnf och skriva över filen MySQL OMI autentisering när MySQL OMI providern uppgraderas.</span><span class="sxs-lookup"><span data-stu-id="b219c-133">Specifies whether to rescan for changes in the my.cnf file and overwrite the MySQL OMI Authentication file when the MySQL OMI Provider is upgraded.</span></span> |

### <a name="default-instance"></a><span data-ttu-id="b219c-134">Standardinstansen</span><span class="sxs-lookup"><span data-stu-id="b219c-134">Default instance</span></span>
<span data-ttu-id="b219c-135">Filen MySQL OMI-autentisering kan definiera en standard-instans och port kod för att hantera flera MySQL-instanser på en Linux-värd enklare.</span><span class="sxs-lookup"><span data-stu-id="b219c-135">The MySQL OMI authentication file can define a default instance and port number to make managing multiple MySQL instances on one Linux host easier.</span></span>  <span data-ttu-id="b219c-136">Standardinstansen markeras med en instans med port 0.</span><span class="sxs-lookup"><span data-stu-id="b219c-136">The default instance is denoted by an instance with port 0.</span></span> <span data-ttu-id="b219c-137">Alla ytterligare instanser ärver egenskaper som anges från standardinstansen om de ange olika värden.</span><span class="sxs-lookup"><span data-stu-id="b219c-137">All additional instances will inherit properties set from the default instance unless they specify different values.</span></span> <span data-ttu-id="b219c-138">Till exempel om MySQL-instans som lyssnar på port '3308' läggs används standardinstansen bind-adress, användarnamn och lösenord för Base64-kodade att testa och övervaka den instans som lyssnar på 3308.</span><span class="sxs-lookup"><span data-stu-id="b219c-138">For example, if MySQL instance listening on port ‘3308’ is added, the default instance’s bind-address, username, and Base64 encoded password will be used to try and monitor the instance listening on 3308.</span></span> <span data-ttu-id="b219c-139">Om instansen på 3308 är bunden till en annan adress och använder samma MySQL användarnamn och lösenord par behövs bara bind-adress och andra egenskaper ärvs.</span><span class="sxs-lookup"><span data-stu-id="b219c-139">If the instance on 3308 is bound to another address and uses the same MySQL username and password pair only the bind-address is needed, and the other properties will be inherited.</span></span>

<span data-ttu-id="b219c-140">I följande tabell har exempel instans inställningar</span><span class="sxs-lookup"><span data-stu-id="b219c-140">The following table has example instance settings</span></span> 

| <span data-ttu-id="b219c-141">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b219c-141">Description</span></span> | <span data-ttu-id="b219c-142">Fil</span><span class="sxs-lookup"><span data-stu-id="b219c-142">File</span></span> |
|:--|:--|
| <span data-ttu-id="b219c-143">Standardinstansen och instansen med port 3308.</span><span class="sxs-lookup"><span data-stu-id="b219c-143">Default instance and instance with port 3308.</span></span> | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| <span data-ttu-id="b219c-144">Standardinstansen och instansen med port 3308 och annat användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b219c-144">Default instance and instance with port 3308 and different user name and password.</span></span> | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a><span data-ttu-id="b219c-145">Programmet för MySQL OMI autentisering fil</span><span class="sxs-lookup"><span data-stu-id="b219c-145">MySQL OMI Authentication File Program</span></span>
<span data-ttu-id="b219c-146">Ingår i installationen av MySQL OMI-providern är ett MySQL OMI autentisering filen program som kan användas för att redigera filen MySQL OMI-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b219c-146">Included with the installation of the MySQL OMI provider is a MySQL OMI authentication file program which can be used to edit the MySQL OMI Authentication file.</span></span> <span data-ttu-id="b219c-147">Programmet för autentisering-filen finns på följande plats.</span><span class="sxs-lookup"><span data-stu-id="b219c-147">The authentication file program can be found at the following location.</span></span>

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> <span data-ttu-id="b219c-148">Filen autentiseringsuppgifter måste kunna läsas av kontot omsagent.</span><span class="sxs-lookup"><span data-stu-id="b219c-148">The credentials file must be readable by the omsagent account.</span></span> <span data-ttu-id="b219c-149">Du bör köra kommandot mycimprovauth som omsgent.</span><span class="sxs-lookup"><span data-stu-id="b219c-149">Running the mycimprovauth command as omsgent is recommended.</span></span>

<span data-ttu-id="b219c-150">Följande tabell innehåller information om syntaxen för att använda mycimprovauth.</span><span class="sxs-lookup"><span data-stu-id="b219c-150">The following table provides details on the syntax for using mycimprovauth.</span></span>

| <span data-ttu-id="b219c-151">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="b219c-151">Operation</span></span> | <span data-ttu-id="b219c-152">Exempel</span><span class="sxs-lookup"><span data-stu-id="b219c-152">Example</span></span> | <span data-ttu-id="b219c-153">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b219c-153">Description</span></span>
|:--|:--|:--|
| <span data-ttu-id="b219c-154">automatisk uppdatering * false\\</span><span class="sxs-lookup"><span data-stu-id="b219c-154">autoupdate *false\\</span></span>|<span data-ttu-id="b219c-155">true *</span><span class="sxs-lookup"><span data-stu-id="b219c-155">true*</span></span> | <span data-ttu-id="b219c-156">mycimprovauth automatisk uppdatering false</span><span class="sxs-lookup"><span data-stu-id="b219c-156">mycimprovauth autoupdate false</span></span> | <span data-ttu-id="b219c-157">Anger huruvida autentiseringsfilen uppdateras automatiskt på Starta om eller uppdatera.</span><span class="sxs-lookup"><span data-stu-id="b219c-157">Sets whether or not the authentication file will be automatically updated on restart or update.</span></span> |
| <span data-ttu-id="b219c-158">standard *användarlösenordet för bind-adress*</span><span class="sxs-lookup"><span data-stu-id="b219c-158">default *bind-address username password*</span></span> | <span data-ttu-id="b219c-159">mycimprovauth standard 127.0.0.1 rot pwd</span><span class="sxs-lookup"><span data-stu-id="b219c-159">mycimprovauth default 127.0.0.1 root pwd</span></span> | <span data-ttu-id="b219c-160">Anger standardinstansen i MySQL OMI autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="b219c-160">Sets the default instance in the MySQL OMI authentication file.</span></span><br><span data-ttu-id="b219c-161">Lösenordsfältet ska anges i klartext - lösenordet i filen MySQL OMI autentisering blir Base64-kodade.</span><span class="sxs-lookup"><span data-stu-id="b219c-161">The password field should be entered in plain text - the password in the MySQL OMI authentication file will be Base 64 encoded.</span></span> |
| <span data-ttu-id="b219c-162">ta bort * default\\</span><span class="sxs-lookup"><span data-stu-id="b219c-162">delete *default\\</span></span>|<span data-ttu-id="b219c-163">portnummer *</span><span class="sxs-lookup"><span data-stu-id="b219c-163">port_num*</span></span> | <span data-ttu-id="b219c-164">mycimprovauth 3308</span><span class="sxs-lookup"><span data-stu-id="b219c-164">mycimprovauth 3308</span></span> | <span data-ttu-id="b219c-165">Tar bort den angivna instansen som antingen standard eller av portnummer.</span><span class="sxs-lookup"><span data-stu-id="b219c-165">Deletes the specified instance by either default or by port number.</span></span> |
| <span data-ttu-id="b219c-166">Hjälp</span><span class="sxs-lookup"><span data-stu-id="b219c-166">help</span></span> | <span data-ttu-id="b219c-167">mycimprov hjälp</span><span class="sxs-lookup"><span data-stu-id="b219c-167">mycimprov help</span></span> | <span data-ttu-id="b219c-168">Visar en lista med kommandon för att använda.</span><span class="sxs-lookup"><span data-stu-id="b219c-168">Prints out a list of commands to use.</span></span> |
| <span data-ttu-id="b219c-169">Skriv ut</span><span class="sxs-lookup"><span data-stu-id="b219c-169">print</span></span> | <span data-ttu-id="b219c-170">mycimprov utskrift</span><span class="sxs-lookup"><span data-stu-id="b219c-170">mycimprov print</span></span> | <span data-ttu-id="b219c-171">Visar en lättläst MySQL OMI autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="b219c-171">Prints out an easy to read MySQL OMI authentication file.</span></span> |
| <span data-ttu-id="b219c-172">Uppdatera portnummer *användarlösenordet för bind-adress*</span><span class="sxs-lookup"><span data-stu-id="b219c-172">update port_num *bind-address username password*</span></span> | <span data-ttu-id="b219c-173">mycimprov uppdatering 3307 127.0.0.1 rot pwd</span><span class="sxs-lookup"><span data-stu-id="b219c-173">mycimprov update 3307 127.0.0.1 root pwd</span></span> | <span data-ttu-id="b219c-174">Uppdaterar den angivna instansen eller lägger till instansen om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="b219c-174">Updates the specified instance or adds the instance if it does not exist.</span></span> |

<span data-ttu-id="b219c-175">Följande exempelkommandon definiera ett standard-användarkonto för MySQL-server på localhost.</span><span class="sxs-lookup"><span data-stu-id="b219c-175">The following example commands define a default user account for the MySQL server on localhost.</span></span>  <span data-ttu-id="b219c-176">Lösenordsfältet ska anges i klartext - lösenord i filen MySQL OMI autentisering kommer att Base64-kodade</span><span class="sxs-lookup"><span data-stu-id="b219c-176">The password field should be entered in plain text - the password in the MySQL OMI authentication file will be Base 64 encoded</span></span>

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a><span data-ttu-id="b219c-177">Databasbehörigheter som krävs för MySQL-prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="b219c-177">Database Permissions Required for MySQL Performance Counters</span></span>
<span data-ttu-id="b219c-178">MySQL-användare behöver åtkomst till följande frågor för att samla in prestandadata för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b219c-178">The MySQL User requires access to the following queries to collect MySQL Server performance data.</span></span> 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


<span data-ttu-id="b219c-179">MySQL-användaren kräver också väljer åtkomst till standard i tabellerna nedan.</span><span class="sxs-lookup"><span data-stu-id="b219c-179">The MySQL user also requires SELECT access to the following default tables.</span></span>

- <span data-ttu-id="b219c-180">INFORMATION_SCHEMA</span><span class="sxs-lookup"><span data-stu-id="b219c-180">information_schema</span></span>
- <span data-ttu-id="b219c-181">MySQL.</span><span class="sxs-lookup"><span data-stu-id="b219c-181">mysql.</span></span> 

<span data-ttu-id="b219c-182">Dessa behörigheter kan tilldelas genom att köra följande kommandon för bevilja.</span><span class="sxs-lookup"><span data-stu-id="b219c-182">These privileges can be granted by running the following grant commands.</span></span>

    GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;


> [!NOTE]
> <span data-ttu-id="b219c-183">Om du vill ge behörighet till en MySQL måste övervakning användaren att bevilja användaren ha behörigheten ' GRANT Option, samt behörigheten beviljas.</span><span class="sxs-lookup"><span data-stu-id="b219c-183">To grant permissions to a MySQL monitoring user the granting user must have the ‘GRANT option’ privilege as well as the privilege being granted.</span></span>

### <a name="define-performance-counters"></a><span data-ttu-id="b219c-184">Definiera prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="b219c-184">Define performance counters</span></span>

<span data-ttu-id="b219c-185">När du konfigurerar OMS-Agent för Linux för att skicka data till logganalys, måste du konfigurera prestandaräknarna som samlar in.</span><span class="sxs-lookup"><span data-stu-id="b219c-185">Once you configure the OMS Agent for Linux to send data to Log Analytics, you must configure the performance counters to collect.</span></span>  <span data-ttu-id="b219c-186">Använd proceduren i [Windows och Linux prestanda datakällor i logganalys](log-analytics-data-sources-windows-events.md) med räknarna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="b219c-186">Use the procedure in [Windows and Linux performance data sources in Log Analytics](log-analytics-data-sources-windows-events.md) with the counters in the following table.</span></span>

| <span data-ttu-id="b219c-187">Objektnamn</span><span class="sxs-lookup"><span data-stu-id="b219c-187">Object Name</span></span> | <span data-ttu-id="b219c-188">Räknarens namn</span><span class="sxs-lookup"><span data-stu-id="b219c-188">Counter Name</span></span> |
|:--|:--|
| <span data-ttu-id="b219c-189">MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="b219c-189">MySQL Database</span></span> | <span data-ttu-id="b219c-190">Ledigt diskutrymme i byte</span><span class="sxs-lookup"><span data-stu-id="b219c-190">Disk Space in Bytes</span></span> |
| <span data-ttu-id="b219c-191">MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="b219c-191">MySQL Database</span></span> | <span data-ttu-id="b219c-192">Tabeller</span><span class="sxs-lookup"><span data-stu-id="b219c-192">Tables</span></span> |
| <span data-ttu-id="b219c-193">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-193">MySQL Server</span></span> | <span data-ttu-id="b219c-194">Avbrutna anslutning Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-194">Aborted Connection Pct</span></span> |
| <span data-ttu-id="b219c-195">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-195">MySQL Server</span></span> | <span data-ttu-id="b219c-196">Anslutningen används Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-196">Connection Use Pct</span></span> |
| <span data-ttu-id="b219c-197">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-197">MySQL Server</span></span> | <span data-ttu-id="b219c-198">Användning av diskutrymme i byte</span><span class="sxs-lookup"><span data-stu-id="b219c-198">Disk Space Use in Bytes</span></span> |
| <span data-ttu-id="b219c-199">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-199">MySQL Server</span></span> | <span data-ttu-id="b219c-200">Fullständiga genomsökning Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-200">Full Table Scan Pct</span></span> |
| <span data-ttu-id="b219c-201">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-201">MySQL Server</span></span> | <span data-ttu-id="b219c-202">InnoDB buffertpool träffar Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-202">InnoDB Buffer Pool Hit Pct</span></span> |
| <span data-ttu-id="b219c-203">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-203">MySQL Server</span></span> | <span data-ttu-id="b219c-204">InnoDB Pool Använd Buffertprocent</span><span class="sxs-lookup"><span data-stu-id="b219c-204">InnoDB Buffer Pool Use Pct</span></span> |
| <span data-ttu-id="b219c-205">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-205">MySQL Server</span></span> | <span data-ttu-id="b219c-206">InnoDB Pool Använd Buffertprocent</span><span class="sxs-lookup"><span data-stu-id="b219c-206">InnoDB Buffer Pool Use Pct</span></span> |
| <span data-ttu-id="b219c-207">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-207">MySQL Server</span></span> | <span data-ttu-id="b219c-208">Viktiga träffar Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-208">Key Cache Hit Pct</span></span> |
| <span data-ttu-id="b219c-209">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-209">MySQL Server</span></span> | <span data-ttu-id="b219c-210">Viktiga Cache används Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-210">Key Cache Use Pct</span></span> |
| <span data-ttu-id="b219c-211">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-211">MySQL Server</span></span> | <span data-ttu-id="b219c-212">Viktiga Cache skrivåtgärder Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-212">Key Cache Write Pct</span></span> |
| <span data-ttu-id="b219c-213">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-213">MySQL Server</span></span> | <span data-ttu-id="b219c-214">Frågan Cache träffar Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-214">Query Cache Hit Pct</span></span> |
| <span data-ttu-id="b219c-215">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-215">MySQL Server</span></span> | <span data-ttu-id="b219c-216">Frågan Cache Prunes Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-216">Query Cache Prunes Pct</span></span> |
| <span data-ttu-id="b219c-217">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-217">MySQL Server</span></span> | <span data-ttu-id="b219c-218">Frågan Cache används Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-218">Query Cache Use Pct</span></span> |
| <span data-ttu-id="b219c-219">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-219">MySQL Server</span></span> | <span data-ttu-id="b219c-220">Tabell träffar Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-220">Table Cache Hit Pct</span></span> |
| <span data-ttu-id="b219c-221">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-221">MySQL Server</span></span> | <span data-ttu-id="b219c-222">Tabell Cache används Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-222">Table Cache Use Pct</span></span> |
| <span data-ttu-id="b219c-223">MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-223">MySQL Server</span></span> | <span data-ttu-id="b219c-224">Tabell Lås konkurrens Pct</span><span class="sxs-lookup"><span data-stu-id="b219c-224">Table Lock Contention Pct</span></span> |

## <a name="apache-http-server"></a><span data-ttu-id="b219c-225">Apache HTTP-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-225">Apache HTTP Server</span></span> 
<span data-ttu-id="b219c-226">Om Apache HTTP-Server har identifierats på datorn när omsagent-paket installeras, installeras en provider för Apache HTTP-Server för prestandaövervakning automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b219c-226">If Apache HTTP Server is detected on the computer when the omsagent bundle is installed, a performance monitoring provider for Apache HTTP Server will be automatically installed.</span></span> <span data-ttu-id="b219c-227">Den här providern förlitar sig på en Apache-modulen måste läsas in i Apache HTTP-Server för att komma åt prestandadata.</span><span class="sxs-lookup"><span data-stu-id="b219c-227">This provider relies on an Apache module that must be loaded into the Apache HTTP Server in order to access performance data.</span></span> <span data-ttu-id="b219c-228">Går att läsa in modulen med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b219c-228">The module can be loaded with the following command:</span></span>
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

<span data-ttu-id="b219c-229">Om du vill ta bort modulen Apache övervakning, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b219c-229">To unload the Apache monitoring module, run the following command:</span></span>
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a><span data-ttu-id="b219c-230">Definiera prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="b219c-230">Define performance counters</span></span>

<span data-ttu-id="b219c-231">När du konfigurerar OMS-Agent för Linux för att skicka data till logganalys, måste du konfigurera prestandaräknarna som samlar in.</span><span class="sxs-lookup"><span data-stu-id="b219c-231">Once you configure the OMS Agent for Linux to send data to Log Analytics, you must configure the performance counters to collect.</span></span>  <span data-ttu-id="b219c-232">Använd proceduren i [Windows och Linux prestanda datakällor i logganalys](log-analytics-data-sources-windows-events.md) med räknarna i följande tabell.</span><span class="sxs-lookup"><span data-stu-id="b219c-232">Use the procedure in [Windows and Linux performance data sources in Log Analytics](log-analytics-data-sources-windows-events.md) with the counters in the following table.</span></span>

| <span data-ttu-id="b219c-233">Objektnamn</span><span class="sxs-lookup"><span data-stu-id="b219c-233">Object Name</span></span> | <span data-ttu-id="b219c-234">Räknarens namn</span><span class="sxs-lookup"><span data-stu-id="b219c-234">Counter Name</span></span> |
|:--|:--|
| <span data-ttu-id="b219c-235">Apache HTTP-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-235">Apache HTTP Server</span></span> | <span data-ttu-id="b219c-236">Upptagen arbetare</span><span class="sxs-lookup"><span data-stu-id="b219c-236">Busy Workers</span></span> |
| <span data-ttu-id="b219c-237">Apache HTTP-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-237">Apache HTTP Server</span></span> | <span data-ttu-id="b219c-238">Inaktiv arbetare</span><span class="sxs-lookup"><span data-stu-id="b219c-238">Idle Workers</span></span> |
| <span data-ttu-id="b219c-239">Apache HTTP-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-239">Apache HTTP Server</span></span> | <span data-ttu-id="b219c-240">PCT upptagen arbetare</span><span class="sxs-lookup"><span data-stu-id="b219c-240">Pct Busy Workers</span></span> |
| <span data-ttu-id="b219c-241">Apache HTTP-Server</span><span class="sxs-lookup"><span data-stu-id="b219c-241">Apache HTTP Server</span></span> | <span data-ttu-id="b219c-242">Totalt antal Pct CPU</span><span class="sxs-lookup"><span data-stu-id="b219c-242">Total Pct CPU</span></span> |
| <span data-ttu-id="b219c-243">Apache virtuell värddator</span><span class="sxs-lookup"><span data-stu-id="b219c-243">Apache Virtual Host</span></span> | <span data-ttu-id="b219c-244">Fel per minut - klient</span><span class="sxs-lookup"><span data-stu-id="b219c-244">Errors per Minute - Client</span></span> |
| <span data-ttu-id="b219c-245">Apache virtuell värddator</span><span class="sxs-lookup"><span data-stu-id="b219c-245">Apache Virtual Host</span></span> | <span data-ttu-id="b219c-246">Fel per minut - Server</span><span class="sxs-lookup"><span data-stu-id="b219c-246">Errors per Minute - Server</span></span> |
| <span data-ttu-id="b219c-247">Apache virtuell värddator</span><span class="sxs-lookup"><span data-stu-id="b219c-247">Apache Virtual Host</span></span> | <span data-ttu-id="b219c-248">KB per begäran</span><span class="sxs-lookup"><span data-stu-id="b219c-248">KB per Request</span></span> |
| <span data-ttu-id="b219c-249">Apache virtuell värddator</span><span class="sxs-lookup"><span data-stu-id="b219c-249">Apache Virtual Host</span></span> | <span data-ttu-id="b219c-250">Begäranden KB per sekund</span><span class="sxs-lookup"><span data-stu-id="b219c-250">Requests KB per Second</span></span> |
| <span data-ttu-id="b219c-251">Apache virtuell värddator</span><span class="sxs-lookup"><span data-stu-id="b219c-251">Apache Virtual Host</span></span> | <span data-ttu-id="b219c-252">Begäranden per sekund</span><span class="sxs-lookup"><span data-stu-id="b219c-252">Requests per Second</span></span> |



## <a name="next-steps"></a><span data-ttu-id="b219c-253">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b219c-253">Next steps</span></span>
* <span data-ttu-id="b219c-254">[Samla in prestandaräknare](log-analytics-data-sources-performance-counters.md) från Linux-agenter.</span><span class="sxs-lookup"><span data-stu-id="b219c-254">[Collect performance counters](log-analytics-data-sources-performance-counters.md) from Linux agents.</span></span>
* <span data-ttu-id="b219c-255">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) att analysera data som samlas in från datakällor och lösningar.</span><span class="sxs-lookup"><span data-stu-id="b219c-255">Learn about [log searches](log-analytics-log-searches.md) to analyze the data collected from data sources and solutions.</span></span> 