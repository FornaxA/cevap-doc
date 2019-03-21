# CBitcoinAddress wrappers introduction

## Regexes to replace values

Do not replace in the transformation functions in base58.cpp

Replace encoding destinations:
`CBitcoinAddress\((.*)\).ToString\(\)`
`EncodeDestination($1)`

Replace functions that return a CBitcoinAddress:
`^CBitcoinAddress `
`CTxDestination `

Replace address strings:
`CBitcoinAddress\((.*\.GetID\(\))\)\.ToString\(\)`
`EncodeDestination($1)`

Replace CTxDestinations:
`CBitcoinAddress\((.*\.GetID\(\))\)`
`CTxDestination($1)`

Find CBitcoinAddress variables; they can *often* be replaced:
`CBitcoinAddress address\((.*)\)`
`CTxDestination dest = DecodeDestination($1)`

Find IsValid() calls; usually on 'address', but not necessarily:
`if \(\!.*\.IsValid\(\)\)`
`if (!IsValidDestination(dest))`

Find clues by searching for:
`address.Get\(\)`
`set\<CBitcoinAddress\> setAddress`
`address\.ToString\(\)`