---
title: aaaConnector versionshistorik | Microsoft Docs
description: "Det här avsnittet listar alla versioner av hello kopplingar för Forefront Identity Manager (FIM) och Microsoft Identity Manager (MIM)"
services: active-directory
documentationcenter: 
author: fimguy
manager: femila
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: fimguy
ms.openlocfilehash: 3522f17c30e46542eaa367ecdefdfd2fc47f71a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connector-version-release-history"></a><span data-ttu-id="5d729-103">Versionshistorik för anslutningsappen</span><span class="sxs-lookup"><span data-stu-id="5d729-103">Connector Version Release History</span></span>
<span data-ttu-id="5d729-104">hello kopplingar för Forefront Identity Manager (FIM) och Microsoft Identity Manager (MIM) uppdateras ofta.</span><span class="sxs-lookup"><span data-stu-id="5d729-104">hello Connectors for Forefront Identity Manager (FIM) and Microsoft Identity Manager (MIM) are updated frequently.</span></span>

> [!NOTE]
> <span data-ttu-id="5d729-105">Det här avsnittet finns bara på FIM och MIM.</span><span class="sxs-lookup"><span data-stu-id="5d729-105">This topic is on FIM and MIM only.</span></span> <span data-ttu-id="5d729-106">Följande kopplingar stöds inte för installation på Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="5d729-106">These Connectors are not supported for install on Azure AD Connect.</span></span> <span data-ttu-id="5d729-107">Utgiven kopplingar är förinstallerat på AADConnect när du uppgraderar toospecified Build.</span><span class="sxs-lookup"><span data-stu-id="5d729-107">Released Connectors are preinstalled on AADConnect when upgrading toospecified Build.</span></span>

<span data-ttu-id="5d729-108">Det här avsnittet listas alla versioner av hello kopplingar som har släppts.</span><span class="sxs-lookup"><span data-stu-id="5d729-108">This topic list all versions of hello Connectors that have been released.</span></span>

<span data-ttu-id="5d729-109">Relaterade länkar:</span><span class="sxs-lookup"><span data-stu-id="5d729-109">Related links:</span></span>

