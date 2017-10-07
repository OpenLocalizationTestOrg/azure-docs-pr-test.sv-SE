---
title: aaaConnect Operations Manager tooLog Analytics | Microsoft Docs
description: "toomaintain din investering i System Center Operations Manager och använda utökat funktioner med logganalys, du kan integrera Operations Manager med din OMS-arbetsyta."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: magoedte
ms.openlocfilehash: b2841c7aa209fec7357dc4c8b1ff4325fdaa37ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-operations-manager-toolog-analytics"></a>Ansluta Operations Manager tooLog Analytics
toomaintain din investering i System Center Operations Manager och använda utökat funktioner med logganalys, du kan integrera Operations Manager med din OMS-arbetsyta.  På så sätt kan du utnyttja hello möjligheter i OMS medan toouse Operations Manager till:

* Fortsätta övervaka hello hälsotillståndet för din IT-tjänster med Operations Manager
* Underhåll integration med dina ITSM lösningar stöd för hantering av incidenter och problem
* Hantera hello livscykeln för agenter distribueras tooon lokala och offentliga moln IaaS-virtuella datorer som du övervakar med Operations Manager

Integrering med System Center Operations Manager lägger till värdet tooyour service operations strategi genom att använda hello hastighet och prestanda för OMS i att samla in, lagra och analysera data från Operations Manager.  OMS hjälper till att korrelera och arbete mot identifierar hello fel på problem och visa repetitioner stöd för din befintliga problem-hanteringen.   hello flexibilitet hello Sök motorn tooexamine prestanda, händelser och avisering data, med omfattande instrumentpaneler och dessa data på ett meningsfullt sätt reporting funktioner tooexpose visar hello styrkan OMS ger i complimenting Operations Manager.

hello-agenter som rapporterar toohello Operations Manager-hanteringsgruppen samla in data från dina servrar baserat på hello logganalys-datakällor och lösningar som du har aktiverat i OMS-prenumeration.  Beroende på hello-lösning som du har aktiverat är data från dessa lösningar antingen skickas direkt från en hanteringsserver för Operations Manager toohello OMS-webbtjänsten eller på grund av hello mängden data som samlas in på hello hanteras med agent system skickas direkt från hello agent tooOMS webbtjänsten. hello management-servern vidarebefordrar hello OMS data direkt toohello OMS webbtjänsten; den skrivs aldrig toohello OperationsManager eller OperationsManagerDW-databasen.  När en hanteringsserver förlorar anslutningen till hello OMS-webbtjänsten, cachelagrar hello data lokalt tills kommunikation har återupprättats med OMS.  Om hello management-servern är offline på grund av tooplanned underhåll eller oplanerade avbrott, återupptar en annan hanteringsserver i hanteringsgruppen hello anslutningen till OMS.  

hello visar följande diagram hello anslutning mellan hello hanteringsservrar och agenter i System Center Operations Manager-hanteringsgruppen och OMS, inklusive hello riktning och portar.   

