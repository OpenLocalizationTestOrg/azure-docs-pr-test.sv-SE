---
title: "aaaDeploy split-kopplingstjänsten | Microsoft Docs"
description: "Dela och slå samman med elastiska Databasverktyg"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 9a993c0f-7052-46cd-aa59-073bea8d535a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6fef641cbc1e73ce34a851c53298a072dade8f9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-split-merge-service"></a><span data-ttu-id="fbc7a-103">Distribuera en tjänst för att dela/sammanslå</span><span class="sxs-lookup"><span data-stu-id="fbc7a-103">Deploy a split-merge service</span></span>
<span data-ttu-id="fbc7a-104">hello dela dokument med verktyget kan du flytta data mellan delat databaser.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-104">hello split-merge tool lets you move data between sharded databases.</span></span> <span data-ttu-id="fbc7a-105">Se [flytta data mellan databaser som skalats ut moln](sql-database-elastic-scale-overview-split-and-merge.md)</span><span class="sxs-lookup"><span data-stu-id="fbc7a-105">See [Moving data between scaled-out cloud databases](sql-database-elastic-scale-overview-split-and-merge.md)</span></span>

## <a name="download-hello-split-merge-packages"></a><span data-ttu-id="fbc7a-106">Hämta hello dela Merge-paket</span><span class="sxs-lookup"><span data-stu-id="fbc7a-106">Download hello Split-Merge packages</span></span>
1. <span data-ttu-id="fbc7a-107">Hämta hello senaste NuGet-versionen från [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-107">Download hello latest NuGet version from [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>
2. <span data-ttu-id="fbc7a-108">Öppna en kommandotolk och navigera toohello katalogen där du hämtade nuget.exe.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-108">Open a command prompt and navigate toohello directory where you downloaded nuget.exe.</span></span> <span data-ttu-id="fbc7a-109">hello hämtningen innehåller PowerShell commmands.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-109">hello download includes PowerShell commmands.</span></span>
3. <span data-ttu-id="fbc7a-110">Hämta hello senaste dela Merge-paketet i hello aktuell katalog med hello nedan kommando:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-110">Download hello latest Split-Merge package into hello current directory with hello below command:</span></span>
   ```
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

<span data-ttu-id="fbc7a-111">hello filerna placeras i en katalog med namnet **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** där *x.x.xxx.x* visar hello versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-111">hello files are placed in a directory named **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** where *x.x.xxx.x* reflects hello version number.</span></span> <span data-ttu-id="fbc7a-112">Hitta hello delade dokument Service-filer i hello **content\splitmerge\service** underkatalog, och hello delade dokument PowerShell-skript (och nödvändiga klient DLL-filer) i hello **content\splitmerge\powershell** underkatalog.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-112">Find hello split-merge Service files in hello **content\splitmerge\service** sub-directory, and hello Split-Merge PowerShell scripts (and required client .dlls) in hello **content\splitmerge\powershell** sub-directory.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbc7a-113">Krav</span><span class="sxs-lookup"><span data-stu-id="fbc7a-113">Prerequisites</span></span>
1. <span data-ttu-id="fbc7a-114">Skapa en Azure SQL DB-databas som ska användas som hello delade dokument status databas.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-114">Create an Azure SQL DB database that will be used as hello split-merge status database.</span></span> <span data-ttu-id="fbc7a-115">Gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-115">Go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="fbc7a-116">Skapa en ny **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-116">Create a new **SQL Database**.</span></span> <span data-ttu-id="fbc7a-117">Namnge hello databas och skapa en ny administratör och lösenord.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-117">Give hello database a name and create a new administrator and password.</span></span> <span data-ttu-id="fbc7a-118">Vara säker på att toorecord hello namn och lösenord för senare användning.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-118">Be sure toorecord hello name and password for later use.</span></span>
2. <span data-ttu-id="fbc7a-119">Se till att din Azure SQL DB-server gör att Azure-tjänster tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-119">Ensure that your Azure SQL DB server allows Azure Services tooconnect tooit.</span></span> <span data-ttu-id="fbc7a-120">I hello-portalen i hello **brandväggsinställningar**, se till att hello **och ge åtkomst tooAzure tjänster** inställningen för**på**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-120">In hello portal, in hello **Firewall Settings**, ensure that hello **Allow access tooAzure Services** setting is set too**On**.</span></span> <span data-ttu-id="fbc7a-121">Klicka på hello ”spara”-ikonen.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-121">Click hello "save" icon.</span></span>
   
   ![Tillåtna tjänster][1]
3. <span data-ttu-id="fbc7a-123">Skapa ett Azure Storage-konto som ska användas för diagnostik utdata.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-123">Create an Azure Storage account that will be used for diagnostics output.</span></span> <span data-ttu-id="fbc7a-124">Gå toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-124">Go toohello Azure Portal.</span></span> <span data-ttu-id="fbc7a-125">Klicka på vänster hello-fältet **ny**, klickar du på **Data + lagring**, sedan **lagring**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-125">In hello left bar, click **New**, click **Data + Storage**, then **Storage**.</span></span>
4. <span data-ttu-id="fbc7a-126">Skapa en Azure-molntjänst som innehåller delning-kopplingstjänsten.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-126">Create an Azure Cloud Service that will contain your Split-Merge service.</span></span>  <span data-ttu-id="fbc7a-127">Gå toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-127">Go toohello Azure Portal.</span></span> <span data-ttu-id="fbc7a-128">Klicka på vänster hello-fältet **ny**, sedan **Compute**, **Molntjänsten**, och **skapa**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-128">In hello left bar, click **New**, then **Compute**, **Cloud Service**, and **Create**.</span></span> 

## <a name="configure-your-split-merge-service"></a><span data-ttu-id="fbc7a-129">Konfigurera din delade kopplingstjänsten</span><span class="sxs-lookup"><span data-stu-id="fbc7a-129">Configure your Split-Merge service</span></span>
### <a name="split-merge-service-configuration"></a><span data-ttu-id="fbc7a-130">Dela kopplingstjänsten konfiguration</span><span class="sxs-lookup"><span data-stu-id="fbc7a-130">Split-Merge service configuration</span></span>
1. <span data-ttu-id="fbc7a-131">Skapa en kopia av hello i hello mappen dit du hämtade hello delade dokument sammansättningar **ServiceConfiguration.Template.cscfg** som levererades tillsammans med **SplitMergeService.cspkg** och byta namn på det. **ServiceConfiguration.cscfg**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-131">In hello folder where you downloaded hello Split-Merge assemblies, create a copy of hello **ServiceConfiguration.Template.cscfg** file that shipped alongside **SplitMergeService.cspkg** and rename it **ServiceConfiguration.cscfg**.</span></span>
2. <span data-ttu-id="fbc7a-132">Öppna **ServiceConfiguration.cscfg** i en textredigerare som Visual Studio som validerar indata till exempel hello format för certifikattumavtryck.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-132">Open **ServiceConfiguration.cscfg** in a text editor such as Visual Studio that validates inputs such as hello format of certificate thumbprints.</span></span>
3. <span data-ttu-id="fbc7a-133">Skapa en ny databas eller välj en befintlig databas tooserve som hello status databas för delade Merge-operationer och hämta hello anslutningssträngen för databasen.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-133">Create a new database or choose an existing database tooserve as hello status database for Split-Merge operations and retrieve hello connection string of that database.</span></span> 
   
   > [!IMPORTANT]
   > <span data-ttu-id="fbc7a-134">För tillfället hello status databasen måste använda hello latinska sortering (SQL\_Latin1\_allmänna\_CP1\_CI\_AS).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-134">At this time, hello status database must use hello Latin  collation (SQL\_Latin1\_General\_CP1\_CI\_AS).</span></span> <span data-ttu-id="fbc7a-135">Mer information finns i [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-135">For more information, see [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx).</span></span>
   >

   <span data-ttu-id="fbc7a-136">Med Azure SQL DB är hello anslutningssträngen vanligtvis hello formuläret:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-136">With Azure SQL DB, hello connection string typically is of hello form:</span></span>
      ```
      Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
      ```

4. <span data-ttu-id="fbc7a-137">Ange den här anslutningssträngen i hello cscfg-filen i båda hello **SplitMergeWeb** och **SplitMergeWorker** roll avsnitt i hello ElasticScaleMetadata inställningen.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-137">Enter this connection string in hello cscfg file in both hello **SplitMergeWeb** and **SplitMergeWorker** role sections in hello ElasticScaleMetadata setting.</span></span>
5. <span data-ttu-id="fbc7a-138">För hello **SplitMergeWorker** roll, ange en giltig anslutning sträng tooAzure lagring för hello **WorkerRoleSynchronizationStorageAccountConnectionString** inställningen.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-138">For hello **SplitMergeWorker** role, enter a valid connection string tooAzure storage for hello **WorkerRoleSynchronizationStorageAccountConnectionString** setting.</span></span>

### <a name="configure-security"></a><span data-ttu-id="fbc7a-139">Konfigurera säkerhet</span><span class="sxs-lookup"><span data-stu-id="fbc7a-139">Configure security</span></span>
<span data-ttu-id="fbc7a-140">För detaljerade anvisningar tooconfigure hello säkerheten för hello-tjänsten, referera toohello [delade dokument säkerhetskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-140">For detailed instructions tooconfigure hello security of hello service, refer toohello [Split-Merge security configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

<span data-ttu-id="fbc7a-141">Hello enligt en enkel testdistributionen för den här självstudiekursen utföra en minimal uppsättning configuration steg tooget hello tjänst igång.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-141">For hello purposes of a simple test deployment for this tutorial, a minimal set of configuration steps will be performed tooget hello service up and running.</span></span> <span data-ttu-id="fbc7a-142">Dessa steg aktiverar endast hello en/datorkontot toocommunicate med hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-142">These steps enable only hello one machine/account executing them toocommunicate with hello service.</span></span>

### <a name="create-a-self-signed-certificate"></a><span data-ttu-id="fbc7a-143">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="fbc7a-143">Create a self-signed certificate</span></span>
<span data-ttu-id="fbc7a-144">Skapa en ny katalog och från den här katalogen execute hello följande kommando med hjälp av en [Developer kommandotolk för Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) fönster:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-144">Create a new directory and from this directory execute hello following command using a [Developer Command Prompt for Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) window:</span></span>

   ```
    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

<span data-ttu-id="fbc7a-145">Du uppmanas att ange lösenord tooprotect hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-145">You are asked for a password tooprotect hello private key.</span></span> <span data-ttu-id="fbc7a-146">Ange ett starkt lösenord och bekräfta.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-146">Enter a strong password and confirm it.</span></span> <span data-ttu-id="fbc7a-147">Du uppmanas sedan hello lösenord toobe används en gång efter som.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-147">You are then prompted for hello password toobe used once more after that.</span></span> <span data-ttu-id="fbc7a-148">Klicka på **Ja** på hello slutet tooimport den toohello betrodda certifikatutfärdare myndigheter rotarkivet.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-148">Click **Yes** at hello end tooimport it toohello Trusted Certification Authorities Root store.</span></span>

### <a name="create-a-pfx-file"></a><span data-ttu-id="fbc7a-149">Skapa en PFX-fil</span><span class="sxs-lookup"><span data-stu-id="fbc7a-149">Create a PFX file</span></span>
<span data-ttu-id="fbc7a-150">Kör följande kommando från hello hello samma fönster där makecert utfördes; Använd hello samma lösenord som du använt toocreate hello certifikat:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-150">Execute hello following command from hello same window where makecert was executed; use hello same password that you used toocreate hello certificate:</span></span>

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### <a name="import-hello-client-certificate-into-hello-personal-store"></a><span data-ttu-id="fbc7a-151">Importera hello klientcertifikatet till hello datorarkivet</span><span class="sxs-lookup"><span data-stu-id="fbc7a-151">Import hello client certificate into hello personal store</span></span>
1. <span data-ttu-id="fbc7a-152">Dubbelklicka i Utforskaren, **MyCert.pfx**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-152">In Windows Explorer, double-click **MyCert.pfx**.</span></span>
2. <span data-ttu-id="fbc7a-153">I hello **guiden Importera certifikat** Välj **aktuell användare** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-153">In hello **Certificate Import Wizard** select **Current User** and click **Next**.</span></span>
3. <span data-ttu-id="fbc7a-154">Bekräfta hello sökväg och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-154">Confirm hello file path and click **Next**.</span></span>
4. <span data-ttu-id="fbc7a-155">Ange hello lösenord, lämna **inkludera alla utökade egenskaper** markerad och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-155">Type hello password, leave **Include all extended properties** checked and click **Next**.</span></span>
5. <span data-ttu-id="fbc7a-156">Lämna **väljer automatiskt hello certifikatarkiv [...]**  markerad och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-156">Leave **Automatically select hello certificate store[…]** checked and click **Next**.</span></span>
6. <span data-ttu-id="fbc7a-157">Klicka på **Slutför** och **OK**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-157">Click **Finish** and **OK**.</span></span>

### <a name="upload-hello-pfx-file-toohello-cloud-service"></a><span data-ttu-id="fbc7a-158">Överför hello PFX-filen toohello Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="fbc7a-158">Upload hello PFX file toohello cloud service</span></span>
1. <span data-ttu-id="fbc7a-159">Gå toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-159">Go toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fbc7a-160">Välj **molntjänster**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-160">Select **Cloud Services**.</span></span>
3. <span data-ttu-id="fbc7a-161">Välj hello molntjänst som du skapade ovan för hello dela/kopplingstjänsten.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-161">Select hello cloud service you created above for hello Split/Merge service.</span></span>
4. <span data-ttu-id="fbc7a-162">Klicka på **certifikat** på hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-162">Click **Certificates** on hello top menu.</span></span>
5. <span data-ttu-id="fbc7a-163">Klicka på **överför** i hello nedre fältet.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-163">Click **Upload** in hello bottom bar.</span></span>
6. <span data-ttu-id="fbc7a-164">Välj hello PFX-filen och ange hello samma lösenord som ovan.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-164">Select hello PFX file and enter hello same password as above.</span></span>
7. <span data-ttu-id="fbc7a-165">Kopiera hello certifikatets tumavtryck från hello ny post i listan hello när klar.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-165">Once completed, copy hello certificate thumbprint from hello new entry in hello list.</span></span>

### <a name="update-hello-service-configuration-file"></a><span data-ttu-id="fbc7a-166">Uppdatera hello-tjänstkonfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="fbc7a-166">Update hello service configuration file</span></span>
<span data-ttu-id="fbc7a-167">Klistra in tumavtrycket för certifikatet hello kopieras över till hello tumavtrycksvärde/attribut för de här inställningarna.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-167">Paste hello certificate thumbprint copied above into hello thumbprint/value attribute of these settings.</span></span>
<span data-ttu-id="fbc7a-168">För hello worker-rollen:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-168">For hello worker role:</span></span>
   ```
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="fbc7a-169">För hello webbroll:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-169">For hello web role:</span></span>

   ```
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

<span data-ttu-id="fbc7a-170">Observera att för Produktionsdistribution separat certifikat ska användas för hello Certifikatutfärdaren för kryptering, hello servercertifikat och klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-170">Please note that for production deployments separate certificates should be used for hello CA, for encryption, hello Server certificate and client certificates.</span></span> <span data-ttu-id="fbc7a-171">Detaljerad information om detta finns i [säkerhetskonfiguration](sql-database-elastic-scale-split-merge-security-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-171">For detailed instructions on this, see [Security Configuration](sql-database-elastic-scale-split-merge-security-configuration.md).</span></span>

## <a name="deploy-your-service"></a><span data-ttu-id="fbc7a-172">Distribuera din tjänst</span><span class="sxs-lookup"><span data-stu-id="fbc7a-172">Deploy your service</span></span>
1. <span data-ttu-id="fbc7a-173">Gå toohello [Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-173">Go toohello [Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="fbc7a-174">Klicka på hello **molntjänster** hello vänster och välj hello molntjänst som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-174">Click hello **Cloud Services** tab on hello left, and select hello cloud service that you created earlier.</span></span>
3. <span data-ttu-id="fbc7a-175">Klicka på **instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-175">Click **Dashboard**.</span></span>
4. <span data-ttu-id="fbc7a-176">Välj hello mellanlagring miljö och klicka sedan på **ladda upp en ny fristående distribution**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-176">Choose hello staging environment, then click **Upload a new staging deployment**.</span></span>
   
   ![Mellanlagring][3]
5. <span data-ttu-id="fbc7a-178">Ange en distributionsetiketten hello i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-178">In hello dialog box, enter a deployment label.</span></span> <span data-ttu-id="fbc7a-179">Klicka på 'Från lokala' för både 'Paketet ”och” Configuration ”och välj hello **SplitMergeService.cspkg** fil- och .cscfg-filen som du tidigare har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-179">For both 'Package' and 'Configuration', click 'From Local' and choose hello **SplitMergeService.cspkg** file and your .cscfg file that you configured earlier.</span></span>
6. <span data-ttu-id="fbc7a-180">Se till att kryssrutan hello **distribuera även om en eller flera roller innehåller en enda instans** är markerad.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-180">Ensure that hello checkbox labeled **Deploy even if one or more roles contain a single instance** is checked.</span></span>
7. <span data-ttu-id="fbc7a-181">Nådde hello tick-knappen i hello nedre högra toobegin hello distribution.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-181">Hit hello tick button in hello bottom right toobegin hello deployment.</span></span> <span data-ttu-id="fbc7a-182">Förväntat tootake toocomplete för några minuter.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-182">Expect it tootake a few minutes toocomplete.</span></span>

   ![Ladda upp][4]

## <a name="troubleshoot-hello-deployment"></a><span data-ttu-id="fbc7a-184">Felsöka hello-distribution</span><span class="sxs-lookup"><span data-stu-id="fbc7a-184">Troubleshoot hello deployment</span></span>
<span data-ttu-id="fbc7a-185">Om din webbroll misslyckas toocome online, är det troligt problem med hello säkerhetskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-185">If your web role fails toocome online, it is likely a problem with hello security configuration.</span></span> <span data-ttu-id="fbc7a-186">Kontrollera att hello SSL har konfigurerats enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-186">Check that hello SSL is configured as described above.</span></span>

<span data-ttu-id="fbc7a-187">Om arbetsrollen misslyckas toocome online, men din webbroll lyckas, är det troligen inte att ansluta toohello status databasen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-187">If your worker role fails toocome online, but your web role succeeds, it is most likely a problem connecting toohello status database that you created earlier.</span></span>

* <span data-ttu-id="fbc7a-188">Kontrollera att det stämmer hello anslutningssträngen i din .cscfg.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-188">Make sure that hello connection string in your .cscfg is accurate.</span></span>
* <span data-ttu-id="fbc7a-189">Kontrollera att hello-servern och databasen finns och att hello användar-id och lösenord är korrekta.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-189">Check that hello server and database exist, and that hello user id and password are correct.</span></span>
* <span data-ttu-id="fbc7a-190">Hello anslutningssträngen bör vara hello formatet för Azure SQL DB:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-190">For Azure SQL DB, hello connection string should be of hello form:</span></span>

   ```  
   Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30
   ```

* <span data-ttu-id="fbc7a-191">Kontrollera att servernamnet hello inte börjar med **https://**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-191">Ensure that hello server name does not begin with **https://**.</span></span>
* <span data-ttu-id="fbc7a-192">Se till att din Azure SQL DB-server gör att Azure-tjänster tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-192">Ensure that your Azure SQL DB server allows Azure Services tooconnect tooit.</span></span> <span data-ttu-id="fbc7a-193">toodo, öppna https://manage.windowsazure.com, klickar du på ”SQL-databaser på hello vänster, klickar du på” servrar ”hello överst och markera din server.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-193">toodo this, open https://manage.windowsazure.com, click “SQL Databases” on hello left, click “Servers” at hello top, and select your server.</span></span> <span data-ttu-id="fbc7a-194">Klicka på **konfigurera** på hello uppifrån och se till att hello **Azure Services** inställningen för ”Ja”.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-194">Click **Configure** at hello top and ensure that hello **Azure Services** setting is set too“Yes”.</span></span> <span data-ttu-id="fbc7a-195">(Se krav för hello avsnittet hello överst i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-195">(See hello Prerequisites section at hello top of this article).</span></span>

## <a name="test-hello-service-deployment"></a><span data-ttu-id="fbc7a-196">Testa hello tjänstdistribution</span><span class="sxs-lookup"><span data-stu-id="fbc7a-196">Test hello service deployment</span></span>
### <a name="connect-with-a-web-browser"></a><span data-ttu-id="fbc7a-197">Ansluta med en webbläsare</span><span class="sxs-lookup"><span data-stu-id="fbc7a-197">Connect with a web browser</span></span>
<span data-ttu-id="fbc7a-198">Fastställa hello webbslutpunkten för din delade kopplingstjänsten.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-198">Determine hello web endpoint of your Split-Merge service.</span></span> <span data-ttu-id="fbc7a-199">Du hittar det i hello klassiska Azure-portalen genom att gå toohello **instrumentpanelen** Molntjänsten och söker **Webbadress** hello höger.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-199">You can find this in hello Azure Classic Portal by going toohello **Dashboard** of your cloud service and looking under **Site URL** on hello right side.</span></span> <span data-ttu-id="fbc7a-200">Ersätt **http://** med **https://** eftersom hello standardsäkerhetsinställningarna inaktivera hello HTTP-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-200">Replace **http://** with **https://** since hello default security settings disable hello HTTP endpoint.</span></span> <span data-ttu-id="fbc7a-201">Läs in hello sidan för denna URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-201">Load hello page for this URL into your browser.</span></span>

### <a name="test-with-powershell-scripts"></a><span data-ttu-id="fbc7a-202">Testa med PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="fbc7a-202">Test with PowerShell scripts</span></span>
<span data-ttu-id="fbc7a-203">hello distribution och din miljö kan testas genom att köra hello ingår exempel PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-203">hello deployment and your environment can be tested by running hello included sample PowerShell scripts.</span></span>

<span data-ttu-id="fbc7a-204">hello skriptfiler ingår är:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-204">hello script files included are:</span></span>

1. <span data-ttu-id="fbc7a-205">**SetupSampleSplitMergeEnvironment.ps1** -ställer in en test datanivå för delade/Merge (se tabellen nedan ger en detaljerad beskrivning)</span><span class="sxs-lookup"><span data-stu-id="fbc7a-205">**SetupSampleSplitMergeEnvironment.ps1** - sets up a test data tier for Split/Merge (see table below for detailed description)</span></span>
2. <span data-ttu-id="fbc7a-206">**ExecuteSampleSplitMerge.ps1** -kör teståtgärder på hello test data tjänstnivån (se tabellen nedan ger en detaljerad beskrivning)</span><span class="sxs-lookup"><span data-stu-id="fbc7a-206">**ExecuteSampleSplitMerge.ps1** - executes test operations on hello test data tier (see table below for detailed description)</span></span>
3. <span data-ttu-id="fbc7a-207">**GetMappings.ps1** - översta exempel på skript som skriver ut hello hello Fragmentera mappningar aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-207">**GetMappings.ps1** - top-level sample script that prints out hello current state of hello shard mappings.</span></span>
4. <span data-ttu-id="fbc7a-208">**ShardManagement.psm1** -helper-skript som omsluter hello ShardManagement API</span><span class="sxs-lookup"><span data-stu-id="fbc7a-208">**ShardManagement.psm1**  - helper script that wraps hello ShardManagement API</span></span>
5. <span data-ttu-id="fbc7a-209">**SqlDatabaseHelpers.psm1** -helper-skript för att skapa och hantera SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="fbc7a-209">**SqlDatabaseHelpers.psm1** - helper script for creating and managing SQL databases</span></span>
   
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="fbc7a-210">PowerShell-fil</span><span class="sxs-lookup"><span data-stu-id="fbc7a-210">PowerShell file</span></span></th>
       <th><span data-ttu-id="fbc7a-211">Steg</span><span class="sxs-lookup"><span data-stu-id="fbc7a-211">Steps</span></span></th>
     </tr>
     <tr>
       <th rowspan="5"><span data-ttu-id="fbc7a-212">SetupSampleSplitMergeEnvironment.ps1</span><span class="sxs-lookup"><span data-stu-id="fbc7a-212">SetupSampleSplitMergeEnvironment.ps1</span></span></th>
       <td>1.    <span data-ttu-id="fbc7a-213">Skapar en Fragmentera kartan manager-databas</span><span class="sxs-lookup"><span data-stu-id="fbc7a-213">Creates a shard map manager database</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="fbc7a-214">Skapar 2 Fragmentera databaser.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-214">Creates 2 shard databases.</span></span>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="fbc7a-215">Skapar en Fragmentera karta för dessa databasen (ta bort alla befintliga Fragmentera mappar på de databaserna som).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-215">Creates a shard map for those database (deletes any existing shard maps on those databases).</span></span> </td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="fbc7a-216">Skapar en liten exempeltabell i båda hello shards och fylls hello tabell i ett hello delar.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-216">Creates a small sample table in both hello shards, and populates hello table in one of hello shards.</span></span></td>
     </tr>
     <tr>
       <td>5.    <span data-ttu-id="fbc7a-217">Deklarerar hello SchemaInfo för hello delat tabellen.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-217">Declares hello SchemaInfo for hello sharded table.</span></span></td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th><span data-ttu-id="fbc7a-218">PowerShell-fil</span><span class="sxs-lookup"><span data-stu-id="fbc7a-218">PowerShell file</span></span></th>
       <th><span data-ttu-id="fbc7a-219">Steg</span><span class="sxs-lookup"><span data-stu-id="fbc7a-219">Steps</span></span></th>
     </tr>
   <tr>
       <th rowspan="4"><span data-ttu-id="fbc7a-220">ExecuteSampleSplitMerge.ps1</span><span class="sxs-lookup"><span data-stu-id="fbc7a-220">ExecuteSampleSplitMerge.ps1</span></span> </th>
       <td>1.    <span data-ttu-id="fbc7a-221">Skickar en delad begäran toohello dela kopplingstjänsten web frontend som delar upp halva hello data från hello första Fragmentera toohello andra Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-221">Sends a split request toohello Split-Merge Service web frontend, which splits half hello data from hello first shard toohello second shard.</span></span></td>
     </tr>
     <tr>
       <td>2.    <span data-ttu-id="fbc7a-222">Omröstningar hello web frontend för hello dela status för tjänstbegäran och väntar tills hello begäran har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-222">Polls hello web frontend for hello split request status and waits until hello request completes.</span></span></td>
     </tr>
     <tr>
       <td>3.    <span data-ttu-id="fbc7a-223">Skickar en sammanslagning begäran toohello dela kopplingstjänsten web frontend som flyttar hello data från hello andra Fragmentera tillbaka toohello första Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-223">Sends a merge request toohello Split-Merge Service web frontend, which moves hello data from hello second shard back toohello first shard.</span></span></td>
     </tr>
     <tr>
       <td>4.    <span data-ttu-id="fbc7a-224">Omröstningar hello web frontend för hello merge begärandestatus och väntar tills hello begäran har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-224">Polls hello web frontend for hello merge request status and waits until hello request completes.</span></span></td>
     </tr>
   </table>
   
## <a name="use-powershell-tooverify-your-deployment"></a><span data-ttu-id="fbc7a-225">Använd PowerShell tooverify distributionen</span><span class="sxs-lookup"><span data-stu-id="fbc7a-225">Use PowerShell tooverify your deployment</span></span>
1. <span data-ttu-id="fbc7a-226">Öppna ett nytt fönster i PowerShell och navigera toohello katalogen där du hämtade hello delade dokument paketet och gå sedan till hello ”powershell” katalog.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-226">Open a new PowerShell window and navigate toohello directory where you downloaded hello Split-Merge package, and then navigate into hello “powershell” directory.</span></span>
2. <span data-ttu-id="fbc7a-227">Skapa en Azure SQL database-server (eller välj en befintlig server) där hello Fragmentera kartan manager och shards kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-227">Create an Azure SQL database server (or choose an existing server) where hello shard map manager and shards will be created.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="fbc7a-228">Hej SetupSampleSplitMergeEnvironment.ps1 skript skapar de här databaserna på hello samma server med tookeep hello standardskript enkla.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-228">hello SetupSampleSplitMergeEnvironment.ps1 script creates all these databases on hello same server by default tookeep hello script simple.</span></span> <span data-ttu-id="fbc7a-229">Detta är inte en begränsning för hello dela kopplingstjänsten sig själv.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-229">This is not a restriction of hello Split-Merge Service itself.</span></span>
   >
   
   <span data-ttu-id="fbc7a-230">En SQL-autentisering-inloggning med läsning och skrivning åtkomst toohello DBs som behövs för hello dela kopplade service toomove data och uppdatera hello Fragmentera mappar.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-230">A SQL authentication login with read/write access toohello DBs will be needed for hello Split-Merge service toomove data and update hello shard map.</span></span> <span data-ttu-id="fbc7a-231">Eftersom hello dela kopplingstjänsten körs i molnet hello, stöder den för närvarande inte integrerad autentisering.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-231">Since hello Split-Merge Service runs in hello cloud, it does not currently support Integrated Authentication.</span></span>
   
   <span data-ttu-id="fbc7a-232">Kontrollera att hello Azure SQL server är konfigurerat tooallow åtkomst från hello IP-adressen för hello-dator som kör dessa skript.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-232">Make sure hello Azure SQL server is configured tooallow access from hello IP address of hello machine running these scripts.</span></span> <span data-ttu-id="fbc7a-233">Du hittar den här inställningen under hello Azure SQL-server / configuration / tillåtna IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-233">You can find this setting under hello Azure SQL server / configuration / allowed IP addresses.</span></span>
3. <span data-ttu-id="fbc7a-234">Köra hello SetupSampleSplitMergeEnvironment.ps1 skriptet toocreate hello exempel miljö.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-234">Execute hello SetupSampleSplitMergeEnvironment.ps1 script toocreate hello sample environment.</span></span>
   
   <span data-ttu-id="fbc7a-235">Kör skriptet kommer att radera alla befintliga Fragmentera management kartdata strukturer på hello Fragmentera kartan manager-databasen och hello delar.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-235">Running this script will wipe out any existing shard map management data structures on hello shard map manager database and hello shards.</span></span> <span data-ttu-id="fbc7a-236">Det kan vara användbart toorerun hello skript om du vill initiera toore hello Fragmentera kartan eller delar.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-236">It may be useful toorerun hello script if you wish toore-initialize hello shard map or shards.</span></span>
   
   <span data-ttu-id="fbc7a-237">Kommandorad:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-237">Sample command line:</span></span>

   ```   
     .\SetupSampleSplitMergeEnvironment.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```      
4. <span data-ttu-id="fbc7a-238">Köra hello Getmappings.ps1 tooview hello skriptmappningar som för närvarande finns i hello exempel miljö.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-238">Execute hello Getmappings.ps1 script tooview hello mappings that currently exist in hello sample environment.</span></span>
   
   ```
     .\GetMappings.ps1 
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net'

   ```         
5. <span data-ttu-id="fbc7a-239">Köra hello ExecuteSampleSplitMerge.ps1 skriptet tooexecute en split-åtgärd (halva hello data flyttas på hello första Fragmentera toohello andra Fragmentera) och en merge-operation (hello data flyttas tillbaka till hello första Fragmentera).</span><span class="sxs-lookup"><span data-stu-id="fbc7a-239">Execute hello ExecuteSampleSplitMerge.ps1 script tooexecute a split operation (moving half hello data on hello first shard toohello second shard) and then a merge operation (moving hello data back onto hello first shard).</span></span> <span data-ttu-id="fbc7a-240">Om du har konfigurerat SSL och vänstra hello http-slutpunkten inaktiverad du kontrollera att du använder hello https:// slutpunkt i stället.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-240">If you configured SSL and left hello http endpoint disabled, ensure that you use hello https:// endpoint instead.</span></span>
   
   <span data-ttu-id="fbc7a-241">Kommandorad:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-241">Sample command line:</span></span>

   ```   
     .\ExecuteSampleSplitMerge.ps1
   
         -UserName 'mysqluser' 
         -Password 'MySqlPassw0rd' 
         -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
         -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
         -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```      
   
   <span data-ttu-id="fbc7a-242">Om du får hello nedan fel är troligen ett problem med din webbslutpunkten certifikat.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-242">If you receive hello below error, it is most likely a problem with your Web endpoint’s certificate.</span></span> <span data-ttu-id="fbc7a-243">Försök att ansluta toohello webbslutpunkten med din favorit webbläsare och kontrollera om det finns fel på certifikatet.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-243">Try connecting toohello Web endpoint with your favorite Web browser and check if there is a certificate error.</span></span>
   
     ```
     Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLSsecure channel.
     ```
   
   <span data-ttu-id="fbc7a-244">Om det lyckades hello utdata ska se ut så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-244">If it succeeded, hello output should look like hello below:</span></span>
   
   ```
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C tooend
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source tootarget shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing hello request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```
6. <span data-ttu-id="fbc7a-245">Experimentera med andra datatyper!</span><span class="sxs-lookup"><span data-stu-id="fbc7a-245">Experiment with other data types!</span></span> <span data-ttu-id="fbc7a-246">Alla dessa skript tar en valfri parameter för - ShardKeyType som gör att du toospecify hello nyckeltyp.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-246">All of these scripts take an optional -ShardKeyType parameter that allows you toospecify hello key type.</span></span> <span data-ttu-id="fbc7a-247">hello standardvärdet är Int32, men du kan också ange Int64, Guid eller Binary.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-247">hello default is Int32, but you can also specify Int64, Guid, or Binary.</span></span>

## <a name="create-requests"></a><span data-ttu-id="fbc7a-248">Startförfrågan</span><span class="sxs-lookup"><span data-stu-id="fbc7a-248">Create requests</span></span>
<span data-ttu-id="fbc7a-249">hello-tjänsten kan användas med hjälp av hello webbgränssnittet eller genom att importera och använda hello SplitMerge.psm1 PowerShell-modul som kommer att skicka din begäran via hello webbroll.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-249">hello service can be used either by using hello web UI or by importing and using hello SplitMerge.psm1 PowerShell module which will submit your requests through hello web role.</span></span>

<span data-ttu-id="fbc7a-250">hello-tjänsten kan flytta data i både delat tabeller och referenstabeller.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-250">hello service can move data in both sharded tables and reference tables.</span></span> <span data-ttu-id="fbc7a-251">Ett delat tabellen har en nyckelkolumn för horisontell partitionering och har olika raddata på varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-251">A sharded table has a sharding key column and has different row data on each shard.</span></span> <span data-ttu-id="fbc7a-252">Är inte en referenstabell delat så att den innehåller hello samma rad data på varje Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-252">A reference table is not sharded so it contains hello same row data on every shard.</span></span> <span data-ttu-id="fbc7a-253">För referenstabeller är användbart för data som inte ändras ofta och använda tooJOIN med delat tabeller i frågor.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-253">Reference tables are useful for data that does not change often and is used tooJOIN with sharded tables in queries.</span></span>

<span data-ttu-id="fbc7a-254">Du måste deklarera hello delat tabeller och referenstabeller som du vill flytta toohave i ordning tooperform en delad merge-operation.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-254">In order tooperform a split-merge operation, you must declare hello sharded tables and reference tables that you want toohave moved.</span></span> <span data-ttu-id="fbc7a-255">Detta åstadkoms med hello **SchemaInfo** API.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-255">This is accomplished with hello **SchemaInfo** API.</span></span> <span data-ttu-id="fbc7a-256">Detta API är i hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** namnområde.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-256">This API is in hello **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** namespace.</span></span>

1. <span data-ttu-id="fbc7a-257">För varje delat tabell skapar en **ShardedTableInfo** -objektet som beskriver hello tabell överordnade schemanamn (valfritt, standardvärden för ”dbo”) hello tabellnamn och hello kolumnnamnet i den tabell som innehåller hello horisontell partitionering nyckel.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-257">For each sharded table, create a **ShardedTableInfo** object describing hello table’s parent schema name (optional, defaults too“dbo”), hello table name, and hello column name in that table that contains hello sharding key.</span></span>
2. <span data-ttu-id="fbc7a-258">För varje tabell, skapar du en **ReferenceTableInfo** -objektet som beskriver hello tabell överordnade schemanamn (valfritt, standardvärden för ”dbo”) och hello tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-258">For each reference table, create a **ReferenceTableInfo** object describing hello table’s parent schema name (optional, defaults too“dbo”) and hello table name.</span></span>
3. <span data-ttu-id="fbc7a-259">Lägg till hello ovan TableInfo objekt tooa nya **SchemaInfo** objekt.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-259">Add hello above TableInfo objects tooa new **SchemaInfo** object.</span></span>
4. <span data-ttu-id="fbc7a-260">Hämta en referens tooa **ShardMapManager** objektet och anropet **GetSchemaInfoCollection**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-260">Get a reference tooa **ShardMapManager** object, and call **GetSchemaInfoCollection**.</span></span>
5. <span data-ttu-id="fbc7a-261">Lägg till hello **SchemaInfo** toohello **SchemaInfoCollection**, tillhandahåller hello Fragmentera mappningsnamn.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-261">Add hello **SchemaInfo** toohello **SchemaInfoCollection**, providing hello shard map name.</span></span>

<span data-ttu-id="fbc7a-262">Ett exempel på detta kan ses i hello SetupSampleSplitMergeEnvironment.ps1 skript.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-262">An example of this can be seen in hello SetupSampleSplitMergeEnvironment.ps1 script.</span></span>

<span data-ttu-id="fbc7a-263">hello dela kopplingstjänsten skapar inte hello måldatabasen (eller schemat för alla tabeller i databasen hello) för dig.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-263">hello Split-Merge service does not create hello target database (or schema for any tables in hello database) for you.</span></span> <span data-ttu-id="fbc7a-264">De måste vara skapats i förväg innan du skickar en begäran toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-264">They must be pre-created before sending a request toohello service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="fbc7a-265">Felsökning</span><span class="sxs-lookup"><span data-stu-id="fbc7a-265">Troubleshooting</span></span>
<span data-ttu-id="fbc7a-266">Hello under meddelandet kan visas när du kör hello exempel powershell-skript:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-266">You may see hello below message when running hello sample powershell scripts:</span></span>

   ```
   Invoke-WebRequest : hello underlying connection was closed: Could not establish trust relationship for hello SSL/TLS secure channel.
   ```

<span data-ttu-id="fbc7a-267">Det här felet betyder att SSL-certifikatet inte har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-267">This error means that your SSL certificate is not configured correctly.</span></span> <span data-ttu-id="fbc7a-268">Följ hello anvisningarna i avsnittet ”ansluta med en webbläsare'.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-268">Please follow hello instructions in section 'Connecting with a web browser'.</span></span>

<span data-ttu-id="fbc7a-269">Om du inte kan skicka begäranden visas här:</span><span class="sxs-lookup"><span data-stu-id="fbc7a-269">If you cannot submit requests you may see this:</span></span>

```
[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 
```

<span data-ttu-id="fbc7a-270">I det här fallet Kontrollera konfigurationsfilen i viss hello inställning för **WorkerRoleSynchronizationStorageAccountConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-270">In this case, check your configuration file, in particular hello setting for **WorkerRoleSynchronizationStorageAccountConnectionString**.</span></span> <span data-ttu-id="fbc7a-271">Det här felet indikerar vanligtvis att hello arbetsrollen inte kunde initiera hello metadata-databasen vid första användning har.</span><span class="sxs-lookup"><span data-stu-id="fbc7a-271">This error typically indicates that hello worker role could not successfully initialize hello metadata database on first use.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png

