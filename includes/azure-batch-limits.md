| **Resurs** | **Standardgräns** | **Övre gräns** |
| --- | --- | --- |
| Batch-konton per region per prenumeration | 3 |50 |
| Dedikerad kärnor per Batch-kontot (service batchläge)<sup>1</sup> | 20 | EJ TILLÄMPLIGT<sup>2</sup> |
| Låg prioritet kärnor per Batch-kontot (service batchläge)<sup>3</sup> | 50 | EJ TILLÄMPLIGT<sup>4</sup> |
| Aktiva jobb och jobbscheman<sup>5</sup> per Batch-kontot | 20 | 5000<sup>6</sup> |
| Pooler per Batch-konto | 20 | 2500 |

<sup>1</sup> dedikerade core kvoter som visas är endast för konton med poolen allokering inställd för**Batch-tjänsten**. För konton med hello inställd för**användarens prenumeration**, core kvoter baseras på hello VM kärnor kvot på regional nivå eller per VM familj i din prenumeration.

<sup>2</sup> hello antalet dedicerade kärnor per Batch-kontot kan ökas men hello högsta antalet är okänt. Kontakta Azure-supporten toodiscuss öka alternativ.

<sup>3</sup> låg prioritet core kvoter som visas är endast för konton med poolen allokering inställd för**Batch-tjänsten**. Kärnor med låg prioritet är inte tillgängligt för konton med poolen allokering inställd för**användarens prenumeration**.

<sup>4</sup> hello antalet låg prioritet kärnor per Batch-kontot kan ökas men hello högsta antalet är okänt. Kontakta Azure-supporten toodiscuss öka alternativ.

<sup>5</sup> slutförda jobb och jobbscheman begränsas inte.

<sup>6</sup> kontakta Azure-supporten om du vill toorequest öka denna gräns.
