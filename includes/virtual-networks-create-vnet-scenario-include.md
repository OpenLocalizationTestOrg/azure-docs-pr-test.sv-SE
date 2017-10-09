## <a name="scenario"></a>Scenario
toobetter illustrerar hur toocreate ett VNet och undernät, det här dokumentet använder hello scenariot nedan.

![VNet-scenario](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

I det här scenariot skapar du ett VNet med namnet **TestVNet** med det reserverade CIDR-blocket **192.168.0.0./16**. Ditt VNet kommer innehålla hello följande undernät: 

* **FrontEnd**, som använder **192.168.1.0/24** som sitt CIDR-block.
* **BackEnd**, som använder **192.168.2.0/24** som sitt CIDR-block.

