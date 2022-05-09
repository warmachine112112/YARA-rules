# -rules
Some  rules i will add from time to time
import "pe"

rule ATM_Malware_PloutusI {
	meta:
		description = "Detects Ploutus I .NET samples based on MetabaseQ report"
		author = "Frank Boldewin (@r3c0nst)"
		reference = "https://raw.githubusercontent.com/fboldewin/YARA-rules/master/ATM.Malware.PloutusI.yar"
		date = "2021-03-03"
		hash1 = "4f6d4c6f97caf888a98a3097b663055b63e605f15ea8f7cc7347283a0b8424c1"
		hash2 = "8ca29597152dc79bcf79394e1ae2635b393d844bb0eeef6709d37e6778457b31"
		hash3 = "dce1f01c08937fb5c98964a0911de403eed2101a9d46c5eb9899755c40c3765a"
		hash4 = "3a1d992277a862640a0835af9dff4b029cfc6c5451e9716f106efaf07702a98c"
		description = "https://www.metabaseq.com/recursos/ploutus-is-back-targeting-itautec-atms-in-latin-america"
		
	strings:
		$Code = {28 ?? 02 00 06 2a}

	condition:
		filesize < 300KB and
		$Code and
		pe.pdb_path contains "Diebold.pdb" and
		pe.imports("mscoree.dll", "_CorExeMain") and
		(for any i in (0..pe.number_of_resources -1): (
			pe.resources[i].type == pe.RESOURCE_TYPE_VERSION and
			(pe.version_info["InternalName"] contains "Diebold.exe")))
} pe 

import 
diebold.exe 