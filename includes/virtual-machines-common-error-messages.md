>[!NOTE]
> Du kan lämna kommentarer på den här sidan för feedback eller via [Azure feedback](https://feedback.azure.com/forums/216843-virtual-machines) med #azerrormessage tagg.

## <a name="error-response-format"></a>Fel svarsformat 
Virtuella Azure-datorer använder hello följande JSON-format för felsvar:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Ett felsvar innehåller alltid en statuskod och ett fel uppstod när objektet. Varje felobjekt innehåller alltid en felkod och ett meddelande. Om hello VM har skapats med en mall, innehåller hello felobjektet också ett information avsnitt som innehåller en inre nivå av felkoder och meddelandet. Normalt sett är hello de flesta inre felmeddelande hello roten fel.


## <a name="common-virtual-machine-management-errors"></a>Vanliga virtuella management fel

Det här avsnittet innehåller hello vanliga felmeddelanden du kan stöta på när du hanterar virtuella datorer:

|  Felkod  |  Felmeddelande  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Det gick inte tooacquire lån medan disken {0}-med blobben med URI {1}. BLOB används redan.  |  
|  AllocationFailed  |  Det gick inte att allokera. Försök att minska hello VM-storlek eller antal virtuella datorer, försök igen senare eller försök att distribuera tooa annan Tillgänglighetsuppsättning eller olika Azure-plats.  |  
|  AllocationFailed  |  hello VM allokering misslyckades på grund av tooan internt fel. Försök igen senare eller försök att distribuera tooa annan plats.  |
|  ArtifactNotFound  |  hello VM-tillägg med utgivare: {0} och typ '{1}' hittades inte på platsen '{2}'.  |
|  ArtifactNotFound  |  Tillägg med utgivare {0}-, Skriv '{1}' och typhanteringsversion '{2}' kunde inte hittas i hello tilläggscentrallagret.  |
|  ArtifactVersionNotFound  |  Ingen version i artefaktcentrallagret för hello som uppfyller hello begärde version: {0}.  |
|  ArtifactVersionNotFound  |  Ingen version i artefaktcentrallagret för hello som uppfyller hello begärde version '{0}' för VM-tillägg med utgivare {1} och typ '{2}'.  |
|  AttachDiskWhileBeingDetached  |  Kan inte bifoga data disk {0}-tooVM '{1}' eftersom hello disk är bortkopplad. Vänta tills hello disken är helt frånkopplad och försök sedan igen.  |
|  BadRequest  |  Justerad ' Tillgänglighetsuppsättningar ännu inte stöds i den här regionen.  |
|  BadRequest  |  Lägga till en virtuell dator med hanterade diskar som hanteras av toonon Tillgänglighetsuppsättning eller lägga till en virtuell dator med blob baserat diskar toomanaged Tillgänglighetsuppsättning inte stöds. Skapa en Tillgänglighetsuppsättning med ”hanterad” egenskapsuppsättning i ordning tooadd en virtuell dator med tooit för hanterade diskar.  |
|  BadRequest  |  Hanterade diskar stöds inte i den här regionen.  |
|  BadRequest  |  Flera VMExtensions per hanterare stöds inte för OS Skriv: {0}. VMExtension '{1}' med hanteraren '{2}' har redan lagts till eller angetts i indata.  |
|  BadRequest  |  Åtgärden: {0} stöds inte på resursen '{1}' med hanterade diskar.  |
|  CertificateImproperlyFormatted  |  Hej hemlighetens JSON-representation som hämtades från {0} har ett datafält som inte är en korrekt formaterad PFX-fil eller angivna hello lösenordet avkoda inte hello PFX-filen korrekt.  |
|  CertificateImproperlyFormatted  |  Det går inte att deserialisera till JSON hello data som hämtats från {0}.  |
|  Konflikt  |  Ändra storlek på diskar tillåts endast när du skapar en virtuell dator eller när hello VM är frigjord.  |
|  ConflictingUserInput  |  Disk {0} kan inte bifogas eftersom hello disk ägs redan av VM-{1}'.  |
|  ConflictingUserInput  |  Käll-och målresursgrupperna är hello samma.  |
|  ConflictingUserInput  |  Käll- och mållagringskontona för disken {0} är olika.  |
|  ContainerAlreadyOnLease  |  Det finns redan ett lån på hello lagringsbehållare som innehåller hello blobben med URI {0}.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  begäran om hello att flytta resurser innehåller KeyVault-resurser som refereras av en eller flera {0} s i hello-begäran. Detta stöds inte för närvarande i mellan prenumerationer Move. Kontrollera hello felinformationen hello ID: N för KeyVault-resursen.  |
|  DiagnosticsOperationInternalError  |  Ett internt fel inträffade när diagnostikprofilen för den virtuella datorn {0} bearbetades.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  BLOB {0} används redan av en annan disk som hör tooVM '{1}'. Du kan undersöka hello blob-metadata för hello referensinformation för disken.  |
|  DiskBlobNotFound  |  Det går inte toofind VHD blob med URI {0} för disken '{1}'.  |
|  DiskBlobNotFound  |  Det går inte toofind VHD blob med URI {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} hemlighet saknar hello {1} taggar. Uppdatera hello hemliga versionen, lägga till taggar hello som krävs och försök.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Unwrap av hemliga {0}-värde med hjälp av viktiga {1} misslyckades.  |
|  DiskImageNotReady  |  Disk image {0} är i {1} tillstånd. Försök igen när avbildningen är klar.  |
|  DiskPreparationError  |  Ett eller flera fel inträffade vid förberedelse av Virtuella diskar. I avsnittet disk instans visa mer information.  |
|  DiskProcessingError  |  Disk bearbetningen stannas som hello VM har andra diskar i felaktiga diskar.  |
|  ImageBlobNotFound  |  Det går inte toofind VHD blob med URI {0} för disken '{1}'.  |
|  ImageBlobNotFound  |  Det går inte toofind VHD blob med URI {0}.  |
|  IncorrectDiskBlobType  |  Diskblobbar kan bara vara av typen sidblob. BLOB {0} för disken '{1}' är av typen blockblob.  |
|  IncorrectDiskBlobType  |  Diskblobbar kan bara vara av typen sidblob. BLOB {0} är av typen '{1}'.  |
|  IncorrectImageBlobType  |  Diskblobbar kan bara vara av typen sidblob. BLOB {0} för disken '{1}' är av typen blockblob.  |
|  IncorrectImageBlobType  |  Diskblobbar kan bara vara av typen sidblob. BLOB {0} är av typen '{1}'.  |
|  InternalOperationError  |  Det gick inte att matcha lagringskontot {0}. Kontrollera att den har skapats via hello Lagringsresursprovidern i hello samma plats som hello compute resurs.  |
|  InternalOperationError  |  {0} målsökningsuppgifter misslyckades.  |
|  InternalOperationError  |  Fel uppstod vid verifiering hello nätverksprofilen för den virtuella datorn {0}.  |
|  InvalidAccountType  |  hello AccountType {0} är ogiltig.  |
|  InvalidParameter  |  hello-värdet för parametern {0} är ogiltigt.  |
|  InvalidParameter  |  hello specificerade administratörslösenordet är inte tillåtet.  |
|  InvalidParameter  |  ”Hej angivna lösenordet måste innehålla mellan {0}-\  tecken och måste uppfylla minst {2} av kraven på lösenordskomplexitet från hello följande: <ol><li> Innehåller en versal bokstav</li><li>Innehåller en gemen bokstav</li><li>Innehåller ett numeriskt värde</li><li>Innehåller ett specialtecken.</li></ol>  |
|  InvalidParameter  |  hello specificerade administratörsanvändarnamnet är inte tillåtet.  |
|  InvalidParameter  |  Det går inte att bifoga en befintlig OS-disk om hello VM skapas från en plattform eller användaravbildning.  |
|  InvalidParameter  |  Behållaren {0} är ogiltig. Behållarnamn måste innehålla 3-63 tecken långt och får bara innehålla gemena alfanumeriska tecken och bindestreck. Bindestreck måste föregås och följas av alfanumeriska tecken.  |
|  InvalidParameter  |  Behållaren {0} i URL: en {1} är ogiltig. Behållarnamn måste innehålla 3-63 tecken långt och får bara innehålla gemena alfanumeriska tecken och bindestreck. Bindestreck måste föregås och följas av alfanumeriska tecken.  |
|  InvalidParameter  |  Hej blobbnamnet i URL-Adressen {0} innehåller ett snedstreck. Detta är för närvarande inte stöd för diskar.  |
|  InvalidParameter  |  hello URI {0} ser inte ut toobe korrekt blob-URI.  |
|  InvalidParameter  |  En disk med namnet {0} redan använder hello samma LUN: {1}.  |
|  InvalidParameter  |  En disk med namnet {0} redan finns.  |
|  InvalidParameter  |  Det går inte att ange användaravbildningsåsidosättanden för en disk som redan har definierats i hello angivna avbildningsfilen referens.  |
|  InvalidParameter  |  En disk med namnet {0} redan använder hello samma VHD URL {1}.  |
|  InvalidParameter  |  hello angivna feldomänen domän antal {0} måste ligga i hello intervallet {1} too\ {2\}.  |
|  InvalidParameter  |  hello licens typen {0} är ogiltig. Giltiga licenstyper är: Windows_Client eller Windows_Server, skiftlägeskänsligt.  |
|  InvalidParameter  |  Linux-värdnamnet får inte överskrida {0} tecken eller innehålla följande tecken hello: {1}.  |
|  InvalidParameter  |  Målsökvägen för offentliga Ssh-nycklar är för närvarande begränsad tooits standard värdet {0} på grund av tooa kända problem i Linux-etableringsagenten.  |
|  InvalidParameter  |  Det finns redan en disk på LUN {0}.  |
|  InvalidParameter  |  Prenumerationen {0} för hello begäran måste matcha hello prenumeration {1} i hello hanterade disk-id.  |
|  InvalidParameter  |  Anpassade data i OSProfile måste vara Base64-kodade och bestå av högst {0} tecken.  |
|  InvalidParameter  |  Blobbnamnet i URL-Adressen {0} måste avslutas med tillägget '{1}'.  |
|  InvalidParameter  |  {0}' är inte ett giltigt avbildade VHD blob namnprefix. Ett giltigt prefix matchar regex '{1}'.  |
|  InvalidParameter  |  Certifikat kan inte läggas tooyour VM om hello VM-agenten inte har etablerats.  |
|  InvalidParameter  |  Det finns redan en disk på LUN {0}.  |
|  InvalidParameter  |  Toocreate hello VM är eftersom hello begärde storlek {0} inte tillgänglig i hello kluster där hello tillgänglighetsuppsättning är allokerad. hello tillgängliga storlekar är: {1}. Läs mer på VM storleksändring strategi på https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  hello begärda VM-storlek {0} inte är tillgänglig i hello aktuella området. hello storlekar som finns tillgängliga i hello aktuella området är: {1}. Lär dig mer om hello tillgängliga storlekar på VM i varje region på https://aka.ms/azure-regions.  |
|  InvalidParameter  |  hello begärda VM-storlek {0} inte är tillgänglig i hello aktuella området. Lär dig mer om hello tillgängliga storlekar på VM i varje region på https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Windows-administratörsanvändarnamnet får inte innehålla fler än {0} tecken långa, sluta med en period(.) eller innehålla följande tecken hello: {1}.  |
|  InvalidParameter  |  Windows-datornamn får inte innehålla fler än {0} tecken långa, vara helt numeriskt eller innehålla följande tecken hello: {1}.  |
|  MissingMoveDependentResources  |  begäran om hello att flytta resurser innehåller inte alla hello beroende resurser. Kontrollera felinformationen för resurs-ID: n som saknas.  |
|  MoveResourcesHaveInvalidState  |  begäran om hello att flytta resurser innehåller virtuella datorer som är associerade med ogiltig storage-konton. Kontrollera information om dessa resurs-ID och refererade lagringskontonamn.  |
|  MoveResourcesHavePendingOperations  |  begäran om hello att flytta resurser innehåller resurser som en åtgärd väntar. Kontrollera information om dessa resurs-ID: n. Gör om åtgärden när du slutför hello väntande åtgärder.  |
|  MoveResourcesNotFound  |  hello flytta resurser innehåller resurser som inte kan hittas. Kontrollera information om dessa resurs-ID: n.  |
|  NetworkingInternalOperationError  |  Okänt nätverkstilldelningsfel.  |
|  NetworkingInternalOperationError  |  Okänt nätverkstilldelningsfel  |
|  NetworkingInternalOperationError  |  Ett internt fel uppstod vid bearbetningen av nätverksprofilen för hello VM.  |
|  notFound  |  Hej Tillgänglighetsuppsättningen {0} kan inte hittas.  |
|  notFound  |  Virtuella källdatorn '{0}' i hello begäran finns inte i den här Azure-plats.  |
|  notFound  |  Det gick inte att hitta klienten med ID:t {0}.  |
|  notFound  |  hello avbildningen {0} kan inte hittas.  |
|  NotSupported  |  hello licenstypen är {0}, men hello avbildningen blob {1} är inte lokal.  |
|  OperationNotAllowed  |  Tillgänglighet Set {0} kan inte tas bort. Innan du tar bort en Tillgänglighetsuppsättning se till att den inte innehåller någon virtuell dator.  |
|  OperationNotAllowed  |  Ändra tillgänglighet ange SKU från 'Justerad' too'Classic' är inte tillåtet.  |
|  OperationNotAllowed  |  Det går inte att ändra tillägg i hello VM när hello VM inte körs.  |
|  OperationNotAllowed  |  hello Capture-åtgärd stöds endast på en virtuell dator med blob baserat diskar. Använd hello 'Image' resurs-API: er toocreate en bild från en hanterad virtuell dator.  |
|  OperationNotAllowed  |  hello resurs {0} kan inte skapas från avbildningen {1} tills bilden har skapats.  |
|  OperationNotAllowed  |  Uppdateringar tooencryptionSettings tillåts inte när VM är tilldelad, försök igen när den har frigjorts  |
|  OperationNotAllowed  |  Lägga till en tooa för hanterade diskar VM med blob baserat diskar stöds inte.  |
|  OperationNotAllowed  |  hello maximala antalet datadiskar tillåtna toobe kopplade tooa VM av den här storleken är {0}.  |
|  OperationNotAllowed  |  Lägga till en blob baserat disk tooVM med hanterade diskar stöds inte.  |
|  OperationNotAllowed  |  Åtgärden '{0}' tillåts inte på avbildning '{1}' eftersom hello avbildningen har markerats för borttagning. Du kan bara försök hello borttagningsåtgärd (eller vänta på ett pågående en toocomplete).  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten på den virtuella datorn '{1}' eftersom hello VM är generaliserad.  |
|  OperationNotAllowed  |  Åtgärden '{0}' tillåts inte som återställning punkt samlingen '{1}' är markerad för borttagning.  |
|  OperationNotAllowed  |  Åtgärden '{0}' tillåts inte för VM-tillägget '{1}' eftersom den har markerats för borttagning. Du kan bara försök hello borttagningsåtgärd (eller vänta på ett pågående en toocomplete).  |
|  OperationNotAllowed  |  Åtgärden '{0}' tillåts inte eftersom {1}' hello virtuella datorer, som tillhandahålls med hello bild '{2}'.  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten eftersom den används hello virtuella ScaleSet '{1}' hello bild '{2}'.  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten på den virtuella datorn '{1}' eftersom hello VM har markerats för borttagning. Du kan bara försök hello borttagningsåtgärd (eller vänta på ett pågående en toocomplete).  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten på VM {1} eftersom hello VM har frigjorts eller markerats toobe frigjorts.  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten på den virtuella datorn '{1}' eftersom hello virtuella datorn körs. Ta stänga av uttryckligen om du stänger av hello virtuell dator från inuti hello gästoperativsystemet.  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten på den virtuella datorn '{1}' eftersom hello inte har frigjorts.  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten på den virtuella datorn '{1}' eftersom VM har tillägget '{2}' i ett feltillstånd.  |
|  OperationNotAllowed  |  Åtgärden: {0} är inte tillåten på den virtuella datorn '{1}' eftersom en annan åtgärd pågår.  |
|  OperationNotAllowed  |  hello åtgärden: {0} kräver hello virtuella '{1}' toobe generaliserad.  |
|  OperationNotAllowed  |  hello-åtgärden kräver hello VM toobe kör (eller ange toorun).  |
|  OperationNotAllowed  |  Disken med storlek {0} GB, vilket är mindre än hello storlek {1}GB av motsvarande disk bild, är inte tillåtet.  |
|  OperationNotAllowed  |  VM Scale Set-tillägg för hanteraren '{0}' kan bara läggas till hello tiden för VM Scale Set skapas.  |
|  OperationNotAllowed  |  VM Scale Set-tillägg för hanteraren '{0}' kan tas bort bara när hello VM Scale Set tas bort.  |
|  OperationNotAllowed  |  VM: {0} används redan av hanterade diskar.  |
|  OperationNotAllowed  |  VM: {0} tillhör too'Classic' tillgänglighetsuppsättning '{1}'. Kontrollera ange toouse 'Justerad' SKU update hello tillgänglighet och försök sedan hello konvertering.  |
|  OperationNotAllowed  |  VM som skapas från avbildningen kan inte ha blob baserat diskar. Alla diskar har toobe hanterade diskar.  |
|  OperationNotAllowed  |  Avbilda åtgärden kan inte slutföras eftersom hello VM inte har generaliserats.  |
|  OperationNotAllowed  |  Hanteringsåtgärder på VM: {0} tillåts inte eftersom Virtuella diskar som är konverterade toomanaged diskar.  |
|  OperationNotAllowed  |  En pågående åtgärd ändrar energisparläge för virtuell dator {0} too\ . Utför åtgärden {2} efter en stund.  |
|  OperationNotAllowed  |  Det går inte tooadd eller uppdatera hello VM. hello begärt {0} för VM-storleken inte kanske är tillgängliga i hello befintliga allokeringsenhet. Läs mer på VM storleksändring strategi på https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Tooresize hello VM är eftersom hello begärde storlek {0} inte tillgänglig i hello kluster där hello tillgänglighetsuppsättning är allokerad. hello tillgängliga storlekar är: {1}. Läs mer på VM storleksändring strategi på https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Tooresize hello VM är eftersom hello begärde storlek {0} inte tillgänglig i hello kluster där hello VM är allokerad. tooresize din VM too\  du frigöra (detta är Stop-åtgärd i hello Azure-portalen) och försök hello storlek igen. Läs mer på VM storleksändring strategi på https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  Operativsystemsetableringen misslyckades för den virtuella datorn {0}' eftersom hello gästoperativsystemet håller på att etableras.  |
|  OSProvisioningClientError  |  Det gick inte att OS-etableringen för VM: {0}. Information om felet: {1} Kontrollera att hello-avbildningen har förberetts på rätt (generaliserats). <ul><li>Instruktioner för Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Det gick inte att nyckelgenerering för SSH-värden. Felinformation: {0}. tooresolve problemet kontrollerar du om Linux-agenten är korrekt konfigurerad. <ul><li>Du kan kontrollera hello instruktionerna på: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Användarnamnet som angetts för hello VM är ogiltig för den här Linux-distribution. Felinformation: {0}.  |
|  OSProvisioningInternalError  |  Operativsystemsetableringen misslyckades för den virtuella datorn {0} på grund av tooan internt fel.  |
|  OSProvisioningTimedOut  |  Etablering av operativsystem för den virtuella datorn {0}-slutfördes inte inom hello tilldelad tid. hello VM kan fortfarande slutföra etableringen. Kontrollera etableringsstatusen senare.  |
|  OSProvisioningTimedOut  |  Etablering av operativsystem för den virtuella datorn {0}-slutfördes inte inom hello tilldelad tid. hello VM kan fortfarande slutföra etableringen. Kontrollera etableringsstatusen senare. Kontrollera också att hello avbildningen har förberetts på rätt (generaliserats).   <ul><li>Instruktioner för Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instruktioner för Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  Etablering av operativsystem för den virtuella datorn {0}-slutfördes inte inom hello tilldelad tid. Hello VM-gästagent upptäcktes dock körs. Detta tyder hello gäst-OS inte har korrekt förberedd toobe som används som en VM-avbildning (med CreateOption = FromImage). tooresolve problemet kan antingen använda hello VHD som är med CreateOption = bifoga eller korrekt förbereda den för användning som en bild:   <ul><li>Instruktioner för Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Instruktioner för Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  hello krävs för VM-storlek inte är tillgänglig på hello valt plats.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Resursen kan inte uppdateras just nu på grund av tooongoing plattformsuppdatering. Försök igen senare.  |
|  StorageAccountLimitation  |  Storage-konto: {0} stöder inte sidblobbar som är nödvändiga toocreate diskar.  |
|  StorageAccountLimitation  |  Lagringskontot {0} har överskridit sin tilldelade kvot.  |
|  StorageAccountLocationMismatch  |  Det gick inte att matcha lagringskontot {0}. Kontrollera att den har skapats via hello Lagringsresursprovidern i hello samma plats som hello compute resurs.  |
|  StorageAccountNotFound  |  Storage-kontot {0} hittades inte. Se till att lagringskontot inte har tagits bort och tillhör toohello samma Azure-plats som hello VM.  |
|  StorageAccountNotRecognized  |  Använd ett lagringskonto som hanteras av Lagringsresursprovidern. Användning av {0} stöds inte.  |
|  StorageAccountOperationInternalError  |  Ett internt fel uppstod vid åtkomst till lagringskontot {0}.  |
|  StorageAccountSubscriptionMismatch  |  Storage-kontot {0} hör inte toosubscription {1}.  |
|  StorageAccountTooBusy  |  Storage-konto: {0} är för upptagen för närvarande. Överväg att använda ett annat konto.  |
|  StorageAccountTypeNotSupported  |  Disk {0} använder {1} som är ett Blob storage-konto. Försök igen med ett allmänt lagringskonto.  |
|  StorageAccountTypeNotSupported  |  Storage-kontot {0} är av typen {1}. Inga Startdiagnostikinställningar har stöd för {2} lagringskontotyper.  |
|  SubscriptionNotAuthorizedForImage  |  hello prenumeration är inte auktoriserad.  |
|  TargetDiskBlobAlreadyExists  |  Det finns redan en BLOB {0}. Ange en annan blob-URI-toocreate {1}' nya tomma data disk'.  |
|  TargetDiskBlobAlreadyExists  |  Avbilda åtgärden kan inte fortsätta eftersom målet avbildningen blob {0} finns redan och hello flaggan toooverwrite VHD-blobbar inte har angetts. Ta bort hello blob eller flaggan hello toooverwrite VHD-blobbar och försök igen.  |
|  TargetDiskBlobAlreadyExists  |  Avbildningen kan inte fortsätta eftersom målavbildningsblobben {0} har en aktiv livslängd.   |
|  TargetDiskBlobAlreadyExists  |  Det finns redan en BLOB {0}. Ange en annan blob-URI som mål för disken '{1}'.  |
|  TooManyVMRedeploymentRequests  |  För många omdistributionsförfrågningar har tagits emot för den virtuella datorn {0}- eller hello virtuella datorer i hello samma tillgänglighetsuppsättning som den här virtuella datorn. Försök igen senare.  |
|  VHDSizeInvalid  |  hello angivna diskstorleksvärdet {0} för disken {1} om du med blob {2} är ogiltig. Diskstorleken måste vara mellan {3} och {4}.  |
|  VMAgentStatusCommunicationError  |  VM: {0} har inte rapporterat status för VM-agenten eller tillägg. Verifiera hello VM har en virtuell datoragent som körs och kan upprätta utgående anslutningar tooAzure lagring.  |
|  VMArtifactRepositoryInternalError  |  Ett fel uppstod vid kommunikation med hello artefakt databasen tooretrieve VM-artefaktdata.  |
|  VMArtifactRepositoryInternalError  |  Ett internt fel uppstod när hello VM artefakt data hämtades från hello artefaktcentrallagret.  |
|  VMExtensionHandlerNonTransientError  |  Hanterare: {0} har rapporterat fel för VM-tillägget '{1}' med terminal felkoden '{2}' och felmeddelande: '{3}'  |
|  VMExtensionManagementInternalError  |  Ett internt fel inträffade vid bearbetningen av VM-tillägget {0}.  |
|  VMExtensionManagementInternalError  |  Flera fel inträffade vid förberedelse hello VM-tillägg. VM-instansvyn mer information finns i.  |
|  VMExtensionProvisioningError  |  VM har rapporterat ett fel vid bearbetningen av tillägget: {0}. Felmeddelande: ”{1}”.  |
|  VMExtensionProvisioningError  |  Det gick inte att toobe har etablerats på hello VM flera VM-tillägg. Hello VM-instansvyn mer information finns i.  |
|  VMExtensionProvisioningTimeout  |  Etablering av VM-tillägget '{0}' har gått ut. Installationen av tillägget tar kanske för lång eller tillståndets status kunde inte hämtas.  |
|  VMMarketplaceInvalidInput  |  Skapar en virtuell dator från en icke-Marketplace-avbildning inte behöver Planinformation, bort ta hello planinformation i hello-begäran. OS-disknamnet är {0}.  |
|  VMMarketplaceInvalidInput  |  information om köp av hello matchar inte. Det går inte toodeploy från hello Marketplace-avbildning. OS-disknamnet är {0}.  |
|  VMMarketplaceInvalidInput  |  Skapa en virtuell dator från Marketplace-avbildning kräver planinformation i hello-begäran. OS-disknamnet är {0}.  |
|  VMNotFound  |  hello VM kan: {0} inte hittas.  |
|  VMRedeploymentFailed  |  VM: {0} Omdistributionen misslyckades på grund av tooan internt fel. Försök igen senare.  |
|  VMRedeploymentTimedOut  |  Omdistributionen av VM: {0} avslutades inte inom hello tilldelad tid. Den kan slutföras i senare. Annars kan du försöka hello-begäran.  |
|  VMStartTimedOut  |  VM: {0} kunde inte starta i hello tilldelad tid. hello VM kan fortfarande starta korrekt. Kontrollera tillståndet hello senare.  |


## <a name="next-steps"></a>Nästa steg
Om du behöver mer hjälp kan du kontakta hello Azure experter på [hello MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du lagra en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och välj **hämta stöder**.
