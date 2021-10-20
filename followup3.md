---
follows: followup1
---

# Distribution of structures over time

{(aim|}

To see how the latin language was being produced over the years.

Many call latin a dying language as it is no longer use in modern culture. The data here suggests that afte the year 800 it is rarely used. We see a slight resurgence after the year 2000 which is strange.

{|aim)}

{(vistype|}

Bar Chart

{|vistype)}

```elm {v}
barHist : Spec
barHist =
    let
        trans =
            transform
                << filter (fiExpr """datum.minDate >= -1000  &&  
                                        (datum.nameLanguage == 'la' )
                                                                    """)

        enc =
            encoding
                << position X [ pName "maxDate", pBin [ biStep 200 ] ]
                << position Y [ pName "nameLanguage", pAggregate opCount ]
                << color [ mName "nameLanguage" ]
    in
    toVegaLite [ height 300, width 800, names, trans [], enc [], bar [] ]
```

{(vismapping|}

X
maxDate produced with a bin size of 200

y
nameLanguage aggregated and counted together

{|vismapping)}

{(dataprep|}

filtered out when beofre any latin records were given
and set the nameLanguage to 'la' only

{|dataprep)}

{(limitations|}

This is quite a basic graph, I would like to have spent more time on it but was unable to.

{|limitations)}
