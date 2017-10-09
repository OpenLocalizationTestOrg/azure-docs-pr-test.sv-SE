| Resurs | Standardgräns | Övre gräns |
| --- | --- | --- |
| Virtuella datorer per [prenumeration](../articles/billing-buy-sign-up-azure-subscription.md) |10 000 <sup>1</sup> per region |10 000 per region |
| Totalt antal VM-kärnor per [prenumeration](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> per region | Kontakta supporten |
| VM-kärnor per serie (Dv2, F osv.) per [prenumeration](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> per region | Kontakta supporten |
| [Medadministratörer](../articles/billing-add-change-azure-subscription-administrator.md) per prenumeration |Obegränsat |Obegränsat |
| [Storage-konton](../articles/storage/common/storage-create-storage-account.md) per prenumeration |200 |200<sup>2</sup> |
| [Resursgrupper](../articles/azure-resource-manager/resource-group-overview.md) per prenumeration |800 |800 |
| [Tillgänglighetsuppsättningar](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) per prenumeration |2 000 per region |2 000 per region |
| Läsningar med Resource Manager API |15 000 per timme |15 000 per timme |
| Skrivningar med Resource Manager API |1 200 per timme |1 200 per timme |
| Storleksförfrågningar med Resource Manager API |4 194 304 byte |4 194 304 byte |
| Taggar per prenumeration<sup>3</sup> |obegränsat |obegränsat |
| Unika taggberäkningar per prenumeration<sup>3</sup> | 10 000 | 10 000 |
| [Molntjänster](../articles/cloud-services/cloud-services-choose-me.md) per prenumeration |Inte tillämpligt<sup>4</sup> |Inte tillämpligt<sup>4</sup> |
| [Tillhörighetsgrupper](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) per prenumeration |Inte tillämpligt<sup>4</sup> |Inte tillämpligt<sup>4</sup> |

<sup>1</sup>Standardgränserna varierar beroende på erbjudandets kategorityp, t.ex. kostnadsfri utvärderingsversion, betala per användning och serie, t.ex. Dv2, F eller G.

<sup>2</sup>Detta gäller både Standard- och Premium-lagringskonton. Om du behöver mer än 200 lagringskonton skickar du en begäran via [Azure-supporten](https://azure.microsoft.com/support/faq/). hello Azure Storage-teamet kommer granska ditt företag fall och godkänna in too250 storage-konton.

<sup>3</sup>Du kan använda ett obegränsat antal taggar per prenumeration. hello antal taggar per resurs eller resursgrupp är begränsad too15. Hanteraren för filserverresurser returnerar endast en [lista över unikt taggnamn och värden](/rest/api/resources/tags#Tags_List) hello prenumeration när hello antal taggar är 10 000 eller mindre. Dock fortfarande hittar du en resurs av taggen när hello antalet överstiger 10 000.  

<sup>4</sup>funktionerna krävs inte längre med Azure-resursgrupper och hello Azure Resource Manager.

> [!NOTE]
> Det är viktigt tooemphasize där virtuella kärnor regionala överstiger samt en nationella inställningar per serie (Dv2, F, etc.) storleksgränsen som tillämpas separat.  Anta till exempel att en prenumeration i regionen Östra USA har en gräns för totalt antal VM-kärnor på 30, en gräns för antal kärnor i A-serien på 30 och en gräns för antal kärnor i D-serien på 30.  Den här prenumerationen skulle få toodeploy 30 A1 virtuella datorer eller 30 D1 virtuella datorer, eller en kombination av hello två inte tooexceed totalt 30 kärnor (till exempel 10 A1 virtuella datorer och 20 D1 virtuella datorer).  
> <!-- -->
> 
> 

