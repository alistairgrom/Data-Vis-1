---
follows: followup1
---

# Structure Type by Latitude

{(aim|}

This visualisation shows how these chosen technological advances correlate to a certain latitude.

We can see the pattern that the most advanced societies (going by the features I have chosen) are generally from Latitude 36 - 48 degrees. Correlating to the very south of greece and Northern Africa to France and Germany. This is likely due to the Empires being located around these latitudes but also the favourable conditions at this far North.

{|aim)}

```elm {v}
densityDist : Spec
densityDist =
    let
        trans =
            transform
                << filter (fiExpr " datum.reprLat > 20  && datum.reprLat < 61")

        transTemple =
            transform
                << filter (fiExpr """ datum.reprLat != '' &&
                                        datum.featureTypes == 'temple'""")

        transBridge =
            transform
                << filter (fiExpr """ datum.reprLat != '' &&
                                        datum.featureTypes == 'bridge'""")

        transMine =
            transform
                << filter (fiExpr """ datum.reprLat != '' &&
                                        datum.featureTypes == 'mine'""")

        transRoad =
            transform
                << filter (fiExpr """ datum.reprLat != '' &&
                                        datum.featureTypes == 'road'""")

        transStation =
            transform
                << filter (fiExpr """ datum.reprLat != '' &&
                                        datum.featureTypes == 'station'""")

        transAqua =
            transform
                << filter (fiExpr """ datum.reprLat != '' &&
                                        datum.featureTypes == 'aqueduct'""")

        encLat =
            encoding
                << position X
                    [ pName "reprLat"
                    , pBin [ biStep 3 ]
                    ]
                << position Y
                    [ pName "featureTypes"
                    , pAggregate opCount
                    ]
                << color [ mName "featureTypes" ]

        specLine =
            asSpec [ encLat [], transTemple [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        specLine1 =
            asSpec [ encLat [], transBridge [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        specLine2 =
            asSpec [ encLat [], transMine [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        specLine3 =
            asSpec [ encLat [], transRoad [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        specLine4 =
            asSpec [ encLat [], transStation [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]

        specLine5 =
            asSpec [ encLat [], transAqua [], line [ maInterpolate miMonotone, maPoint (pmMarker []) ] ]
    in
    toVegaLite [ height 300, width 800, places, trans [], layer [ specLine, specLine1, specLine2, specLine3, specLine4, specLine5 ] ]
```

{(vistype|}

Line Graph

{|vistype)}

{(vismapping|}

X - Latitudes

Y - Count of `featureTypes`

{|vismapping)}

{(dataprep|}

Filtering of featureTypes
Latitudes have been binned with a value of 3 per bin
Latitude has been filtered from 20 to 60 as there are only some values and the graph is odd proportionally when these are included.

{|dataprep)}

{(limitations|}

Again a checkbox feature may be better to only filter the locations you want to see.
The dataset has a large amount of unknown and blank featureTypes and also latitude columns so this could be skewing the data.

{|limitations)}
