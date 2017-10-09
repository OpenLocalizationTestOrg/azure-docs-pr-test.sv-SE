

## <a name="deployment-considerations"></a>Distributionsöverväganden
* **Azure-prenumeration** – toodeploy mer än ett fåtal beräkningsintensiva fall bör du överväga en prenumeration med användningsbaserad betalning eller andra köpalternativ. Om du använder ett [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/) kan du bara använda ett begränsat antal Azure Compute-kärnor.

* **Priser och tillgänglighet** -storlekar för dessa Virtuella erbjuds endast i hello Standard prisnivå. Kontrollera [produkter som är tillgängliga efter region] (https://azure.microsoft.com/regions/services/) för tillgänglighet i Azure-regioner. 
* **Kärnor kvoten** – du kan behöva tooincrease hello kärnor kvot i din Azure-prenumeration från hello standardvärdet. Din prenumeration kan också begränsa hello antal kärnor som du kan distribuera i vissa VM storlek familjer, inklusive hello H-serien. toorequest en kvot ökar, [öppna en supportbegäran online customer](../articles/azure-supportability/how-to-create-azure-support-request.md) utan kostnad. (Standardgränser kan variera beroende på din prenumerationskategori.)
  
  > [!NOTE]
  > Kontakta Azure-supporten om du har stora kapacitetsbehov. Azure-kvoter är kredit begränsar inte kapacitet garantier. Oavsett din kvot endast debiteras du för kärnor att du använder.
  > 
  > 
* **Virtuellt nätverk** – en Azure [virtuellt nätverk](https://azure.microsoft.com/documentation/services/virtual-network/) är inte obligatoriska toouse hello beräkningsintensiva instanser. Men för många distributioner behöver du minst en molnbaserad Azure virtuellt nätverk eller en plats-till-plats-anslutning om du behöver tooaccess lokala resurser. Vid behov, skapa ett nytt virtuellt nätverk toodeploy hello instanser. Beräkningsintensiva VMs tooa virtuellt nätverks läggs i en tillhörighetsgrupp stöds inte.
* **Ändra storlek på** – på grund av deras speciell maskinvara, du kan bara ändra storlek på beräkningsintensiva instanser inom hello samma storlek familj (H-serien eller beräkningsintensiva A-serien). Du kan till exempel bara ändra en H-serien virtuell dator från en H-serien storlek tooanother. Dessutom stöds storleksändring från en icke-beräkningsintensiva storlek tooa beräkningsintensiva storlek inte.  
