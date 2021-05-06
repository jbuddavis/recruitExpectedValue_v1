# Estimating the Expected Draft Value of High School Recruits

Here I assign Draft Value & Draft Monetary Value to high-school football recruits based on their composite 247 Ranking. Draft Value and Draft Monetary value are used because it can quantify the value of undrafted players (i.e., 0). Whereas if only "draft pick" is used, it is difficult to quantify undrafted players. 

# Data

The recruit dataset comprises the classes from 2010-2017 and was derived from the https://collegefootballdata.com/ API. The Draft dataset comprises years 2010-2021 and was also downloaded through the https://collegefootballdata.com/ API. Recruiting and Draft datasets were merged using the "athleteId" and "collegeAthleteId" fields and cleaned manually as best as possible.

The Draft Value data is from work by @statsbylopez https://statsbylopez.netlify.app/post/rethinking-draft-curve/. The  Draft Monetary Values are from https://www.spotrac.com/nfl/draft/. Draft Monetary Value uses the total rookie contract value as if the player were drafted in 2021. Draft Value and Draft Monetary Value are utilized for this analysis because it allows us quantify the value of undrafted players (i.e., 0). Whereas if the draft pick were used, the value to assign to undrafted players would be ambiguous. 

## Expected Draft Value

Given that only 8 drafts are included in the dataset, simply taking the average Draft Value of each pick yields too much high-frequency noise to define clear trends within the data. To help smooth these data, a rolling average was used with a binsize window +/- 5 ranks from each chosen rank. For example, for recruits ranked #15 in their class, a Draft Value was estimated by taking the average actual Draft Value of recruits ranked inclusively between #10-#20 (red line in Fig 1 & 2). 

