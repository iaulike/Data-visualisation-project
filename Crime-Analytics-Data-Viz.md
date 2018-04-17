Data-Viz-Assignment
================
Ingrid Baade
April 16, 2018

Crime Analytics
---------------

Intro paragraph which says what my primary finding is and describes two or three supporting visualisations.

``` r
SeattleData <- as_tibble(read.csv("seattle_incidents_summer_2014.csv"))
SanFranciscoData <- as_tibble(read.csv("sanfrancisco_incidents_summer_2014.csv"))

SeattleData
```

    ## # A tibble: 32,779 x 19
    ##    RMS.CDW.ID General.Offense.~ Offense.Code Offense.Code.Ex~ Offense.Type
    ##         <int>             <dbl> <fct>                   <int> <fct>       
    ##  1     483839       2015218538. 2202                        0 BURGLARY-FO~
    ##  2     481252       2015213067. 2610                        0 FRAUD-IDENT~
    ##  3     481375       2015210301. 2316                        0 THEFT-MAIL  
    ##  4     481690       2015209327. 2599                        0 COUNTERFEIT 
    ##  5     478198       2015207880. 2399                        3 THEFT-OTH   
    ##  6     480485       2015904103. 2308                        0 THEFT-BUILD~
    ##  7     470170       2015185464. 2308                        0 THEFT-BUILD~
    ##  8     465137       2015174988. 2605                        0 FRAUD-CREDI~
    ##  9     461710       2015168191. 2610                        0 FRAUD-IDENT~
    ## 10     456091       2015157266. 2606                        1 FRAUD-CHECK 
    ## # ... with 32,769 more rows, and 14 more variables:
    ## #   Summary.Offense.Code <fct>, Summarized.Offense.Description <fct>,
    ## #   Date.Reported <fct>, Occurred.Date.or.Date.Range.Start <fct>,
    ## #   Occurred.Date.Range.End <fct>, Hundred.Block.Location <fct>,
    ## #   District.Sector <fct>, Zone.Beat <fct>, Census.Tract.2000 <dbl>,
    ## #   Longitude <dbl>, Latitude <dbl>, Location <fct>, Month <int>,
    ## #   Year <int>

``` r
SanFranciscoData
```

    ## # A tibble: 28,993 x 13
    ##    IncidntNum Category     Descript       DayOfWeek Date  Time  PdDistrict
    ##         <int> <fct>        <fct>          <fct>     <fct> <fct> <fct>     
    ##  1  140734311 ARSON        ARSON OF A VE~ Sunday    08/3~ 23:50 BAYVIEW   
    ##  2  140736317 NON-CRIMINAL LOST PROPERTY  Sunday    08/3~ 23:45 MISSION   
    ##  3  146177923 LARCENY/THE~ GRAND THEFT F~ Sunday    08/3~ 23:30 SOUTHERN  
    ##  4  146177531 LARCENY/THE~ GRAND THEFT F~ Sunday    08/3~ 23:30 RICHMOND  
    ##  5  140734220 NON-CRIMINAL FOUND PROPERTY Sunday    08/3~ 23:23 RICHMOND  
    ##  6  140734349 DRUG/NARCOT~ POSSESSION OF~ Sunday    08/3~ 23:13 SOUTHERN  
    ##  7  140734349 DRUG/NARCOT~ POSSESSION OF~ Sunday    08/3~ 23:13 SOUTHERN  
    ##  8  140734349 DRIVING UND~ DRIVING WHILE~ Sunday    08/3~ 23:13 SOUTHERN  
    ##  9  140738147 OTHER OFFEN~ EVADING A POL~ Sunday    08/3~ 23:00 INGLESIDE 
    ## 10  140734258 TRESPASS     TRESPASSING    Sunday    08/3~ 23:00 CENTRAL   
    ## # ... with 28,983 more rows, and 6 more variables: Resolution <fct>,
    ## #   Address <fct>, X <dbl>, Y <dbl>, Location <fct>, PdId <dbl>

``` r
# data checks
# table(wday(mdy(SanFranciscoData$Date), label=TRUE), SanFranciscoData$DayOfWeek)
# table(month(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start)),SeattleData$Month)

City <- rep(c("Seattle","San Francisco"),c(dim(SeattleData)[1], dim(SanFranciscoData)[1]))                
Date <- date(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start))
Date <- append(Date, mdy(SanFranciscoData$Date))

Month <- SeattleData$Month
Month <- append(Month, month(mdy(SanFranciscoData$Date)))

DayOfWeek <- wday(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start), label=TRUE, abbr = FALSE)
DayOfWeek <- append(DayOfWeek, SanFranciscoData$DayOfWeek)

Hour <- hour(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start))
Hour <- append(Hour, as.numeric(substr(SanFranciscoData$Time,start=1, stop=2)))

CrimeTibble <- tibble(City = City, Date = Date, Month = Month, DayOfWeek = DayOfWeek, Hour = Hour)

CrimeTibble
```

    ## # A tibble: 61,772 x 5
    ##    City    Date       Month DayOfWeek  Hour
    ##    <chr>   <date>     <dbl>     <int> <dbl>
    ##  1 Seattle 2014-06-28    6.         7   10.
    ##  2 Seattle 2014-06-01    6.         1    0.
    ##  3 Seattle 2014-08-31    8.         1    9.
    ##  4 Seattle 2014-06-20    6.         6   13.
    ##  5 Seattle 2014-06-01    6.         1   11.
    ##  6 Seattle 2014-06-19    6.         5   14.
    ##  7 Seattle 2014-06-01    6.         1    0.
    ##  8 Seattle 2014-08-22    8.         6   11.
    ##  9 Seattle 2014-08-01    8.         6    0.
    ## 10 Seattle 2014-07-20    7.         1   12.
    ## # ... with 61,762 more rows

