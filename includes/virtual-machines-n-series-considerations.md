## <a name="deployment-considerations"></a>Distributionsöverväganden

* Tillgängligheten för virtuella datorer N-serien finns [produkter som är tillgängliga efter region](https://azure.microsoft.com/en-us/regions/services/).

* N-serien virtuella datorer kan bara distribueras i hello Resource Manager-distributionsmodellen.

* Skapa en VM N-serien med hjälp av när hello Azure-portalen på hello **grunderna** bladet väljer en **VM disktyp** av **Hårddisk**. toochoose en tillgänglig N-serien storlek på hello **storlek** bladet, klickar du på **visa alla**.

* N-serien virtuella datorer stöder inte Virtuella diskar som backas upp av Azure Premium-lagring.

* Om du vill toodeploy flera N-serien virtuella datorer kan du en prenumeration med användningsbaserad betalning eller andra köpalternativ. Om du använder ett [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/) kan du bara använda ett begränsat antal Azure Compute-kärnor.

* Du kanske behöver tooincrease hello kärnor kvoten (per region) i din Azure-prenumeration och öka hello separat kvoten för NC eller NV kärnor. toorequest en kvot ökar, [öppna en supportbegäran online customer](../articles/azure-supportability/how-to-create-azure-support-request.md) utan kostnad. Standardgränser kan variera beroende på din prenumerationskategori.

* En VM-avbildning som du kan distribuera på N-serien virtuella datorer är hello [vetenskap virtuell dator i Azure Data](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md). hello datavetenskap virtuella förinstallerar och konfigurerar många populära datavetenskap och djup learning verktyg. Det kan också förinstalleras NVIDIA Tesla GPU drivrutiner för NC-instanser.





