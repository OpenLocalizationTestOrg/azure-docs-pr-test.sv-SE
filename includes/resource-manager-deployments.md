## <a name="incremental-and-complete-deployments"></a>Inkrementella och fullständiga distributioner
När du distribuerar dina resurser kan ange du att hello distribution är en inkrementell uppdatering eller en fullständig uppdatering. hello främsta skillnaden mellan dessa två lägen är hur Resource Manager hanterar befintliga resurser i resursgruppen hello som inte ingår i hello mallen:

* I Resource Manager-fullständig läge **tar bort** resurser som finns i resursgruppen hello men har inte angetts i hello mallen. 
* I Resource Manager-inkrementell läge **lämnar oförändrat** resurser som finns i resursgruppen hello men har inte angetts i hello mallen.

För båda lägena försöker Resource Manager tooprovision alla resurser som har angetts i hello mallen. Om hello resursen finns redan i hello resursgruppen och dess inställningar är desamma, resulterar hello åtgärden i ingen ändring. Om du ändrar hello inställningar för en resurs kan etableras hello resurs med de nya inställningarna. Om du försöker tooupdate hello plats eller en typ för en befintlig resurs misslyckas hello distributionen med ett fel. I stället distribuerar en ny resurs med hello plats eller ange att du behöver.

Som standard använder Resource Manager hello inkrementell läge.

tooillustrate hello skillnaden mellan inkrementella och fullständiga läge, Överväg hello följande scenario.

**Befintlig resursgrupp** innehåller:

* Resurs A
* Resurs B
* Resurs C

**Mallen** definierar:

* Resurs A
* Resurs B
* Resurs D

När de distribueras i **inkrementella** läge hello resursgruppen innehåller:

* Resurs A
* Resurs B
* Resurs C
* Resurs D

När de distribueras i **fullständig** resurs C-läge har tagits bort. hello resursgruppen innehåller:

* Resurs A
* Resurs B
* Resurs D