``` r
mapSF <- qmap("san francisco", zoom=11)
```

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=san+francisco&zoom=11&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=san%20francisco&sensor=false

    ## Warning: `panel.margin` is deprecated. Please use `panel.spacing` property
    ## instead

``` r
mapSF + geom_point(data = SanFranciscoData, aes(x = SanFranciscoData$X, y = SanFranciscoData$Y), color="red", size=.5, alpha=0.2)
```

![](Crime-Analytics-Data-Viz_files/figure-markdown_github/SanFranciscoMap-1.png)

``` r
mapS <- qmap("seattle", zoom=11)
```

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=seattle&zoom=11&size=640x640&scale=2&maptype=terrain&language=en-EN&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=seattle&sensor=false

    ## Warning: `panel.margin` is deprecated. Please use `panel.spacing` property
    ## instead

``` r
mapS + geom_point(data = SeattleData, aes(x=SeattleData$Longitude, y=SeattleData$Latitude), color="red", size=.5, alpha=.2)
```

    ## Warning: Removed 2050 rows containing missing values (geom_point).

![](Crime-Analytics-Data-Viz_files/figure-markdown_github/SeattleMap-1.png)

``` r
plot(SanFranciscoData$X,SanFranciscoData$Y)
```

![](Crime-Analytics-Data-Viz_files/figure-markdown_github/other%20stuff-1.png)

``` r
plot(SeattleData$Longitude[SeattleData$Longitude < 0],SeattleData$Latitude[SeattleData$Latitude > 0])
```

![](Crime-Analytics-Data-Viz_files/figure-markdown_github/other%20stuff-2.png)

``` r
table(weekdays(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start)))
```

    ## 
    ##    Friday    Monday  Saturday    Sunday  Thursday   Tuesday Wednesday 
    ##      4960      4587      4647      4715      4596      4612      4662

``` r
table(weekdays(mdy(SanFranciscoData$Date)))
```

    ## 
    ##    Friday    Monday  Saturday    Sunday  Thursday   Tuesday Wednesday 
    ##      4451      4005      4319      4218      3968      3930      4102

``` r
plot(sort(mdy_hms(SeattleData$Date.Reported)-mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start)))
```

![](Crime-Analytics-Data-Viz_files/figure-markdown_github/other%20stuff-3.png)

``` r
table(hour(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start)))
```

    ## 
    ##    0    1    2    3    4    5    6    7    8    9   10   11   12   13   14 
    ## 2073  989  827  589  475  393  550  811 1130 1155 1286 1281 2020 1589 1481 
    ##   15   16   17   18   19   20   21   22   23 
    ## 1697 1669 1844 1838 1738 1866 1942 1931 1605

``` r
#table(hour(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start)), weekdays(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start)))

table(hour(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start)), wday(mdy_hms(SeattleData$Occurred.Date.or.Date.Range.Start),label=TRUE, abbr = FALSE))
```

    ##     
    ##      Sunday Monday Tuesday Wednesday Thursday Friday Saturday
    ##   0     368    259     304       258      259    323      302
    ##   1     175    130      98       114      124    121      227
    ##   2     205    107     101        86       87     91      150
    ##   3     119     55      67        84       60     59      145
    ##   4      82     48      80        62       64     53       86
    ##   5      52     34      51        80       55     62       59
    ##   6      58     99      97       107       58     82       49
    ##   7      70    122     142       130      143    137       67
    ##   8      91    223     156       176      174    159      151
    ##   9     154    183     179       171      146    180      142
    ##   10    133    180     222       207      164    193      187
    ##   11    167    222     172       195      181    176      168
    ##   12    275    287     241       274      335    340      268
    ##   13    215    193     227       247      239    244      224
    ##   14    212    216     199       209      219    253      173
    ##   15    249    253     250       207      254    269      215
    ##   16    254    229     273       238      228    227      220
    ##   17    250    288     259       288      263    290      206
    ##   18    242    256     264       276      251    277      272
    ##   19    249    233     272       230      259    254      241
    ##   20    283    237     256       287      265    296      242
    ##   21    306    312     273       277      256    245      273
    ##   22    266    219     281       283      286    318      278
    ##   23    240    202     148       176      226    311      302

Including Plots
---------------

You can also embed plots, for example:

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