![Figure_2](https://user-images.githubusercontent.com/75027599/117321947-8f0a3580-ae5b-11eb-9541-2eebed8cb5af.png)

The rolling average dataset yieled a clear relationship between recruit rank and draft value, where highly ranked recruits average significantly higher Draft Value than their lower ranked peers. To define an Expected Draft Value and Expected Draft Monetary Value, a blended logarithmic function was fitted to the rolling average data (black line in Fig 1 & 2). These "Expected" values follow a smoother monotonic relationship than the simple rolling average. 

![Figure_1](https://user-images.githubusercontent.com/75027599/117321890-83b70a00-ae5b-11eb-9574-b77e3c87bc6f.png)

Overall these figures help quantify the magnitude difference between high-ranked and low-ranked HS recruits in terms of likely Draft Value and Draft Contracts. **The #1 ranked High School recruit is expected to have a Draft Value of around 94.3 (~pick #31) and land a 4-Year Draft Contract of $13,000,000.** Comparatively the #300 recruit is expected to have a Draft Value of 5.7 (UNDRAFTED) and a contract of just $740,000, two-orders of magnitude lower than the #1 recruit.

## Draft Value Over Expected

The difference between a recruits *Actual Draft Value* (DV) and *Expected Draft Value* (EDV), can be expressed as *Draft Value Over Expected* (DVOE). Where low-ranked recruits that were drafted early have high DVOE. Conversely, high-ranked recruits that went undrafted have low DVOE. 

It's  important to keep in mind that both Draft Value and Draft Monetary Value follow steeply dipping exponential relationships. Most of the value is concentrated in the earliest picks. As such, there will be strong biases towards certain positions, like QB, which are prioritized in the early rounds of the draft. DVOE does not correct for these effects whatsoever.

# Shortcomings

In general, the "Expected" values are conservative estimates. In a given actual draft there is $1.6B in monetary contracts and ~10,000 in Draft Value to disperse among the  selected players. Integrating our "Expected" value for each recruiting class yields only $1.2B in monetary value, and ~8,500 in Draft Value. This would tend to create a bias towards positive DVOE. 

However, this bias is overcome by incomplete merging of the drafted dataset and the recruit dataset. To put it simply, not every recruit that was drafted, is appropriately credited with Draft Value. Over 576 draft picks were not correctly merged. As such, only ~63,200 in Draft Value is assigned out of an actual ~80,000. Our cumulative Expected Draft Value is ~67,500. **Due to not properly assigning draft credit to every player who was drafted, we have a bias towards negative DVOE throughout the dataset.** This bias is about -650 DVOE per year. As the draft and recruit datasets are improved, we will continue to update these efforts. As of this time we are limited to the data and time available. 

# Observations

## Recruits

### Top Recruits in DVOE

It's not surprising that the top recruits in DVOE are predominately QBs who were drafted very early. DVOE seems to strike a nice balance between (A) rewarding early draft picks without just listing all the #1 overall picks and (B) rewarding low-ranked players like Baker, while still recognizing the actual draft value of highly-ranked recruits Saquon.

|   | Name             | POS  | Rec Stars | Recruit Rank | Draft School | NFL Team     | Draft Year | Draft Pick |  DVOE |
|---|------------------|------|----------:|-------------:|--------------|--------------|-----------:|-----------:|------:|
| 1 | Baker Mayfield   | PRO  |         3 |         1034 | Oklahoma     | Cleveland    |       2018 |          1 | 217.5 |
| 2 | Joe Burrow       | DUAL |         4 |          281 | LSU          | Cincinnati   |       2020 |          1 | 213.3 |
| 3 | Jared Goff       | PRO  |         4 |          213 | California   | Los Angeles  |       2016 |          1 | 212.5 |
| 4 | Marcus Mariota   | DUAL |         3 |          492 | Oregon       | Tennessee    |       2015 |          2 | 209.1 |
| 5 | Blake Bortles    | PRO  |         3 |          812 | UCF          | Jacksonville |       2014 |          3 | 204.8 |
| 6 | Greg Robinson    | OT   |         4 |          124 | Auburn       | Los Angeles  |       2014 |          2 | 201.0 |
| 7 | Saquon Barkley   | RB   |         4 |          119 | Penn State   | New York     |       2018 |          2 | 200.4 |
| 8 | Quinnen Williams | DT   |         4 |          153 | Alabama      | New York     |       2019 |          3 | 197.8 |

### Bottom Recruits in DVOE

The bottom recruits in DVOE are dominated by highly-ranked linemen who either went undrafted (Trenton Thompson), or went late in the draft where actual Draft Value is low (Ronald Powell). These lowest DVOE players highlight the high Expected Draft Value of highly-ranked recruits. The fact that they received litte to no actual draft value is the exception and not the norm

|   | Name               | POS | Rec Stars | Recruit Rank | Draft School | NFL Team    | Draft Year | Draft Pick |  DVOE |
|---|--------------------|-----|----------:|-------------:|--------------|-------------|-----------:|-----------:|------:|
| 1 | Trenton Thompson   | DT  |         5 |            1 | Georgia      |             |            |            | -94.3 |
| 2 | Ronald Powell      | WDE |         5 |            1 | Florida      | New Orleans |       2014 |        169 | -80.7 |
| 3 | Martez Ivey        | OT  |         5 |            2 | Florida      |             |            |            | -76.4 |
| 4 | Seantrel Henderson | OT  |         5 |            2 | Miami        | Buffalo     |       2014 |        237 | -68.5 |
| 5 | La'el Collins      | OT  |         5 |            3 | LSU          |             |            |            | -68.5 |
| 6 | D.J. Humphries     | OT  |         5 |            3 | Florida      |             |            |            | -68.5 |
| 7 | Greg Little        | OT  |         5 |            3 | Ole Miss     |             |            |            | -68.5 |
| 8 | Shea Patterson     | PRO |         5 |            4 | Ole Miss     |             |            |            | -64.9 |

## Which Recruiting Classes Lived Up to the Hype?
Based on each recruit's DVOE, we can quantify which recruiting classes have exceeded their expectations when it came time to enter the NFL. At the very top are elite classes from Alabama and Ohio State. But I'm really happy to see some lower ranked classes in here like 2010 TAMU (Matthews & Joeckel), 2014 NCSU (Samuels & Chubb), and 2016 Iowa (Hockerson & Fant) which produced highly rated draft picks from low ranked guys. 

|      | School        | Class | Draft Value | Exp. Draft Value |   DVOE |
|------|---------------|-------|------------:|-----------------:|-------:|
| 1    | Alabama       | 2017  |      1325.1 |            530.3 |  794.8 |
| 2    | Ohio State    | 2013  |      1027.1 |            322.4 |  704.7 |
| 3    | Ohio State    | 2015  |       686.1 |              233 |  453.1 |
| 4    | Texas A&M     | 2010  |       576.7 |            135.9 |  440.8 |
| 5    | Louisville    | 2011  |       515.5 |             86.8 |  428.7 |
| 6    | NC State      | 2014  |       496.7 |               84 |  412.7 |
| 7    | Iowa          | 2016  |       387.7 |             55.1 |  332.6 |
| 8    | Washington    | 2014  |       404.3 |             78.5 |  325.8 |
| 9    | Stanford      | 2014  |       465.4 |            148.2 |  317.2 |
| 10   | Washington    | 2011  |       429.9 |            118.5 |  311.4 |
| ...  | ...           | ...   |         ... |              ... |    ... |
| 1633 | Tennessee     | 2015  |        79.9 |            262.3 | -182.4 |
| 1634 | Auburn        | 2010  |          30 |            222.7 | -192.7 |
| 1635 | Ole Miss      | 2016  |       126.5 |            322.5 |   -196 |
| 1636 | Texas         | 2012  |       121.3 |            318.7 | -197.4 |
| 1637 | Florida       | 2010  |       394.4 |            593.5 | -199.1 |
| 1638 | Florida State | 2017  |         112 |            352.8 | -240.8 |
| 1639 | Texas         | 2011  |        28.7 |            271.7 |   -243 |
| 1640 | USC           | 2013  |        61.3 |            305.5 | -244.2 |
| 1641 | USC           | 2010  |        87.8 |            359.5 | -271.7 |
| 1642 | Texas         | 2010  |        31.5 |              434 | -402.5 |

On the other side of this table we have high ranked recruiting classes that failed to produce much actual Draft Value. These classes are loaded with 5-Stars who went undrafted. In general, I'm happy with how DVOE has ended up with highly-ranked classes dispersed throughout the entirety of this table.

## Teams
At a more coarse level, we can use DVOE to look at which college programs have done the best job at getting their recruiting talent drafted. Transfers are handled by which school they were drafted from (ex. Joe Burrow's DVOE goes to LSU and not OSU). 

Perhaps not surprisingly we see the Big Three at the top. OSU, Alabama, and Clemson have all done an excellent job getting their players drafted over the last 7 years. I'm also happy to see schools like Washington, Iowa, and Louisville on here. While not recruiting powerhouses, these programs have been putting guys into the league at a higher than expected clip.

|     | School        | Draft Value | Exp. Draft Value |    DVOE |
|-----|---------------|------------:|-----------------:|--------:|
| 1   | Ohio State    |      4015.4 |           2205.1 |  1810.3 |
| 2   | Alabama       |      4635.7 |           3161.5 |  1474.2 |
| 3   | Clemson       |      2739.7 |           1361.3 |  1378.4 |
| 4   | Washington    |      1809.8 |            900.7 |   909.1 |
| 5   | Iowa          |      1205.7 |            470.1 |   735.6 |
| 6   | LSU           |      2758.7 |           2040.9 |   717.8 |
| 7   | Louisville    |      1160.1 |            532.9 |   627.2 |
| 8   | UCF           |       707.7 |            287.3 |   420.4 |
| 9   | Texas A&M     |      1648.9 |           1279.9 |   369.0 |
| 10  | NC State      |       756.3 |            451.1 |   305.2 |
| ... | ...           | ...         | ...              | ...     |
| 285 | Rutgers       | 145.7       | 437.1            | -291.4  |
| 286 | Arizona       | 75.5        | 413.5            | -338.0  |
| 287 | Ole Miss      | 819.9       | 1185.8           | -365.9  |
| 288 | Virginia      | 167.1       | 568.0            | -400.9  |
| 289 | Florida State | 1956.8      | 2376.9           | -420.1  |
| 290 | Auburn        | 1087.9      | 1602.6           | -514.7  |
| 291 | Nebraska      | 194.2       | 718.6            | -524.4  |
| 292 | USC           | 1655.4      | 2312.2           | -656.8  |
| 293 | Tennessee     | 594.8       | 1290.3           | -695.5  |
| 294 | Texas         | 524.0       | 1816.0           | -1292.0 |

On the other side of this table, we see a lot of old-guard Blue Bloods. While these programs might have recruited well relative to most P5 schools, they've certainly struggled to get guys into the league at the level expected by their recruiting. And generally these are schools which have underperformed in terms of onfield success. Also, Rutgers.

## Conferences

At the Conference Level, we see that recruits in the Big Ten and ACC are getting drafted at a higher than expected value. The SEC is slightly above predicted. While the Pac-12 and Big 12 recruits struggle to get drafted at the value suggested by their high school ranking.

| Conference        | Draft Value | Exp. Draft Value |    DVOE |
|-------------------|------------:|-----------------:|--------:|
| Big Ten           |     10720.2 |           9584.4 |  1135.8 |
| ACC               |     10935.4 |           9984.8 |   950.6 |
| American Athletic |      2727.4 |           2454.2 |   273.2 |
| SEC               |     18560.3 |          18406.2 |   154.1 |
| Big East          |         7.9 |              2.7 |     5.2 |
| FBS Independents  |      2012.0 |           2028.3 |   -16.3 |
| Mountain West     |      1247.5 |           1505.7 |  -258.2 |
| Sun Belt          |       285.8 |            695.0 |  -409.2 |
| Pac-12            |      9159.1 |           9581.7 |  -422.6 |
| Mid-American      |       808.1 |           1265.9 |  -457.8 |
| Conference USA    |       992.9 |           1469.7 |  -476.8 |
| Big 12            |      4665.3 |           6436.9 | -1771.6 |

## Coaches
### Which Coaches Have an eye for NFL Talent? 
We can look at coaches in a number of ways with DVOE. First, let's start at the beginning: *which coaches are the best at finding diamonds in the rough?* For this we'll look at the DVOE of the recruits landed by each coach (Example: Joe Burrow's DVOE would be credited to Urban Meyer). 

At the top we see our usual suspects. There are some really interesting surprises in here though: Sherman and Addazio especially.

|     | Coach           | Draft Value | Exp. Draft Value |   DVOE |
|-----|-----------------|-----------:|-----------------:|-------:|
| 1   | Urban Meyer     |     3978.1 |           2392.8 | 1585.3 |
| 2   | Nick Saban      |     4733.2 |           3206.2 | 1527.0 |
| 3   | Dabo Swinney    |     2747.6 |           1368.6 | 1379.0 |
| 4   | Kirk Ferentz    |     1215.6 |            473.5 |  742.1 |
| 5   | Mike Sherman    |      891.9 |            197.3 |  694.6 |
| 6   | Chris Petersen  |      981.9 |            464.6 |  517.3 |
| 7   | Steve Sarkisian |     1070.4 |            703.7 |  366.7 |
| 8   | Kliff Kingsbury |      573.5 |            251.5 |  322.0 |
| 9   | Steve Addazio   |      512.2 |            193.4 |  318.8 |
| 10  | Dave Doeren     |      640.1 |            321.9 |  318.2 |
| ... | ...             |        ... |              ... |    ... |
| 231 | Jeff Tedford    |       77.8 |            384.7 | -306.9 |
| 232 | Todd Graham     |      164.1 |            489.3 | -325.2 |
| 233 | Mike London     |      132.6 |            491.5 | -358.9 |
| 234 | Gus Malzahn     |      616.3 |           1006.2 | -389.9 |
| 235 | Rich Rodriguez  |       65.0 |            462.3 | -397.3 |
| 236 | Jimbo Fisher    |     1984.1 |           2381.8 | -397.7 |
| 237 | Mark Richt      |     1172.4 |           1649.5 | -477.1 |
| 238 | Butch Jones     |      461.9 |            993.4 | -531.5 |
| 239 | Bob Stoops      |      515.8 |           1134.7 | -618.9 |
| 240 | Mack Brown      |      181.5 |           1160.5 | -979.0 | 

At the bottom we see some really big names. While Jimbo's recruits have generated a tremendous amount of NFL Draft Value, they haven't lived up to the lofty expectations imposed by his highly ranked classes. One thing to note here, is that a lot of the classes from Jimbo and Mack Brown ended up being coaches/developed by their replacements (Taggart/Strong). So let's look at DVOE another way...

### Which Coaches Get Upperclassmen Drafted? 

Now DVOE is assigned to coaches based on who was the head coach at the time of their draft (Example: Joe Burrow's DVOE would be credited to Ed Orgeron). This can help us infer which coachs get their players dveloped. There is some slight shuffling at the top, as some of Urban's recruiting success is now credited to Day. In general, most of the names at the top are fairly intuitive. 

|     | Coach             | Draft Valu | Exp. Draft Value |   DVOE |
|-----|-------------------|-----------:|-----------------:|-------:|
| 1   | Nick Saban        |     4635.7 |           3161.5 | 1474.2 |
| 2   | Dabo Swinney      |     2739.7 |           1361.3 | 1378.4 |
| 3   | Urban Meyer       |     2585.7 |           1263.6 | 1322.1 |
| 4   | Chris Petersen    |     1569.9 |            675.2 |  894.7 |
| 5   | Kirk Ferentz      |     1205.7 |            470.1 |  735.6 |
| 6   | Ed Orgeron        |     2233.1 |           1596.5 |  636.6 |
| 7   | Ryan Day          |     1429.7 |            941.5 |  488.2 |
| 8   | Kevin Sumlin      |     1309.0 |            852.0 |  457.0 |
| 9   | Bobby Petrino     |      727.8 |            315.7 |  412.1 |
| 10  | Dave Doeren       |      756.3 |            451.1 |  305.2 |
| ... | ...               |        ... |              ... |    ... |
| 231 | Mike Norvell      |      364.6 |            686.9 | -322.3 |
| 232 | Mark Richt        |      584.2 |            921.3 | -337.1 |
| 233 | Bronco Mendenhall |      122.0 |            508.9 | -386.9 |
| 234 | Willie Taggart    |      415.9 |            828.0 | -412.1 |
| 235 | Clay Helton       |     1510.6 |           1943.2 | -432.6 |
| 236 | Will Muschamp     |      705.9 |           1158.2 | -452.3 |
| 237 | Gus Malzahn       |     1087.9 |           1602.6 | -514.7 |
| 238 | Tom Herman        |      318.9 |            877.0 | -558.1 |
| 239 | Jeremy Pruitt     |      153.4 |            712.9 | -559.5 |
| 240 | Charlie Strong    |      280.2 |           1147.2 | -867.0 |

At the bottom, we've replaced many of the names (ex. Mack Brown with Charlie Strong). Perhaps the most surprising are the guys that are the same: Mark Richt & Gus Malzahn. Both guys who had to play little brother to Urban/Saban in recruiting and on-field success. 

## Year to Year Accuracy of Recruit Ratings

One thing DVOE can help inform us about is the temporal improvement in ranking players. Since 2010, there has been a gradual improvement in the DVOE of playeres ranked either as 4 or 5 Stars. We can see that a Blue Chip in the 2017 recruiting class was more often drafted higher than expected. While a Blue Chip in 2010 was drafted lower than expected. This does not to seem to be an effect of merge quality between the draft and recruit datasets, for most years we are missing approximately the same number of drafted recruits. 

![BCs](https://user-images.githubusercontent.com/75027599/117348899-ca672d00-ae78-11eb-9991-309ab1775efb.png)


## Spatial Trends

We can also look at the hometowns of Blue Chips to see if recruits from some states return higher expected Draft Value compared to those from other states. To avoid small sample size issues with some states (see Taven Bryan, the Jaguars, and Wyoming), we'll look at the cumulative DVOE produced by Blue Chips from each state from recruiting classes 2010-2017. We can see that in aggregate, Blue Chips from Alabama, Louisiana, Ohio, and South Carolina tend to get drafted higher than expected based on their high school ranking. While Blue Chips from California, Texas, and Georgia have not produced their expected Draft Value. Florida, where an immense amount of Blue Chip talent comes from, generally produces Draft Value comparable to that which we expect from High School rankings.

![states](https://user-images.githubusercontent.com/75027599/117351789-1f587280-ae7c-11eb-914b-dd9f08403261.png)

## Positions 

Recruits listed at some positions tend to produce Higher Draft value than expected by their recruit ranking. Recruits listed as WDE, CB, OT, DUAL (QB), and ATH tend to do the best job at exceeding their Expected Draft Value. Conversely, OG/RB/LB/DT/WR tend to be the positions that are most likely to fall short of their expected draft position. Of these lowest-DVOE positions OG/RB/LB/WR tend to be the most logical to me. These positions tend to be undervalued in the first round of the NFL Draft [relative to NFL Rosters and the number of Blue Chips ranked by the composite](https://twitter.com/drmartylawrenc1/status/1385626436128710656/photo/1). 

DT surprises me though. Generally, DL are highly valued by the NFL Draft. Maybe this just reflects that WDE/SDE are more highly valued in the NFL? Also, keep in mind the negative bias inherent in our DVOE data. The relative-DVOE-order is what is important in the below table, not the exact value of DVOE. 


| Position | Draft Value | Exp. Draft Value | DVOE  |
|----------|-------------|------------------|-------|
| WDE      | 4691        | 3723             | 968   |
| CB       | 6444        | 5675             | 769   |
| OT       | 6838        | 6122             | 715   |
| DUAL     | 2250        | 1623             | 627   |
| ATH      | 4743        | 4434             | 309   |
| P        | 70          | 75               | -6    |
| LS       | 27          | 34               | -8    |
| OC       | 712         | 744              | -32   |
| SDE      | 3700        | 3735             | -35   |
| TE       | 2453        | 2538             | -86   |
| K        | 138         | 249              | -110  |
| FB       | 64          | 177              | -113  |
| PRO      | 2606        | 2723             | -117  |
| APB      | 1225        | 1529             | -304  |
| S        | 4428        | 4827             | -399  |
| WR       | 7720        | 8625             | -905  |
| DT       | 4156        | 5153             | -997  |
| OLB      | 3568        | 4840             | -1271 |
| ILB      | 1366        | 2652             | -1286 |
| RB       | 3266        | 4630             | -1364 |
| OG       | 1764        | 3306             | -1542 |

# Conclusions

DVOE seems to do well at quantifying how well or how poorly a football lived up to their recruit ranking when it came to the NFL draft. DVOE allows us to quantify which schools, conference, and coaches have an eye for talent and development. DVOE can be used to assess potential temporal and spatial changes in accuracy with regards to recruit rankings. Future works will leverage DVOE to focus on commonalities between recruits that are underrated or overrated coming out of high school and attempt to optimize recruit rankings for age, location, and position. 