![OMS-operationer-manager-integrering-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

Om din IT-säkerhetsprinciper inte tillåter datorer på ditt nätverk tooconnect toohello Internet, kan hanteringsservrar vara konfigurerade tooconnect toohello OMS Gateway tooreceive konfigurationsinformation och skicka insamlade data beroende hello lösning har aktiverat.  Mer information och anvisningar om hur tooconfigure Operations Manager management group toocommunicate via en OMS-Gateway toohello OMS-tjänst, se [ansluta datorer tooOMS med hello OMS Gateway](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Systemkrav
Granska hello följande information tooverify förutsättningar vara uppfyllda innan du börjar.

* OMS har bara stöd för Operations Manager 2016, UR6 för Operations Manager 2012 SP1 och senare, och Operations Manager 2012 R2 UR2 och större.  Stöd för proxy har lagts till i Operations Manager 2012 SP1 UR7 och Operations Manager 2012 R2 UR3.
* Alla Operations Manager-agenter måste uppfylla krav på minsta. Kontrollera att agenter på hello minsta uppdatering, annars trafik för Windows-agenten misslyckas och många fel kan fylla hello Operations Manager-händelseloggen.
* En OMS-prenumeration.  Mer information finns [Kom igång med logganalys](log-analytics-get-started.md).

### <a name="network"></a>Nätverk
hello informationen under listan hello proxy- och brandväggsinställningarna konfigurationsinformation krävs för hello Operations Manager-agenten och hanteringsservrar Operations-konsolen toocommunicate med OMS.  Trafik från varje komponent är utgående från din nätverkstjänst toohello OMS.     

|Resurs | Portnummer| Kringgå http-kontroll|  
|---------|------|-----------------------|  
|**Agent**|||  
|\*.ods.opinsights.azure.com| 443 |Ja|  
|\*.oms.opinsights.azure.com| 443|Ja|  
|\*.blob.core.windows.net| 443|Ja|  
|\*.azure-automation.net| 443|Ja|  
|**Hanteringsserver**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Ja|  
|\*.ods.opinsights.azure.com| 443| Ja|  
|*.azure-automation.net | 443| Ja|  
|**Operations Manager-konsolen tooOMS**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80 och 443||  
|\*.microsoft.com| 80 och 443||  
|\*.microsoftonline.com| 80 och 443||  
|\*.mms.microsoft.com| 80 och 443||  
|login.windows.net| 80 och 443||  


## <a name="connecting-operations-manager-toooms"></a>Ansluta Operations Manager tooOMS
Utför följande serie steg tooconfigure hello din Operations Manager management group tooconnect tooone OMS-arbetsytor.

1. I hello Operations Manager-konsolen väljer du hello **Administration** arbetsytan.
2. Expandera hello Operations Management Suite-noden och klicka på **anslutning**.
3. Klicka på hello **registrera tooOperations Management Suite** länk.
4. På hello **guiden Operations Management Suite Onboarding: autentisering** , ange hello e-postadress eller telefonnummer och lösenordet för administratörskontot för hello som är associerad med din OMS-prenumeration och klicka på  **Logga in**.
5. När du har autentiserats, på hello **guiden Operations Management Suite Onboarding: Välj arbetsyta** sidan som du är uppmanas tooselect OMS-arbetsyta.  Om du har mer än en arbetsytan, Välj hello arbetsytan som du vill tooregister hello Operations Manager-hanteringsgruppen från hello listrutan och klicka sedan på **nästa**.
   
   > [!NOTE]
   > Operations Manager stöder bara en OMS-arbetsytan i taget. hello anslutning och hello-datorer som var registrerade tooOMS med hello tidigare arbetsytan tas bort från OMS.
   > 
   > 
6. På hello **guiden Operations Management Suite Onboarding: Sammanfattning** , bekräfta dina inställningar och om de är korrekt, klickar du på **skapa**.
7. På hello **guiden Operations Management Suite Onboarding: Slutför** klickar du på **Stäng**.

### <a name="add-agent-managed-computers"></a>Lägg till datorer som hanteras med agent
När du har konfigurerat integrering med din OMS-arbetsyta detta endast upprättar en anslutning med OMS, inga data samlas in från hello agenter som rapporterar tooyour hanteringsgruppen. Detta sker inte förrän efter att du konfigurerar vilka specifika datorer som hanteras med agent samlar in data för Log Analytics. Du kan antingen välja hello datorobjekt individuellt eller så kan du välja en grupp som innehåller Windows-datorobjekt. Du kan inte välja en grupp som innehåller instanser av en annan klass, till exempel logiska diskar eller SQL-databaser.

1. Öppna hello Operations Manager-konsolen och välj hello **Administration** arbetsytan.
2. Expandera hello Operations Management Suite-noden och klicka på **anslutning**.
3. Klicka på hello **lägga till en datorgrupp** länken under hello åtgärder rubriken hello-höger på hello-fönstret.
4. I hello **datorsökning** dialogrutan som du kan söka efter datorer eller grupper som övervakas av Operations Manager. Välj datorer eller grupper tooonboard tooOMS, klicka på **Lägg till**, och klicka sedan på **OK**.

Du kan visa datorer och grupper konfigurerade toocollect data från hello datorer som hanteras med noden under Operations Management Suite i hello **Administration** arbetsytan hello Operations-konsolen.  Härifrån kan du lägga till eller ta bort datorer och grupper som behövs.

### <a name="configure-oms-proxy-settings-in-hello-operations-console"></a>Konfigurera proxyinställningar för OMS i hello Operations-konsolen
Utför följande steg om en intern proxyserver mellan hello hanteringsgrupp och OMS-webbtjänsten hello.  De här inställningarna hanteras centralt från hello hanteringsgruppen och distribuerade tooagent-hanterade system som ingår i hello scope toocollect data för OMS.  Detta är fördelaktigt för när vissa lösningar kringgå hello management server och skicka data direkt tooOMS webbtjänsten.

1. Öppna hello Operations Manager-konsolen och välj hello **Administration** arbetsytan.
2. Expandera Operations Management Suite och klicka sedan på **anslutningar**.
3. I hello OMS anslutning vyn klickar du på **konfigurera proxyservern**.
4. På **guiden Operations Management Suite: proxyserver** väljer **använder en proxy server tooaccess hello Operations Management Suite**, och skriv sedan hello URL: en med hello portnummer, till exempel http:// corpproxy:80 och klicka sedan på **Slutför**.

Om proxyservern kräver autentisering, utföra hello följande steg tooconfigure autentiseringsuppgifter och inställningar som behöver toopropagate toomanaged datorer som rapporterar tooOMS i hello management group.

1. Öppna hello Operations Manager-konsolen och välj hello **Administration** arbetsytan.
2. Under **Kör som-konfiguration** väljer du **Profiler**.
3. Öppna hello **System Center Advisor Run As Profile Proxy** profil.
4. Klicka på Lägg till toouse kör som-konto i hello guiden kör som-profilen. Du kan skapa en [kör som-konto](https://technet.microsoft.com/library/hh321655.aspx) eller Använd ett befintligt konto. Det här kontot måste toohave tillräckliga behörigheter toopass via hello proxyserver.
5. tooset Hej konto toomanage, Välj **en vald klass, grupp eller objekt**, klicka på **Välj...** och klicka sedan på **grupp...** tooopen hello **grupp Sök** rutan.
6. Sök efter och välj sedan **övervakning servergrupp för Microsoft System Center Advisor**.  Klicka på **OK** när du har valt hello grupp tooclose hello **grupp Sök** rutan.
7. Klicka på **OK** tooclose hello **Lägg till ett kör som-konto** rutan.
8. Klicka på **spara** toocomplete hello guiden och spara ändringarna.

När hello anslutning skapas och du kan konfigurera vilka agenter som ska samla in och rapportera data tooOMS används hello följande konfiguration i hello management-grupp, inte nödvändigtvis i ordning:

* hello kör som-konto **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** skapas.  Den är associerad med hello-kör som-profilen **Microsoft System Center Advisor kör som-profilen Blob** och är på två klasser - **insamlingsserver** och **Operations Manager-Hanteringsgrupp** .
* Två kopplingar skapas.  hello först heter **Microsoft.SystemCenter.Advisor.DataConnector** och konfigureras automatiskt med en prenumeration som vidarebefordrar alla varningar som genereras av instanser av alla klasser i hello management group tooOMS logg Analytics. hello andra connector är **Advisor Connector**, som ansvarar för kommunikation med OMS-webbtjänst och delning av data.
* Agenter och grupper som du har valt toocollect data i hello management group läggs toohello **övervakning servergrupp för Microsoft System Center Advisor**.

## <a name="management-pack-updates"></a>Uppdateringar av hanteringspaket
När konfigurationen är klar upprättar hello Operations Manager-hanteringsgruppen en anslutning med hello OMS-tjänsten.  hello hanteringsservern synkroniserar hello webbtjänsten och ta emot uppdaterade konfigurationsinformation i hello form av hanteringspaket för hello lösningar som du har aktiverat som integrerar med Operations Manager.   Operations Manager söker efter uppdateringar för dessa hanteringspaket och automatiskt hämta och importerar dem när de är tillgängliga.  Det finns två regler särskilt som styra detta:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -uppdaterar hello grundläggande OMS-hanteringspaket. Som standard körs var 12: e timme.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** -uppdaterar lösning hanteringspaket som är aktiverad i din arbetsyta. Körs var fem (5): e minut som standard.

Du kan åsidosätta de här två reglerna tooeither förhindra automatisk hämtning genom att inaktivera dem eller ändra hello frekvens för hur ofta hello hanteringsservern synkroniseras med OMS toodetermine om ett nytt management pack är tillgänglig och ska hämtas.  Gör hello [hur tooOverride en regel eller Övervakare](https://technet.microsoft.com/library/hh212869.aspx) toomodify hello **frekvens** parameter med ett värde i sekunder toochange hello synkroniseringsschema eller ändra hello **aktiverad**  parametern toodisable hello regler.  Mål hello åsidosätter tooall objekt i klassen Operations Manager-Hanteringsgrupp.

Om du vill toocontinue efter befintliga Ändra kontroll processen för att styra management pack-versioner i hanteringsgruppen för produktion, kan du inaktivera hello regler och aktivera dem vid specifika tidpunkter när uppdateringar är tillåtna. Om du har en utvecklings- eller QA hanteringsgruppen i din miljö och anslutningen toohello Internet, kan du konfigurera den hanteringsgruppen med en OMS-arbetsytan toosupport det här scenariot.  Detta gör du tooreview och utvärdera hello iterativ versioner av hello OMS hanteringspaket innan du släpper dem till hanteringsgruppen för produktion.

## <a name="switch-an-operations-manager-group-tooa-new-oms-workspace"></a>Växla en Operations Manager grupp tooa ny OMS-arbetsyta
1. Logga in tooyour OMS-prenumeration och skapa en arbetsyta i [Microsoft Operations Management Suite](http://oms.microsoft.com/).
2. Öppna hello Operations Manager-konsolen med ett konto som är medlem i rollen för hello Operations Manager-administratörer och väljer hello **Administration** arbetsytan.
3. Expandera Operations Management Suite och välj **anslutningar**.
4. Välj hello **konfigurera åtgärden Management Suite** länk på hello mitten sida av fönstret hello.
5. Följ hello **guiden Operations Management Suite Onboarding** och ange hello e-postadressen eller telefonnumret numret och lösenordet för administratörskontot för hello som är associerad med den nya OMS-arbetsytan.
   
   > [!NOTE]
   > Hej **guiden Operations Management Suite Onboarding: Välj arbetsyta** visar hello befintlig arbetsyta som används.
   > 
   > 

## <a name="validate-operations-manager-integration-with-oms"></a>Kontrollera Operations Manager Integration with OMS
Det finns några olika sätt som du kan kontrollera att din OMS tooOperations integrering Manager har lyckats.

### <a name="tooconfirm-integration-from-hello-oms-portal"></a>tooconfirm integration från hello OMS-portalen
1. I hello OMS-portalen klickar du på hello **inställningar** panelen
2. Välj **anslutna källor**.
3. Du bör se hello namnet på hello hanteringsgrupp visas i listan med hello antalet agenter och status när data togs emot senast i hello tabell under hello System Center Operations Manager-avsnittet.
   
   ![OMS-inställningar-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)
4. Obs hello **arbetsyte-ID** värde under hello vänster sida i hello inställningssidan.  Du kan verifiera den mot din Operations Manager-hanteringsgrupp nedan.  

### <a name="tooconfirm-integration-from-hello-operations-console"></a>tooconfirm integration från hello Operations-konsolen
1. Öppna hello Operations Manager-konsolen och välj hello **Administration** arbetsytan.
2. Välj **hanteringspaket** och i hello **letar:** Skriv text **Advisor** eller **Intelligence**.
3. Beroende på hello lösningar som du har aktiverat, kan du se ett motsvarande hanteringspaket som anges i hello sökresultat.  Om du har aktiverat hello lösning för aviseringar, till exempel finns hello hanteringspaket Microsoft System Center Advisor Alert Management i hello lista.
4. Från hello **övervakning** visa, navigera toohello **Operations hanteringstillstånd Suite\Health** vyn.  Välj en hanteringsserver under hello **Hanteringsservertillstånd** fönstret och i hello **detaljvy** rutan Bekräfta hello värdet för egenskapen **URI för autentiseringstjänst** matchar hello OMS arbetsyte-ID.
   
   ![OMS-OpsMgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-oms"></a>Ta bort Integration med OMS
När du inte längre behöver integrering mellan Operations Manager-hanteringsgruppen och OMS-arbetsyta, finns det flera steg som krävs för tooproperly ta bort hello anslutning och konfiguration i hello management group. hello har följande procedur du uppdatera din OMS-arbetsyta genom att ta bort hello-referens för hanteringsgruppen, ta bort hello OMS-kopplingar och ta sedan bort hanteringspaket stöder OMS.   

Hanteringspaket för hello lösningar som du har aktiverat som integrerar med Operations Manager och hello management packs krävs toosupport integrering med hello OMS-tjänsten kan inte enkelt tas bort från hello hanteringsgrupp.  Det beror på att vissa av hello OMS hanteringspaket har beroenden i andra relaterade management packs.  toodelete hanteringspaket med ett beroende på andra hanteringspaket hämta hello skriptet [ta bort ett hanteringspaket med beroenden](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) från TechNet Script Center.  

1. Öppna hello Operations Managers kommandotolk med ett konto som är medlem i rollen för hello Operations Manager-administratörer.
   
    > [!WARNING]
    > Verifiera du inte har några anpassade hanteringspaket med hello word Advisor eller IntelligencePack i hello namn innan du fortsätter, annars hello följande ta bort dem från hello hanteringsgruppen.
    > 

2. Hello Kommandotolken, Skriv`Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Nästa typ`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
4. tooremove management packs återstående som är beroende av andra System Center Advisor management Pack ska du använda hello skriptet *RecursiveRemove.ps1* du hämtade från tidigare hello TechNet Script Center.  
 
    > [!NOTE]
    > Ta inte bort hello Microsoft System Center Advisor eller Microsoft System Center Advisor internt hanteringspaket.  
    >  

5. Öppna hello Operations Manager Operations-konsolen med ett konto som är medlem i rollen för hello Operations Manager-administratörer.
6. Under **Administration**väljer hello **hanteringspaket** nod och i hello **letar:** skriver **Advisor** och kontrollera hello följande hanteringspaket importeras fortfarande i hanteringsgruppen:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor internt
7. I hello OMS-portalen klickar du på hello **inställningar** panelen.
8. Välj **anslutna källor**.
9. Du bör se hello namn i hello tabell under hello System Center Operations Manager-avsnittet, hello management-grupp som du vill tooremove från hello arbetsyta.  Under kolumnen hello **senaste Data**, klickar du på **ta bort**.  
   
    > [!NOTE]
    > Hej **ta bort** länken blir inte tillgängliga förrän efter 14 dagar om ingen aktivitet upptäckts från hello ansluten hanteringsgrupp.  
    > 

10. Ett fönster visas där du tooconfirm som du vill tooproceed med hello borttagningen.  Klicka på **Ja** tooproceed. 

toodelete hello två kopplingar - Microsoft.SystemCenter.Advisor.DataConnector och Advisor Connector, spara hello PowerShell-skriptet nedan tooyour datorn och köra med hello följande exempel:

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> hello-dator som du kör skriptet från, om inte en hanteringsserver, bör ha hello Operations Managers kommandotolk installerat beroende på hello version av hanteringsgruppen.
> 
> 

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with hello specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with hello specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

I framtida hello om du tänker ansluta din management group tooan OMS-arbetsyta, behöver du toore import hello `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` management pack-filen från hello senaste uppdateringen tillämpas tooyour hanteringsgruppen.  Du hittar den här filen i hello `%ProgramFiles%\Microsoft System Center 2012` eller hello `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` mapp.

## <a name="next-steps"></a>Nästa steg
tooadd funktioner och samla data, se [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).


