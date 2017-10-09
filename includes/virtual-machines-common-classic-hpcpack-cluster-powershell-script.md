



Beroende på din miljö och alternativ kan hello skript skapa alla hello klustret infrastrukturen, till exempel hello Azure-nätverk, storage-konton, molntjänster, domänkontrollant, fjärrdatorer eller lokala SQL-databaser, huvudnod och ytterligare kluster noder. Hello skript kan även använda befintlig Azure-infrastrukturen och skapar endast hello HPC klusternoder.

Bakgrundsinformation om hur du planerar en HPC Pack klustret finns hello [produktutvärdering och planera](https://technet.microsoft.com/library/jj899596.aspx) och [komma igång](https://technet.microsoft.com/library/jj899590.aspx) innehåll i hello HPC Pack 2012 R2 TechNet-biblioteket.

## <a name="prerequisites"></a>Krav
* **Azure-prenumeration**: du kan använda en prenumeration i antingen hello Azure Global eller Azure Kina service. Din prenumerationsbegränsningar påverkar hello antalet och typen av klusternoder som du kan distribuera. Mer information finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../articles/azure-subscription-service-limits.md).
* **Windows-klientdator med Azure PowerShell 0.8.10 eller senare installerat och konfigurerat**: se [Kom igång med Azure PowerShell](/powershell/azureps-cmdlets-docs) för installationen instruktioner och steg tooconnect tooyour Azure-prenumeration.
* **HPC Pack IaaS distributionsskriptet**: hämta och packa upp hello senaste versionen av hello skriptet från hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Kontrollera hello version av hello skriptet genom att köra `New-HPCIaaSCluster.ps1 –Version`. Den här artikeln baserat på version 4.5.2 hello skriptet.
* **Konfigurationsfilen**: skapa en XML-fil att hello skript använder tooconfigure hello HPC-kluster. Mer information och exempel finns avsnitt senare i den här artikeln och hello filen Manual.rtf som medföljer hello distributionsskriptet.

## <a name="syntax"></a>Syntax
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Kör hello skriptet som administratör.
> 
> 

### <a name="parameters"></a>Parametrar
* **ConfigFile**: Anger hello sökvägen hello configuration file toodescribe hello HPC-kluster. Läs mer om hello-konfigurationsfil i det här avsnittet eller i hello fil Manual.rtf i hello mapp hello skript.
* **AdminUserName**: Anger hello användarnamn. Om hello domänskog skapas av hello skript, blir det hello lokalt administratörsanvändarnamn för alla virtuella datorer och Hej administratör domännamn. Om det redan finns hello domänskog anger hello domänanvändare som hello lokal administratör användaren namnet tooinstall HPC Pack.
* **AdminPassword**: Anger hello administratörslösenord. Om inte anges på kommandoraden för hello efterfrågar hello skript tooinput hello lösenord.
* **HPCImageName** (valfritt): Anger hello HPC Pack VM avbildningens namn används toodeploy hello HPC-kluster. Det måste vara en Microsoft HPC Pack-avbildning från hello Azure Marketplace. Om inte angetts (rekommenderas vanligtvis), hello skript väljer hello senaste publicerade [HPC Pack 2012 R2-avbildningen](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). hello baseras senaste på Windows Server 2012 R2 Datacenter med HPC Pack 2012 R2 uppdatering 3.
  
  > [!NOTE]
  > Distributionen misslyckas om du inte anger en giltig HPC Pack-bild.
  > 
  > 
* **LogFile** (valfritt): anger loggfilssökvägen hello-distribution. Om inget anges skapar hello skriptet en loggfil i hello temp-katalogen på hello-dator som kör hello skript.
* **Framtvinga** (valfritt): förhindrar att alla hello bekräftelse meddelanden.
* **NoCleanOnFailure** (valfritt): Anger att hello Azure virtuella datorer som inte har distribuerat inte tas bort. Ta bort dessa virtuella datorer manuellt innan du köra hello skriptdistributionen toocontinue hello eller hello distributionen kan misslyckas.
* **PSSessionSkipCACheck** (valfritt): för varje molntjänst med virtuella datorer som distribueras med det här skriptet, skapas automatiskt ett självsignerat certifikat av Azure och alla hello virtuella datorer i hello Molntjänsten använda det här certifikatet som standard Windows hello Remote Management (WinRM)-certifikat. toodeploy HPC-funktioner i dessa virtuella datorer i Azure, hello skript som standard installeras tillfälligt certifikaten i hello lokal dator\\betrodda rotcertifikatutfärdare för hello klienten datorn toosuppress hello ”inte betrodd Certifikatutfärdare” säkerhet ett fel uppstod vid körning av skript. hello certifikat tas bort när hello skriptet har körts. Om den här parametern anges hello certifikat har installerats inte på klientdatorn hello och hello Säkerhetsvarning ignoreras.
  
  > [!IMPORTANT]
  > Den här parametern rekommenderas inte för Produktionsdistribution.
  > 
  > 

### <a name="example"></a>Exempel
hello följande exempel skapas ett HPC Pack kluster som använder konfigurationsfilen *MyConfigFile.xml*, och anger administratörsautentiseringsuppgifter som för att installera hello klustret.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Annat som är bra att tänka på
* hello skript kan välja att jobbet via webbportalen för hello HPC Pack eller hello HPC Pack REST API.
* hello-skript kan du köra skript före och efter konfigurationen i hello huvudnod om du vill tooinstall ytterligare programvara eller konfigurera andra inställningar.

## <a name="configuration-file"></a>Konfigurationsfilen
hello konfigurationsfilen för hello distributionsskriptet är en XML-fil. hello schemafilen HPCIaaSClusterConfig.xsd är i hello HPC Pack IaaS skript distributionsmappen. **IaaSClusterConfig** hello rotelement hello konfigurationsfil som innehåller hello underordnade element som beskrivs i detalj i hello filen Manual.rtf i hello distributionsmappen skript.

