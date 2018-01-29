---
title: Hantera Azure-stacken storage-konton | Microsoft Docs
description: "Lär dig att hitta, hantera, återställa och frigöra Stack för Azure storage-konton"
services: azure-stack
documentationcenter: 
author: AniAnirudh
manager: darmour
editor: 
ms.assetid: 627d355b-4812-45cb-bc1e-ce62476dab34
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/6/2017
ms.author: anirudha
ms.openlocfilehash: 6e14bd6312135b45984a82099e68a934ec2a4a70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Hantera Storage-konton i Azure-stacken
Lär dig hur du hanterar storage-konton i Azure stackutrymme för att hitta, återställa och frigöra lagringskapacitet baserat på affärsbehov.

## <a name="find"></a>Hitta ett lagringskonto
Lista med lagringskonton i region kan visas i Azure-stacken genom att:

1. Navigera till https://adminportal.local.azurestack.external i en webbläsare.
2. Logga in på Azure Stack-administrationsportalen som ett moln-operatorn (med de autentiseringsuppgifter du angav under distribution)
3. Leta reda på standardinstrumentpanelen – den **Region management** listan och klicka på den region som du vill utforska. Till exempel **(lokala**).
   
   ![](media/azure-stack-manage-storage-accounts/image1.png)
4. Välj **lagring** från den **Resursproviders** lista.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
5. Nu i bladet storage Resource Provider administratör – rulla ned till den **lagringskonton** och klicka på den.
   
   ![](media/azure-stack-manage-storage-accounts/image3.png)
   
   Den resulterande sidan är listan över storage-konton i den regionen.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

Som standard visas de första 10 kontona. Du kan välja att hämta mer genom att klicka på den **läsa in mer** länken längst ned i listan.

ELLER

Om du är intresserad av ett visst lagringskonto – kan du **filtrera och hämta de relevanta kontona** endast.


**Så här filtrerar för konton:**

1. Klicka på **Filter** längst upp på bladet.
2. I bladet Filter kan du ange **kontonamn**, **prenumerations-ID** eller **status** att finjustera listan över storage-konton som ska visas. Använd dem efter behov.
3. Klicka på **uppdatering**. Därefter uppdatera listan.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Så här återställer du filtret: Klicka på **Filter**, rensa valen och uppdatera.

Sökrutan (överst på bladet storage-konton lista) kan du markera den markerade texten i listan över konton. Detta är mycket praktiska i fall när det fullständiga namnet eller ID: t inte är tillgängliga.

Du kan använda fritext här för att hitta det konto som du är intresserad av.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Titta på kontoinformation
Du kan klicka på kontot du vill visa viss information när du har hittat de konton som du är intresserad av. Öppnar ett nytt blad med kontoinformation som: typ av konto, skapelsetid, plats, osv.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Återställa ett Borttaget konto
Du kan vara i en situation där du behöver återställa ett Borttaget konto.

Det finns ett enkelt sätt att göra det i Azure Stack:

1. Bläddra till listan storage-konton. Se [hitta ett lagringskonto](#find) i det här avsnittet för mer information.
2. Leta upp det specifika kontot i listan. Du kan behöva filtrera.
3. Kontrollera den *tillstånd* för kontot. Det ska stå **borttagna**.
4. Klicka på det konto som öppnar informationsbladet konto.
5. Utöver det här bladet, leta upp den **återställa** och klicka sedan på den.
6. Bekräfta genom att klicka på **Ja**.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Återställningen är nu i *bearbeta... Vänta* för en indikation på att det lyckades.
   Du kan också klicka på ”klockikonen” överst i portalen för att visa förloppet uppgifter.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   När återställda kontot har synkroniserats kan kan den användas igen.

### <a name="some-gotchas"></a>Vissa saker
* Kontot för borttagna visar tillstånd som **utanför kvarhållning**.
  
  Detta innebär att kontot har överskridit kvarhållningsperioden och kan inte återställas.
* Din kontot visas inte i kontolistan över.
  
  Detta kan betyda att kontot redan har skräpinsamlats. I det här fallet kan den inte återställas. Se [frigöra kapacitet](#reclaim) i det här avsnittet.

## <a name="set-the-retention-period"></a>Ställa in kvarhållningsperioden
Kvarhållning period inställningen kan en moln-operatorn för att ange en tidsperiod i dagar (mellan 0 och 9 999 dagar) under vilken alla kontot potentiellt kan återställas. Loggperioden har angetts till 15 dagar. Ange värdet till ”0” betyder att alla borttagna konton är omedelbart utanför kvarhållning och har markerats för periodiska skräpinsamling.

**Ändra kvarhållningsperioden:**

1. Navigera till https://adminportal.local.azurestack.external i en webbläsare.
2. Logga in på Azure Stack-administrationsportalen som ett moln-operatorn (med de autentiseringsuppgifter du angav under distribution)
3. Leta reda på standardinstrumentpanelen – den **Region management** listan och klicka på den region som du vill utforska – till exempel **(lokala**).
4. Välj **lagring** från den **Resursproviders** lista.
5. Klicka på **inställningar** längst upp för att öppna bladet inställningen.
6. Klicka på **Configuration** redigera värdet för kvarhållning perioden.

   Ange antalet dagar och spara den.
   
   Det här värdet är omedelbart effektivt och har angetts för hela region.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Frigöra kapacitet
En av sidoeffekter för med en kvarhållningsperiod är att ett Borttaget konto fortsätter att använda kapacitet tills det kommer utanför kvarhållningsperioden. Som en moln-operator får du behöver ett sätt att frigöra utrymme för kontot även om loggperioden inte har ännu gått.

Du kan frigöra kapacitet med hjälp av portalen eller PowerShell.

**Att frigöra kapacitet med hjälp av portalen:**
1. Navigera till bladet storage-konton. Se [hitta ett lagringskonto](#find).
2. Klicka på **frigöra utrymme** längst upp på bladet.
3. Läsa meddelandet och klicka sedan på **OK**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Vänta tills den lyckas meddelandet finns på klockikonen på portalen.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Uppdatera sidan Storage-konton. Borttagna konton visas inte längre i listan, eftersom de har rensats.

Du kan också använda PowerShell för att åsidosätta explicit kvarhållningsperioden och omedelbart frigöra kapacitet.

**Att frigöra kapacitet med hjälp av PowerShell:**   

1. Bekräfta att du har Azure PowerShell installeras och konfigureras. Om inte, Använd följande instruktioner: 
   * Om du vill installera den senaste versionen av Azure PowerShell och koppla den till din Azure-prenumeration, se [hur du installerar och konfigurerar du Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Mer information om Azure Resource Manager cmdlets finns [med hjälp av Azure PowerShell med Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Kör följande cmdlet:

> [!NOTE]
> Om du kör denna cmdlet du permanent ta bort kontot och dess innehåll. Den kan inte återställas. Använd det här med försiktighet.


        Clear-ACSStorageAccount -ResourceGroupName system.local -FarmName <farm ID>


Mer information finns i [Stack Azure powershell-dokumentationen.](https://msdn.microsoft.com/library/mt637964.aspx)
 

## <a name="migrate-a-container"></a>Migrera en behållare
På grund av en ojämn lagring används av klienter, operatör molnet kan hitta en eller fler underliggande klient delar med mer utrymme än andra. Om detta inträffar kan försöka operatorn molnet frigör du utrymme på hög-resursen genom att manuellt migrera vissa blobbbehållare till en annan resurs. 

Du måste använda PowerShell för att migrera behållare.
> [!NOTE]
>BLOB-behållaren migrering har inte stöd för Direktmigrering och för närvarande är en offline-åtgärd. Under migreringen och tills den är klar underliggande blobbar i behållaren kan inte användas som är ”offline”. 

**För att migrera behållare med hjälp av PowerShell.**

1. Bekräfta att du har Azure PowerShell installeras och konfigureras. Om inte, Använd följande instruktioner:
    * Om du vill installera den senaste versionen av Azure PowerShell och koppla den till din Azure-prenumeration, se [hur du installerar och konfigurerar du Azure PowerShell](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Mer information om Azure Resource Manager cmdlets finns [med hjälp av Azure PowerShell med Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Hämta servergruppens namn: 
      
      `$farm = Get-ACSFarm -ResourceGroupName system.local`
3. Hämta resurser: 

   `$shares = Get-ACSShare -ResourceGroupName system.local -FarmName $farm.FarmName`

4. Hämta behållarna för en viss resurs. Observera att antalet och avsikt är valfria parametrar finns:
            
   `$containers = Get-ACSContainer -ResourceGroupName system.local -FarmName $farm.FarmName -ShareName $shares[0].ShareName -Count 4 -Intent Migration`  

   Granska $containers:

   `$containers`

    ![](media/azure-stack-manage-storage-accounts/image13.png)
5. Hämta de bästa mål resurserna för behållaren migreringen:

    `$destinationshares= Get-ACSSharesForMigration  -ResourceGroupName system.local -FarmName $farm.farmname -SourceShareName $shares[0].ShareName`

    Granska $destinationshares:

    `$destinationshares`

    ![](media/azure-stack-manage-storage-accounts/image14.png)
6. Startar migreringen för en behållare, meddelande om detta är en async-implementering, så kan en slinga alla behållare i en resurs och spåra statusen med returnerade jobb-id.

    `$jobId = Start-ACSContainerMigration -ResourceGroupName system.local -FarmName $farm.farmname -ContainerToMigrate $containers[1] -DestinationShareUncPath $destinationshares.UncPath`

    Granska $jobId:

   ```
   $jobId
   d1d5277f-6b8d-4923-9db3-8bb00fa61b65
   ```
7. Kontrollera status för migreringen av dess jobb-id. När behållaren migreringen är klar har MigrationStatus angetts till ”slutförd”.

    `Get-ACSContainerMigrationStatus -ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image15.png)

8. Du kan avbryta ett pågående migreringsjobb. Detta är en asynkron åtgärd igen och kan spåras i $jobid:

    `Stop-ACSContainerMigration-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId-Verbose`

    ![](media/azure-stack-manage-storage-accounts/image16.png)

    Du kan kontrollera status för migreringen Avbryt igen:

    `Get-ACSContainerMigrationStatus-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image17.png)




  
  