* [<span data-ttu-id="5d729-110">Hämta senaste kopplingar</span><span class="sxs-lookup"><span data-stu-id="5d729-110">Download Latest Connectors</span></span>](http://go.microsoft.com/fwlink/?LinkId=717495)
* <span data-ttu-id="5d729-111">[Allmän LDAP Connector](active-directory-aadconnectsync-connector-genericldap.md) refererar dokumentationen</span><span class="sxs-lookup"><span data-stu-id="5d729-111">[Generic LDAP Connector](active-directory-aadconnectsync-connector-genericldap.md) reference documentation</span></span>
* <span data-ttu-id="5d729-112">[Allmän SQL Connector](active-directory-aadconnectsync-connector-genericsql.md) refererar dokumentationen</span><span class="sxs-lookup"><span data-stu-id="5d729-112">[Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md) reference documentation</span></span>
* <span data-ttu-id="5d729-113">[Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) refererar dokumentationen</span><span class="sxs-lookup"><span data-stu-id="5d729-113">[Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) reference documentation</span></span>
* <span data-ttu-id="5d729-114">[PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) refererar dokumentationen</span><span class="sxs-lookup"><span data-stu-id="5d729-114">[PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) reference documentation</span></span>
* <span data-ttu-id="5d729-115">[Kopplingen för Lotus Domino](active-directory-aadconnectsync-connector-domino.md) refererar dokumentationen</span><span class="sxs-lookup"><span data-stu-id="5d729-115">[Lotus Domino Connector](active-directory-aadconnectsync-connector-domino.md) reference documentation</span></span>


## <a name="116040-aadconnect-pending-release"></a><span data-ttu-id="5d729-116">1.1.604.0 (AADConnect väntande versionen)</span><span class="sxs-lookup"><span data-stu-id="5d729-116">1.1.604.0 (AADConnect Pending Release)</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="5d729-117">Fast problem:</span><span class="sxs-lookup"><span data-stu-id="5d729-117">Fixed issues:</span></span>

* <span data-ttu-id="5d729-118">Allmän Web Services:</span><span class="sxs-lookup"><span data-stu-id="5d729-118">Generic Web Services:</span></span>
  * <span data-ttu-id="5d729-119">Ett problem som förhindrar att ett SOAP-projekt som skapas när det finns två eller fler slutpunkter har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="5d729-119">Fixed an issue preventing a SOAP project from being created when there were two or more endpoints.</span></span>
* <span data-ttu-id="5d729-120">Allmän SQL:</span><span class="sxs-lookup"><span data-stu-id="5d729-120">Generic SQL:</span></span>
  * <span data-ttu-id="5d729-121">Hello driften av import hello GSQL inte konvertera tid korrekt när sparade tooconnector utrymme.</span><span class="sxs-lookup"><span data-stu-id="5d729-121">In hello operation of import hello GSQL was not converting time correctly, when saved tooconnector space.</span></span> <span data-ttu-id="5d729-122">hello datum och tid standardformat för anslutarplats för hello GSQL har ändrats från ”åååå-MM-dd: ssZ' too'yyyy-MM-dd: ssZ '.</span><span class="sxs-lookup"><span data-stu-id="5d729-122">hello default date and time format for connector space of hello GSQL was changed from 'yyyy-MM-dd hh:mm:ssZ' too'yyyy-MM-dd HH:mm:ssZ'.</span></span>

## <a name="115510-aadconnect-115530"></a><span data-ttu-id="5d729-123">1.1.551.0 (AADConnect 1.1.553.0)</span><span class="sxs-lookup"><span data-stu-id="5d729-123">1.1.551.0 (AADConnect 1.1.553.0)</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="5d729-124">Fast problem:</span><span class="sxs-lookup"><span data-stu-id="5d729-124">Fixed issues:</span></span>

* <span data-ttu-id="5d729-125">Allmän Web Services:</span><span class="sxs-lookup"><span data-stu-id="5d729-125">Generic Web Services:</span></span>
  * <span data-ttu-id="5d729-126">Hej Wsconfig verktyget konverterades inte korrekt hello Json-matris från ”exempelbegäran” för hello metod för REST-tjänst.</span><span class="sxs-lookup"><span data-stu-id="5d729-126">hello Wsconfig tool did not convert correctly hello Json array from "sample request" for hello REST service method.</span></span> <span data-ttu-id="5d729-127">Detta orsakade problem med serialisering denna Json-matris för hello REST-begäran.</span><span class="sxs-lookup"><span data-stu-id="5d729-127">This caused problems with serialization this Json array for hello REST request.</span></span>
  * <span data-ttu-id="5d729-128">Web Service Connector Configuration Tool stöder inte användning av diskutrymme symboler i JSON-attributnamn</span><span class="sxs-lookup"><span data-stu-id="5d729-128">Web Service Connector Configuration Tool does not support usage of space symbols in JSON attribute names</span></span> 
    * <span data-ttu-id="5d729-129">Ett mönster för ersättning kan läggas till manuellt toohello WSConfigTool.exe.config fil, t.ex.```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span><span class="sxs-lookup"><span data-stu-id="5d729-129">A Substitution pattern can be added manually toohello WSConfigTool.exe.config file, e.g. ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span></span>

* <span data-ttu-id="5d729-130">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="5d729-130">Lotus Notes:</span></span>
  * <span data-ttu-id="5d729-131">När hello alternativet **Tillåt anpassade certifiers för enheter i organisationen/organiserad** inaktiveras hello anslutningen misslyckas under export (uppdatering) när hello export flöda alla attribut är exporterade tooDomino men när hello Exportera en KeyNotFoundException returneras tooSync.</span><span class="sxs-lookup"><span data-stu-id="5d729-131">When hello option **Allow custom certifiers for Organization/Organizational Units** is disabled then hello connector fails during export (Update) After hello export flow all attributes are exported tooDomino but at hello time of export a KeyNotFoundException is returned tooSync.</span></span> 
    * <span data-ttu-id="5d729-132">Detta inträffar eftersom hello Byt namn på åtgärden misslyckas när den försöker toochange DN (användarnamn attribut) genom att ändra en hello attribut nedan:</span><span class="sxs-lookup"><span data-stu-id="5d729-132">This happens because hello rename operation fails when it tries toochange DN (UserName attribute) by changing one of hello attributes below:</span></span>  
      - <span data-ttu-id="5d729-133">Efternamn</span><span class="sxs-lookup"><span data-stu-id="5d729-133">LastName</span></span>
      - <span data-ttu-id="5d729-134">Förnamn</span><span class="sxs-lookup"><span data-stu-id="5d729-134">FirstName</span></span>
      - <span data-ttu-id="5d729-135">MiddleInitial</span><span class="sxs-lookup"><span data-stu-id="5d729-135">MiddleInitial</span></span>
      - <span data-ttu-id="5d729-136">AltFullName</span><span class="sxs-lookup"><span data-stu-id="5d729-136">AltFullName</span></span>
      - <span data-ttu-id="5d729-137">AltFullNameLanguage</span><span class="sxs-lookup"><span data-stu-id="5d729-137">AltFullNameLanguage</span></span>
      - <span data-ttu-id="5d729-138">organisationsenhet</span><span class="sxs-lookup"><span data-stu-id="5d729-138">ou</span></span>
      - <span data-ttu-id="5d729-139">altcommonname</span><span class="sxs-lookup"><span data-stu-id="5d729-139">altcommonname</span></span>

  * <span data-ttu-id="5d729-140">När **Tillåt anpassade certifiers för enheter i organisationen/organiserad** alternativ är aktiverat, men krävs certifiers är fortfarande tom sedan KeyNotFoundException inträffar.</span><span class="sxs-lookup"><span data-stu-id="5d729-140">When **Allow custom certifiers for Organization/Organizational Units** option is enabled, but required certifiers are still empty, then KeyNotFoundException occurs.</span></span>

### <a name="enhancements"></a><span data-ttu-id="5d729-141">Förbättringar av:</span><span class="sxs-lookup"><span data-stu-id="5d729-141">Enhancements:</span></span>

* <span data-ttu-id="5d729-142">Allmän SQL:</span><span class="sxs-lookup"><span data-stu-id="5d729-142">Generic SQL:</span></span>
  * <span data-ttu-id="5d729-143">**Scenario: gjorts implementerat:** ”*” funktionen</span><span class="sxs-lookup"><span data-stu-id="5d729-143">**Scenario: redesigned Implemented:** "*" feature</span></span>
  * <span data-ttu-id="5d729-144">**Lösningsbeskrivning av:** ändras tillvägagångssätt för [med flera värden referens attribut hantering](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="5d729-144">**Solution description:** Changed approach for [multi-valued reference attributes handling](active-directory-aadconnectsync-connector-genericsql.md).</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="5d729-145">Fast problem:</span><span class="sxs-lookup"><span data-stu-id="5d729-145">Fixed issues:</span></span>

* <span data-ttu-id="5d729-146">Allmän Web Services:</span><span class="sxs-lookup"><span data-stu-id="5d729-146">Generic Web Services:</span></span>
  * <span data-ttu-id="5d729-147">Kan inte importera serverkonfigurationen om WebService koppling finns</span><span class="sxs-lookup"><span data-stu-id="5d729-147">Can’t import Server configuration if WebService Connector is present</span></span>
  * <span data-ttu-id="5d729-148">WebService-anslutningen fungerar inte med flera webbtjänster</span><span class="sxs-lookup"><span data-stu-id="5d729-148">WebService Connector is not working with multiple  Web Services</span></span>

* <span data-ttu-id="5d729-149">Allmän SQL:</span><span class="sxs-lookup"><span data-stu-id="5d729-149">Generic SQL:</span></span>
  * <span data-ttu-id="5d729-150">Inga objekt av typen listas för enstaka värde refererade attribut</span><span class="sxs-lookup"><span data-stu-id="5d729-150">No object types are listed for single value referenced attribute</span></span>
  * <span data-ttu-id="5d729-151">Deltaimport för ändringsspårning strategi tar bort objekt när värdet tas bort från Flervärde tabell</span><span class="sxs-lookup"><span data-stu-id="5d729-151">Delta import on Change Tracking strategy deletes object when value is removed from multi-value table</span></span>
  * <span data-ttu-id="5d729-152">OverflowException i GSQL connector med DB2 på AS / 400</span><span class="sxs-lookup"><span data-stu-id="5d729-152">OverflowException in GSQL connector with DB2 on AS/400</span></span>

<span data-ttu-id="5d729-153">Lotus:</span><span class="sxs-lookup"><span data-stu-id="5d729-153">Lotus:</span></span>
  * <span data-ttu-id="5d729-154">Tillagda alternativet tooenable\disable söker organisationsenheter innan du öppnar GlobalParameters sidan</span><span class="sxs-lookup"><span data-stu-id="5d729-154">Added option tooenable\disable searching OUs before opening GlobalParameters page</span></span>

## <a name="114430"></a><span data-ttu-id="5d729-155">1.1.443.0</span><span class="sxs-lookup"><span data-stu-id="5d729-155">1.1.443.0</span></span>

<span data-ttu-id="5d729-156">Utgiven: 2017 mars</span><span class="sxs-lookup"><span data-stu-id="5d729-156">Released: 2017 March</span></span>

### <a name="enhancements"></a><span data-ttu-id="5d729-157">Förbättringar</span><span class="sxs-lookup"><span data-stu-id="5d729-157">Enhancements</span></span>

* <span data-ttu-id="5d729-158">Allmän SQL:</span><span class="sxs-lookup"><span data-stu-id="5d729-158">Generic SQL:</span></span></br><span data-ttu-id="5d729-159">
  **Scenariot Symptom:** det är en välkänd begränsning med hello SQL-anslutningen där vi endast tillåta en referenstyp tooone objekt och kräver korsreferens med medlemmar.</span><span class="sxs-lookup"><span data-stu-id="5d729-159">
  **Scenario Symptoms:**  It is a well-known limitation with hello SQL Connector where we only allow a reference tooone object type and require cross reference with members.</span></span> </br><span data-ttu-id="5d729-160">
**Lösningsbeskrivning av:** hello bearbetningssteg för referenser fanns ”*” alternativet är valt kan alla kombinationer av objekt av typen returneras tillbaka toohello Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="5d729-160">
**Solution description:** In hello processing step for references were "*" option is chosen, ALL combinations of object types will be returned back toohello sync engine.</span></span>

>[!Important]
- <span data-ttu-id="5d729-161">Detta skapar många platshållare</span><span class="sxs-lookup"><span data-stu-id="5d729-161">This will create many placeholders</span></span>
- <span data-ttu-id="5d729-162">Det är obligatoriskt toomake att hello namn är unikt mellan objekttyper.</span><span class="sxs-lookup"><span data-stu-id="5d729-162">It is required toomake sure hello naming is unique cross object types.</span></span>


* <span data-ttu-id="5d729-163">Allmän LDAP:</span><span class="sxs-lookup"><span data-stu-id="5d729-163">Generic LDAP:</span></span></br><span data-ttu-id="5d729-164">
 **Scenario:** när endast några behållare har markerats i en specifik partition, sedan hello Sök fortfarande görs i hela partition.</span><span class="sxs-lookup"><span data-stu-id="5d729-164">
**Scenario:** When only few containers are selected in specific partition, then hello search still will be done in whole partition.</span></span> <span data-ttu-id="5d729-165">Specifika filtreras som synkroniseringstjänsten, men inte av MA vilket kan leda till försämrade prestanda.</span><span class="sxs-lookup"><span data-stu-id="5d729-165">Specific will be filtered by Synchronization Service, but not by MA which might cause performance degradation.</span></span> </br>

 <span data-ttu-id="5d729-166">**Lösningsbeskrivning av:** ändras GLDAP connector kod toomake det möjligt gå igenom alla behållare och söka efter objekt i var och en av dem, i stället för att söka i hela hello-partitionen.</span><span class="sxs-lookup"><span data-stu-id="5d729-166">**Solution description:** Changed GLDAP connector's code toomake it possible go through all containers and search objects in each of them, instead of searching in hello whole partition.</span></span>


* <span data-ttu-id="5d729-167">Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="5d729-167">Lotus Domino:</span></span>

  <span data-ttu-id="5d729-168">**Scenario:** Domino mail borttagning stöd för en person tas bort vid en export.</span><span class="sxs-lookup"><span data-stu-id="5d729-168">**Scenario:** Domino mail deletion support for a person removal during an export.</span></span> </br><span data-ttu-id="5d729-169">
  **Lösning:** konfigurerbara e borttagning stöd för en person tas bort vid en export.</span><span class="sxs-lookup"><span data-stu-id="5d729-169">
**Solution:** Configurable mail deletion support for a person removal during an export.</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="5d729-170">Fast problem:</span><span class="sxs-lookup"><span data-stu-id="5d729-170">Fixed issues:</span></span>
* <span data-ttu-id="5d729-171">Allmän Web Services:</span><span class="sxs-lookup"><span data-stu-id="5d729-171">Generic Web Services:</span></span>
 * <span data-ttu-id="5d729-172">När du ändrar hello-tjänstens URL i SAP wsconfig projekt via webbtjänsten konfigurationsverktyget sedan händer hello följande fel: hittade inte en del av hello sökväg</span><span class="sxs-lookup"><span data-stu-id="5d729-172">When changing hello service URL in Default SAP wsconfig projects through WebService Configuration Tool then hello following error happens: Could not find a part of hello path</span></span>

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* <span data-ttu-id="5d729-173">Allmän LDAP:</span><span class="sxs-lookup"><span data-stu-id="5d729-173">Generic LDAP:</span></span>
 * <span data-ttu-id="5d729-174">GLDAP Connector kan inte se alla attribut i AD LDS</span><span class="sxs-lookup"><span data-stu-id="5d729-174">GLDAP Connector does not see all attributes in AD LDS</span></span>
 * <span data-ttu-id="5d729-175">Guiden radbrytningar när inga UPN-attribut identifieras från hello LDAP-directory-schemat</span><span class="sxs-lookup"><span data-stu-id="5d729-175">Wizard breaks when no UPN attributes are detected from hello LDAP directory schema</span></span>
 * <span data-ttu-id="5d729-176">Delta importen misslyckas med identifiering av fel som inte finns vid fullständig import när attributet ”objectclass” inte är markerat</span><span class="sxs-lookup"><span data-stu-id="5d729-176">Delta Imports Failing with discovery errors not present during full import, when "objectclass" attribute is not selected</span></span>
 * <span data-ttu-id="5d729-177">En ”konfigurera partitioner och hierarkier” konfigurationssidan visar inte alla objekt som är vilken typ som är lika toohello partition för nya servrar i hello generisk</span><span class="sxs-lookup"><span data-stu-id="5d729-177">A "Configure Partitions and Hierarchies” configuration page, doesn’t show any objects which type is equal toohello partition for Novel servers in hello Generic</span></span>  
<span data-ttu-id="5d729-178">LDAP-MA.</span><span class="sxs-lookup"><span data-stu-id="5d729-178">LDAP MA.</span></span> <span data-ttu-id="5d729-179">De visade endast objekt från RootDSE partition.</span><span class="sxs-lookup"><span data-stu-id="5d729-179">They showed only objects from RootDSE partition.</span></span>


* <span data-ttu-id="5d729-180">Allmän SQL:</span><span class="sxs-lookup"><span data-stu-id="5d729-180">Generic SQL:</span></span>
 * <span data-ttu-id="5d729-181">Korrigering för allmän SQL vattenstämpel Deltaimport flervärdesattribut inte importera programfel</span><span class="sxs-lookup"><span data-stu-id="5d729-181">Fix for Generic SQL watermark Delta Import multivalued attribute not imported bug</span></span>
 * <span data-ttu-id="5d729-182">När du exporterar deleted\added värdena för flervärdesattribut, men de är inte deleted\added i datakällan.</span><span class="sxs-lookup"><span data-stu-id="5d729-182">When exporting deleted\added values of multivalued attribute, they are not deleted\added in data source.</span></span>  


* <span data-ttu-id="5d729-183">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="5d729-183">Lotus Notes:</span></span>
 * <span data-ttu-id="5d729-184">Ett fält ”Full” visas i hello metaversum korrekt men när du exporterar tooNotes hello värde för hello-attributet är Null eller tom.</span><span class="sxs-lookup"><span data-stu-id="5d729-184">A specific field "Full Name" is shown in hello metaverse correctly however when exporting tooNotes hello value for hello attribute is Null or Empty.</span></span>
 * <span data-ttu-id="5d729-185">Korrigering för Certifier dubblettfel</span><span class="sxs-lookup"><span data-stu-id="5d729-185">Fix for duplicate Certifier error</span></span>
 * <span data-ttu-id="5d729-186">När hello objekt utan data är markerad på hello kopplingen för Lotus Domino med andra objekt sedan felmeddelande vi hello identifiering när du utför en fullständig Import.</span><span class="sxs-lookup"><span data-stu-id="5d729-186">When hello Object without any data is selected on hello Lotus Domino Connector with other objects then we receive hello Discovery error while performing Full-Import.</span></span>
 * <span data-ttu-id="5d729-187">När Deltaimport körs på hello kopplingen för Lotus Domino hello slutet av den kör, hello Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service ibland returnerar ett programfel.</span><span class="sxs-lookup"><span data-stu-id="5d729-187">When Delta Import is being running on hello Lotus Domino Connector, at hello end of that run, hello Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service sometimes returns an Application Error.</span></span>
 * <span data-ttu-id="5d729-188">Gruppen medlemskap övergripande fungerar bra och underhålls, förutom när du kör hello export tootry tooremove en användare från medlemskap det visas som slutförd med en uppdatering, men hello användaren inte komma bort från medlemskap i Lotus Notes.</span><span class="sxs-lookup"><span data-stu-id="5d729-188">Group membership overall works fine and is maintained, except when running hello export tootry tooremove a user from membership it shows as successful with an update, but hello user doesn’t actually get removed from membership in Lotus Notes.</span></span>
 * <span data-ttu-id="5d729-189">Ett möjlighet toochoose läge exporten som ”Lägg till objekt längst ned” lades till i configuration GUI för Lotus MA tooappend nya objekt längst ned under hello export för attribut med flera värden.</span><span class="sxs-lookup"><span data-stu-id="5d729-189">An opportunity toochoose mode of export as “Append Item at bottom” was added in configuration GUI of Lotus MA tooappend new items at bottom during hello export for multi-valued attributes.</span></span>
 * <span data-ttu-id="5d729-190">Anslutningen läggs hello behövs logik toodelete hello filen från hello e-postmappen och ID-valvet.</span><span class="sxs-lookup"><span data-stu-id="5d729-190">Connector will add hello needed logic toodelete hello file from hello Mail Folder and ID Vault.</span></span>
 * <span data-ttu-id="5d729-191">Ta bort medlemskap som inte fungerar för mellan NAB medlem.</span><span class="sxs-lookup"><span data-stu-id="5d729-191">Delete membership not working for cross NAB member.</span></span>
 * <span data-ttu-id="5d729-192">Värden ska vara har tagits bort från flervärdesattribut</span><span class="sxs-lookup"><span data-stu-id="5d729-192">Values should be successfully deleted from multi-valued attribute</span></span>

## <a name="111170"></a><span data-ttu-id="5d729-193">1.1.117.0</span><span class="sxs-lookup"><span data-stu-id="5d729-193">1.1.117.0</span></span>
<span data-ttu-id="5d729-194">Utgiven: 2016 mars</span><span class="sxs-lookup"><span data-stu-id="5d729-194">Released: 2016 March</span></span>

<span data-ttu-id="5d729-195">**Ny koppling**</span><span class="sxs-lookup"><span data-stu-id="5d729-195">**New Connector**</span></span>  
<span data-ttu-id="5d729-196">Inledande versionen av hello [allmänna SQL Connector](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="5d729-196">Initial release of hello [Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md).</span></span>

<span data-ttu-id="5d729-197">**Nya funktioner:**</span><span class="sxs-lookup"><span data-stu-id="5d729-197">**New features:**</span></span>

* <span data-ttu-id="5d729-198">Allmän LDAP Connector:</span><span class="sxs-lookup"><span data-stu-id="5d729-198">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="5d729-199">Stöd för Deltaimport med Isode har lagts till.</span><span class="sxs-lookup"><span data-stu-id="5d729-199">Added support for delta import with Isode.</span></span>
* <span data-ttu-id="5d729-200">Web Services Connector:</span><span class="sxs-lookup"><span data-stu-id="5d729-200">Web Services Connector:</span></span>
  * <span data-ttu-id="5d729-201">Uppdaterade hello csEntryChangeResult aktivitet och setImportErrorCode aktivitet tooallow objektet nivån fel toobe returnerade tillbaka toohello Synkroniseringsmotorn.</span><span class="sxs-lookup"><span data-stu-id="5d729-201">Updated hello csEntryChangeResult activity and setImportErrorCode activity tooallow object level errors toobe returned back toohello sync engine.</span></span>
  * <span data-ttu-id="5d729-202">Uppdaterade hello SAP6 och SAP6User mallar toouse hello nya objekt nivån fel funktioner.</span><span class="sxs-lookup"><span data-stu-id="5d729-202">Updated hello SAP6 and SAP6User templates toouse hello new object level error functionality.</span></span>
* <span data-ttu-id="5d729-203">Lotus Domino-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="5d729-203">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="5d729-204">Du måste en certifier per adressbok för exporten.</span><span class="sxs-lookup"><span data-stu-id="5d729-204">For export, you need one certifier per address book.</span></span> <span data-ttu-id="5d729-205">Du kan nu använda hello samma lösenord för alla certifiers toomake hello management enklare.</span><span class="sxs-lookup"><span data-stu-id="5d729-205">You can now use hello same password for all certifiers toomake hello management easier.</span></span>

<span data-ttu-id="5d729-206">**Fast problem:**</span><span class="sxs-lookup"><span data-stu-id="5d729-206">**Fixed issues:**</span></span>

* <span data-ttu-id="5d729-207">Allmän LDAP Connector:</span><span class="sxs-lookup"><span data-stu-id="5d729-207">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="5d729-208">För IBM Tivoli DS identifierades vissa referensattribut på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="5d729-208">For IBM Tivoli DS, some reference attributes were not detected correctly.</span></span>
  * <span data-ttu-id="5d729-209">Mellanslag i hello början och slutet av strängar för öppna LDAP under en Deltaimport har trunkerats.</span><span class="sxs-lookup"><span data-stu-id="5d729-209">For Open LDAP during a delta import, whitespaces at hello beginning and end of strings were truncated.</span></span>
  * <span data-ttu-id="5d729-210">För Novell och NetIQ export som flytta ett objekt mellan organisationsenheter-behållare och hello samma tid som har bytt namn till hello objekt misslyckades.</span><span class="sxs-lookup"><span data-stu-id="5d729-210">For Novell and NetIQ, an export that moved an object between OUs/containers and at hello same time renamed hello object failed.</span></span>
* <span data-ttu-id="5d729-211">Web Services Connector:</span><span class="sxs-lookup"><span data-stu-id="5d729-211">Web Services Connector:</span></span>
  * <span data-ttu-id="5d729-212">Om hello webbtjänsten har flera slutpunkter för samma bindning, sedan hello Connector inte korrekt identifiera dessa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="5d729-212">If hello web service had multiple end-points for same binding, then hello Connector did not correctly discover these end-points.</span></span>
* <span data-ttu-id="5d729-213">Lotus Domino-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="5d729-213">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="5d729-214">Export av hello fullName attributet tooa e-post i databasen fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="5d729-214">An export of hello fullName attribute tooa mail-in database did not work.</span></span>
  * <span data-ttu-id="5d729-215">Export som både läggas till och ta bort medlemmen från en grupp bara exporterade hello lägga till medlemmar.</span><span class="sxs-lookup"><span data-stu-id="5d729-215">An export which both added and removed member from a group only exported hello added members.</span></span>
  * <span data-ttu-id="5d729-216">Om ett Notes-dokument är ogiltig (hello attributet isValid ange toofalse), hello anslutningen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="5d729-216">If a Notes Document is invalid (hello attribute isValid set toofalse), then hello Connector fails.</span></span>

## <a name="older-releases"></a><span data-ttu-id="5d729-217">Äldre versioner</span><span class="sxs-lookup"><span data-stu-id="5d729-217">Older releases</span></span>
<span data-ttu-id="5d729-218">Före mars 2016 publicerades hello kopplingar som supportfrågor.</span><span class="sxs-lookup"><span data-stu-id="5d729-218">Before March 2016, hello Connectors were released as support topics.</span></span>

<span data-ttu-id="5d729-219">**Allmän LDAP**</span><span class="sxs-lookup"><span data-stu-id="5d729-219">**Generic LDAP**</span></span>

* <span data-ttu-id="5d729-220">[KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597 September 2015</span><span class="sxs-lookup"><span data-stu-id="5d729-220">[KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="5d729-221">[KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015 mars</span><span class="sxs-lookup"><span data-stu-id="5d729-221">[KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="5d729-222">[KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534 januari 2015</span><span class="sxs-lookup"><span data-stu-id="5d729-222">[KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, 2015 January</span></span>
* <span data-ttu-id="5d729-223">[KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419 September 2014</span><span class="sxs-lookup"><span data-stu-id="5d729-223">[KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 September</span></span>
* <span data-ttu-id="5d729-224">[KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014 mars</span><span class="sxs-lookup"><span data-stu-id="5d729-224">[KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 March</span></span>

<span data-ttu-id="5d729-225">**WebServices**</span><span class="sxs-lookup"><span data-stu-id="5d729-225">**WebServices**</span></span>

* <span data-ttu-id="5d729-226">[KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419 September 2014</span><span class="sxs-lookup"><span data-stu-id="5d729-226">[KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="5d729-227">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="5d729-227">**PowerShell**</span></span>

* <span data-ttu-id="5d729-228">[KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419 September 2014</span><span class="sxs-lookup"><span data-stu-id="5d729-228">[KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="5d729-229">**Lotus Domino**</span><span class="sxs-lookup"><span data-stu-id="5d729-229">**Lotus Domino**</span></span>

* <span data-ttu-id="5d729-230">[KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597 September 2015</span><span class="sxs-lookup"><span data-stu-id="5d729-230">[KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="5d729-231">[KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015 mars</span><span class="sxs-lookup"><span data-stu-id="5d729-231">[KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="5d729-232">[KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712 2014 augusti</span><span class="sxs-lookup"><span data-stu-id="5d729-232">[KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014 August</span></span>
* <span data-ttu-id="5d729-233">[KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003 februari 2014</span><span class="sxs-lookup"><span data-stu-id="5d729-233">[KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, 2014 February</span></span>  
* <span data-ttu-id="5d729-234">[KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, oktober 2013</span><span class="sxs-lookup"><span data-stu-id="5d729-234">[KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 October</span></span>
* <span data-ttu-id="5d729-235">[KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534 2013 augusti</span><span class="sxs-lookup"><span data-stu-id="5d729-235">[KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 August</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d729-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5d729-236">Next steps</span></span>
<span data-ttu-id="5d729-237">Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5d729-237">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="5d729-238">Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="5d729-238">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
