
* konvertering av hello kräver en omstart av hello VM, så schemalägga hello migreringen av dina virtuella datorer för en befintlig underhållsfönster. 

* Det går inte att ångra hello konvertering. 

* Vara säker på att tootest hello konvertering. Migrera en virtuell testdator innan du utför hello migrering i produktion.

* Under hello konverteringen frigöra hello VM. hello VM får en ny IP-adress när den startas efter hello konverteringen. Om det behövs kan du [tilldela en statisk IP-adress](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) toohello VM.

* hello tas ursprungliga virtuella hårddiskar och hello storage-konto som används av hello VM innan konverteringen inte bort. De fortsätta tooincur avgifter. tooavoid att debiteras för dessa artefakter, ta bort hello ursprungliga VHD-blobbar när du har kontrollerat att hello konverteringen har slutförts.
