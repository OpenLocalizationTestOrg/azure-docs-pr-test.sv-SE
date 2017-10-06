---
title: "Azure AD Connect-synkronisering: driftåtgärder och saker | Microsoft Docs"
description: "Det här avsnittet beskrivs åtgärder för Azure AD Connect-synkronisering och hur tooprepare för användning av den här komponenten."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e6b21262e0924785808888d66f08a04a56581edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect-synkronisering: driftåtgärder och beräkningen
hello syftet med det här avsnittet är toodescribe operativa uppgifter för Azure AD Connect-synkronisering.

## <a name="staging-mode"></a>Mellanlagringsläge
Mellanlagringsläge kan användas för flera olika scenarier, inklusive:

* Hög tillgänglighet.
* Testa och distribuera nya konfigurationsändringar.
* Introducerar en ny server och inaktivera hello gamla.

Med en server i fristående läge, kan du ändra ändringar toohello konfiguration och förhandsgranska hello innan du aktivera hello-server. Du kan också toorun fullständig import och fullständig synkronisering tooverify att alla ändringar förväntas innan du gör dessa ändringar till din produktionsmiljö.

Under installationen kan du välja hello server toobe i **mellanlagringsläge**. Den här åtgärden gör hello-server som är aktiva för import och synkronisering, men några exporter körs inte. En server i fristående läge körs inte Lösenordssynkronisering och tillbakaskrivning av lösenord, även om dessa funktioner har valts under installationen. När du inaktiverar mellanlagringsläget hello server börjar exporten, aktiverar Lösenordssynkronisering och aktivera tillbakaskrivning av lösenord.

Du kan fortfarande framtvinga en export med hello synkronisering service manager.

En server i mellanlagringsläge fortsätter tooreceive ändringar från Active Directory och Azure AD. Den har alltid en kopia av hello senaste ändringarna och kan snabbt ta över hello ansvar för en annan server. Om du gör ändringar tooyour primära konfigurationsservern är ditt ansvar toomake hello samma ändringar toohello server i mellanlagringsläge.

Hello mellanlagringsläge är olika för de med kunskap om äldre sync-tekniker, eftersom hello-servern har en egen SQL-databas. Den här arkitekturen kan hello mellanlagring läge server toobe finns i olika datacenter.

### <a name="verify-hello-configuration-of-a-server"></a>Kontrollera hello konfigurationen för en server
tooapply den här metoden gör du följande:

1. [Förbereda](#prepare)
2. [Konfiguration](#configuration)
3. [Importera och synkronisera](#import-and-synchronize)
4. [Kontrollera](#verify)
5. [Växeln active server](#switch-active-server)

#### <a name="prepare"></a>Förbereda
1. Installera Azure AD Connect, Välj **mellanlagringsläge**, och avmarkera **starta synkroniseringen** på hello sista sidan i guiden för installation av hello. Det här läget kan du toorun hello Synkroniseringsmotorn manuellt.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Logga ut/logga in och starta välja från hello **synkroniseringstjänsten**.

#### <a name="configuration"></a>Konfiguration
Om du har gjort ändringar toohello primära servern och vill toocompare hello konfiguration med hello mellanlagring server, använder du [Azure AD Connect configuration Dokumenteraren](https://github.com/Microsoft/AADConnectConfigDocumenter).

#### <a name="import-and-synchronize"></a>Importera och synkronisera
1. Välj **kopplingar**, och välj hello första anslutningstjänsten med hello typen **Active Directory Domain Services**. Klicka på **kör**väljer **fullständig import**, och **OK**. Utföra de här stegen för alla kopplingar av den här typen.
2. Välj hello Connector med typen **Azure Active Directory (Microsoft)**. Klicka på **kör**väljer **fullständig import**, och **OK**.
3. Kontrollera att fliken hello kopplingar är markerad. För varje koppling med typen **Active Directory Domain Services**, klickar du på **kör**väljer **Deltasynkronisering**, och **OK**.
4. Välj hello Connector med typen **Azure Active Directory (Microsoft)**. Klicka på **kör**väljer **Deltasynkronisering**, och **OK**.

Du har nu stegvis export ändras tooAzure AD och lokala AD (om du använder Exchange-hybridinstallation). hello nästa steg kan du tooinspect vad handlar om toochange innan du börjar faktiskt hello exportera toohello kataloger.

#### <a name="verify"></a>Verifiera
1. Starta en kommandotolk och gå för`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Kör: `csexport "Name of Connector" %temp%\export.xml /f:x` hello namnet på hello Connector finns i synkroniseringstjänsten. Den har en liknande too"contoso.com namn – AAD” för Azure AD.
3. Kopiera hello PowerShell-skriptet från avsnittet hello [CSAnalyzer](#appendix-csanalyzer) tooa fil med namnet `csanalyzer.ps1`.
4. Öppna ett PowerShell-fönster och bläddra toohello mappen där du skapade hello PowerShell-skript.
5. Kör: `.\csanalyzer.ps1 -xmltoimport %temp%\export.xml`.
6. Nu har du en fil med namnet **processedusers1.csv** som kan granskas i Microsoft Excel. Alla ändringar som mellanlagras toobe exporteras tooAzure AD finns i den här filen.
7. Gör nödvändiga ändringar toohello data eller konfigurationer och kör stegen igen (importera och synkronisera och kontrollera) tills hello ändringar som är i färd toobe exporteras förväntas.

#### <a name="switch-active-server"></a>Växeln active server
1. Inaktivera hello server (DirSync/FIM/Azure AD Sync) så att den inte exporterar tooAzure AD eller Ställ in den i mellanlagringsläge (Azure AD Connect) på hello aktiva servern.
2. Köra installationsguiden för hello på hello server i **mellanlagringsläge** och inaktivera **mellanlagringsläge**.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Haveriberedskap
En del av hello implementering designen är tooplan för vilka toodo om det är en katastrof där du förlorar hello synkroniseringsserver. Det finns olika modeller toouse och vilka en toouse beror på flera faktorer, inklusive:

* Vad är din tolerans för ändringar som inte kan se tooobjects i Azure AD under hello driftstopp?
* Om du använder Lösenordssynkronisering hello användare accepterar de har toouse hello gamla lösenord i Azure AD om de ändra lokalt?
* Har du ett beroende i realtid åtgärder, till exempel tillbakaskrivning av lösenord?

Beroende på hello svar toothese frågor och organisationens princip, kan en hello följande strategier implementeras:

* Återskapa vid behov.
* En ledig belägna standby-servern, kallas även **mellanlagringsläge**.
* Använda virtuella datorer.

Om du inte använder hello inbyggda SQL Express-databas, så du bör också granska hello [hög tillgänglighet för SQL](#sql-high-availability) avsnitt.

### <a name="rebuild-when-needed"></a>Återskapa vid behov
En lönsam strategi är tooplan för återskapning av en server när det behövs. Vanligtvis installerar hello Synkroniseringsmotorn och hello inledande import och synkronisering kan slutföras inom några timmar. Om det inte en extra server, behöver du möjliga tootemporarily en domain controller toohost hello Synkroniseringsmotorn.

hello motorn synkroniseringsserver lagrar inte någon status om hello objekt så hello databasen kan byggas från hello data i Active Directory och Azure AD. Hej **sourceAnchor** attributet är används toojoin hello objekt från lokala och hello molnet. Om du återskapar hello server med befintliga objekt lokala och hello molnet, och sedan hello Synkroniseringsmotorn matchar de här objekten tillsammans igen på ominstallation. hello saker du behöver toodocument och spara är hello konfigurationsändringar toohello server, t.ex filtrering och Synkroniseringsregler. Dessa anpassade konfigurationer måste återställas innan du startar synkroniseringen.

### <a name="have-a-spare-standby-server---staging-mode"></a>Har en ledig standby server - mellanlagringsläge
Om du har en mer komplex miljö rekommenderas med en eller flera reservservrar. Under installationen, kan du aktivera en server toobe i **mellanlagringsläge**.

Mer information finns i [mellanlagringsläge](#staging-mode).

### <a name="use-virtual-machines"></a>Använda virtuella datorer
En gemensam och stöds metod är toorun hello Synkroniseringsmotorn i en virtuell dator. Om hello värden har ett problem, kan hello avbildningen med hello synkroniseringsserver motorn vara migrerade tooanother server.

### <a name="sql-high-availability"></a>Hög tillgänglighet för SQL
Om du inte använder hello SQL Server Express som medföljer Azure AD Connect, bör sedan hög tillgänglighet för SQL Server också övervägas. hello lösningar för hög tillgänglighet som stöds är SQL-kluster och AOA (alltid på Tillgänglighetsgrupper). Stöds inte lösningar omfattar spegling.

Stöd för SQL AOA har lagts till tooAzure AD Connect i version 1.1.524.0. Du måste aktivera SQL AOA innan du installerar Azure AD Connect. Under installationen av identifierar Azure AD Connect om angivna hello SQL-instansen är aktiverat för SQL AOA eller inte. Om SQL AOA är aktiverat räknat Azure AD Connect ytterligare ut om SQL AOA är konfigurerade toouse synkron eller asynkron replikeringen. När du ställer in hello tillgänglighetsgruppens lyssnare, rekommenderas att du ställer in hello RegisterAllProvidersIP egenskapen too0. Detta beror på att Azure AD Connect används för närvarande SQL Native Client tooconnect tooSQL och SQL Native Client stöder inte hello användning av MultiSubNetFailover-egenskapen.

## <a name="appendix-csanalyzer"></a>Bilaga CSAnalyzer
Avsnittet hello [Kontrollera](#verify) på hur toouse det här skriptet.

```
Param(
    [Parameter(Mandatory=$true, HelpMessage="Must be a file generated using csexport 'Name of Connector' export.xml /f:x)")]
    [string]$xmltoimport="%temp%\exportedStage1a.xml",
    [Parameter(Mandatory=$false, HelpMessage="Maximum number of users per output file")][int]$batchsize=1000,
    [Parameter(Mandatory=$false, HelpMessage="Show console output")][bool]$showOutput=$false
)

#LINQ isn't loaded automatically, so force it
[Reflection.Assembly]::Load("System.Xml.Linq, Version=3.5.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089") | Out-Null

[int]$count=1
[int]$outputfilecount=1
[array]$objOutputUsers=@()

#XML must be generated using "csexport "Name of Connector" export.xml /f:x"
write-host "Importing XML" -ForegroundColor Yellow

#XmlReader.Create won't properly resolve hello file location,
#so expand and then resolve it
$resolvedXMLtoimport=Resolve-Path -Path ([Environment]::ExpandEnvironmentVariables($xmltoimport))

#use an XmlReader toodeal with even large files
$result=$reader = [System.Xml.XmlReader]::Create($resolvedXMLtoimport) 
$result=$reader.ReadToDescendant('cs-object')
do 
{
    #create hello object placeholder
    #adding them up here means we can enforce consistency
    $objOutputUser=New-Object psobject
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name ID -Value ""
    Add-Member -InputObject $objOutputUser -MemberType NoteProperty -Name Type -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name DN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name operation -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name UPN -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name displayName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name sourceAnchor -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name alias -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name primarySMTP -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name onPremisesSamAccountName -Value ""
    Add-Member -inputobject $objOutputUser -MemberType NoteProperty -Name mail -Value ""

    $user = [System.Xml.Linq.XElement]::ReadFrom($reader)
    if ($showOutput) {Write-Host Found an exported object... -ForegroundColor Green}

    #object id
    $outID=$user.Attribute('id').Value
    if ($showOutput) {Write-Host ID: $outID}
    $objOutputUser.ID=$outID

    #object type
    $outType=$user.Attribute('object-type').Value
    if ($showOutput) {Write-Host Type: $outType}
    $objOutputUser.Type=$outType

    #dn
    $outDN= $user.Element('unapplied-export').Element('delta').Attribute('dn').Value
    if ($showOutput) {Write-Host DN: $outDN}
    $objOutputUser.DN=$outDN

    #operation
    $outOperation= $user.Element('unapplied-export').Element('delta').Attribute('operation').Value
    if ($showOutput) {Write-Host Operation: $outOperation}
    $objOutputUser.operation=$outOperation

    #now that we have hello basics, go get hello details

    foreach ($attr in $user.Element('unapplied-export-hologram').Element('entry').Elements("attr"))
    {
        $attrvalue=$attr.Attribute('name').Value
        $internalvalue= $attr.Element('value').Value

        switch ($attrvalue)
        {
            "userPrincipalName"
            {
                if ($showOutput) {Write-Host UPN: $internalvalue}
                $objOutputUser.UPN=$internalvalue
            }
            "displayName"
            {
                if ($showOutput) {Write-Host displayName: $internalvalue}
                $objOutputUser.displayName=$internalvalue
            }
            "sourceAnchor"
            {
                if ($showOutput) {Write-Host sourceAnchor: $internalvalue}
                $objOutputUser.sourceAnchor=$internalvalue
            }
            "alias"
            {
                if ($showOutput) {Write-Host alias: $internalvalue}
                $objOutputUser.alias=$internalvalue
            }
            "proxyAddresses"
            {
                if ($showOutput) {Write-Host primarySMTP: ($internalvalue -replace "SMTP:","")}
                $objOutputUser.primarySMTP=$internalvalue -replace "SMTP:",""
            }
        }
    }

    $objOutputUsers += $objOutputUser

    Write-Progress -activity "Processing ${xmltoimport} in batches of ${batchsize}" -status "Batch ${outputfilecount}: " -percentComplete (($objOutputUsers.Count / $batchsize) * 100)

    #every so often, dump hello processed users in case we blow up somewhere
    if ($count % $batchsize -eq 0)
    {
        Write-Host Hit hello maximum users processed without completion... -ForegroundColor Yellow

        #export hello collection of users as as CSV
        Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
        $objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation

        #increment hello output file counter
        $outputfilecount+=1

        #reset hello collection and hello user counter
        $objOutputUsers = $null
        $count=0
    }

    $count+=1

    #need toobail out of hello loop if no more users tooprocess
    if ($reader.NodeType -eq [System.Xml.XmlNodeType]::EndElement)
    {
        break
    }

} while ($reader.Read)

#need toowrite out any users that didn't get picked up in a batch of 1000
#export hello collection of users as as CSV
Write-Host Writing processedusers${outputfilecount}.csv -ForegroundColor Yellow
$objOutputUsers | Export-Csv -path processedusers${outputfilecount}.csv -NoTypeInformation
```

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**  

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)  
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)  
