## <a name="scenario"></a>Scenario
Det här dokumentet beskriver via en distribution som använder flera nätverkskort i virtuella datorer i ett specifikt scenario. I det här scenariot har du en två skikt IaaS arbetsbelastning i Azure. Varje nivå har distribuerats i undernätet i ett virtuellt nätverk (VNet). hello frontend-nivå består av flera webbservrar, grupperade i en belastningsutjämnare för hög tillgänglighet. hello serverdel nivå består av flera databasservrar. Dessa databasservrar kommer att distribueras med två nätverkskort, ett för åtkomst till databasen, hello andra för hantering. hello scenariot omfattar också Nätverkssäkerhetsgrupper (NSG: er) toocontrol vilken trafik tillåts tooeach undernät och NIC i hello-distribution. hello bilden nedan visar hello grundläggande arkitektur i det här scenariot.  

![MultiNIC scenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

