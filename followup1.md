---
follows: coursework
---

Put your initial followup design concept here

# Language Distribution from BC to AD

{(aim|}

This visualisation of the language the name of a place/dataset entry is written in. This data allows us to understand the influence the particular culture had on the world in general in the given time period. If a place was particularly influential they would have a large proportion of the names in the dataset in their respective language.

From the data we have here we can see the trends for which languages have more ancient origins than others. Latin and Akkadian being 2 large languages before year 0. After year 0 we have italian as the largest followed not very closely by english.

An interesting occurence is that while you can see the Latin language areas move to Italian the same cannot be said for Akkadian, this could be due to it being one of the oldest languages in the Indo-Eurpoean region alongside Sumerian.

{|aim)}

```elm {v}
pieChart : Spec
pieChart =
    let
        cfg =
            configure
                << configuration (coView [ vicoStroke Nothing ])

        transBC =
            transform
                << filter (fiExpr "datum.nameLanguage != ''")
                << window [ ( [ wiAggregateOp opCount, wiField "nameLanguage" ], "langCountBC" ) ] []
                << filter (fiExpr "datum.langCountBC > 8950 && datum.minDate <= 0 ")

        transAD =
            transform
                << filter (fiExpr "datum.nameLanguage != ''")
                << window [ ( [ wiAggregateOp opCount, wiField "nameLanguage" ], "langCountAD" ) ] []
                << filter (fiExpr "datum.langCountAD > 8950 && datum.minDate > 0 ")

        encBC =
            encoding
                << position Theta
                    [ pName "langCountBC"
                    , pAggregate opSum
                    , pStack stZero
                    ]
                << color
                    [ mName "nameLanguage"
                    , mLegend
                        [ leTitle "Language Codes"
                        , leLabelFontSize 20
                        , leTitleFontSize 15
                        , leColumns 1
                        ]
                    ]

        encAD =
            encoding
                << position Theta
                    [ pName "langCountAD"
                    , pAggregate opSum
                    , pStack stZero
                    ]
                << color
                    [ mName "nameLanguage"
                    , mLegend
                        [ leTitle "Language Codes"
                        , leLabelFontSize 20
                        , leTitleFontSize 15
                        , leColumns 1
                        ]
                    ]

        pieSpec =
            asSpec [ arc [ maOuterRadius 180 ] ]

        labelEnc =
            encoding
                << text [ tName "nameLanguage" ]

        labelSpec =
            asSpec [ labelEnc [], textMark [ maRadius 190 ] ]

        viewBC =
            asSpec [ title "Before Christ" [], height 350, width 50, cfg [], names, transBC [], encBC [], layer [ pieSpec, labelSpec ] ]

        viewAD =
            asSpec [ title "Anno Domini" [], height 350, width 50, cfg [], names, transAD [], encAD [], layer [ pieSpec, labelSpec ] ]
    in
    toVegaLite [ hConcat [ viewBC, viewAD ] ]
```

| code | Language                 |
| ---- | ------------------------ |
| akk  | Akkadian                 |
| arb  | Arabic                   |
| en   | English                  |
| grc  | Greek, Ancient (to 1453) |
| hy   | Armenian                 |
| it   | Italian                  |
| la   | Latin                    |
| ru   | Russian                  |
| sux  | Sumerian                 |
| tr   | Turkish                  |
| xur  | Urartian                 |

{(vistype|}

Pie Chart

{|vistype)}

{(vismapping|}

theta nameLanguage count

{|vismapping)}

{(dataprep|}

The results with blank nameLanguages are filtered out
The names are aggregated and counted to form the field langCount which then is filtered with only languages with over (8950, number to get roughly 10 results) and by year > 0 and year < 0

{|dataprep)}

{(limitations|}

The largest language in this dataset is ' ' empty so this has an impact on the quality of the data.

{|limitations)}
