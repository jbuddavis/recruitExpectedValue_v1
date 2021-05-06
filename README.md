# Estimating the Expected Draft Value of High School Recruits

Here I assign Draft Value & Draft Monetary Value to high-school football recruits based on their composite 247 Ranking. Draft Value and Draft Monetary value are used because it can quantify the value of undrafted players (i.e., 0). Whereas if only "draft pick" is used, it is difficult to quantify undrafted players. 

# Data

The recruit dataset comprises the classes from 2010-2017 and was derived from the https://collegefootballdata.com/ API. The Draft dataset comprises years 2010-2021 and was also downloaded through the https://collegefootballdata.com/ API. Recruiting and Draft datasets were merged using the "athleteId" and "collegeAthleteId" fields and cleaned manually as best as possible.

The Draft Value data is from work by @statsbylopex https://statsbylopez.netlify.app/post/rethinking-draft-curve/. The  Draft Monetary Values are from https://www.spotrac.com/nfl/draft/. Draft Monetary Value uses the total rookie contract value as if the player were drafted in 2021. Draft Value and Draft Monetary Value are utilized for this analysis because it allows us quantify the value of undrafted players (i.e., 0). Whereas if the draft pick were used, the value to assign to undrafted players would be ambiguous. 

## Expected Draft Value

Given that only 8 drafts are included in the dataset, simply taking the average Draft Value of each pick yields too much high-frequency noise to define clear trends within the data. To help smooth these data, a rolling average was used with a binsize window +/- 5 ranks from each chosen rank. For example, for recruits ranked #15 in their class, a Draft Value was estimated by taking the average actual Draft Value of recruits ranked inclusively between #10-#20 (red line in Fig 1 & 2). 

![Figure_2](https://user-images.githubusercontent.com/75027599/117321947-8f0a3580-ae5b-11eb-9541-2eebed8cb5af.png)

The rolling average dataset yieled a clear relationship between recruit rank and draft value, where highly ranked recruits average significantly higher Draft Value than their lower ranked peers. To define an Expected Draft Value and Expected Draft Monetary Value, a blended logarithmic function was fitted to the rolling average data (black line in Fig 1 & 2). These "Expected" values follow a smoother monotonic relationship than the simple rolling average. 

![Figure_1](https://user-images.githubusercontent.com/75027599/117321890-83b70a00-ae5b-11eb-9574-b77e3c87bc6f.png)

Overall these figures help quantify the magnitude difference between high-ranked and low-ranked HS recruits in terms of likely Draft Value and Draft Contracts. **The #1 ranked High School recruit is expected to have a Draft Value of around 94.3 (~pick #31) and land a 4-Year Draft Contract of $13,000,000.** Comparatively the #300 recruit is expected to have a Draft Value of 5.7 (UNDRAFTED) and a contract of just $740,000, two-orders of magnitude lower than the #1 recruit.

## Draft Value Over Expected

The difference between a recruits *Actual Draft Value* (DV) and *Expected Draft Value* (EDV), can be expressed as *Draft Value Over Expected* (DVOE). Where low-ranked recruits that were drafted early have high DVOE. Conversely, high-ranked recruits that went undrafted have low DVOE. 

In general, the "Expected" values are conservative estimates. In a given actual draft there is $1.6B in monetary contracts and ~10,000 in Draft Value to disperse among the  selected players. Integrating our "Expected" value for each recruiting class yields only $1.2B in monetary value, and ~8,500 in Draft Value. This would tend to create a bias towards positive DVOE. 

However, this bias is overcome by incomplete merging of the drafted dataset and the recruit dataset. To put it simply, not every recruit that was drafted, is appropriately credited with Draft Value. Cumulatively, only ~63,200 in Draft Value is assigned out of an actual ~80,000. Our cumulative Expected Draft Value is ~67,500. **Due to not properly assigning draft credit to recruits, we have a bias towards negative DVOE throughout the dataset.** This bias is about 650 DVOE per year. 

It's also important to keep in mind that both Draft Value and Draft Monetary Value follow steeply dipping exponential relationships. Most of the value is concentrated in the earliest picks. As such, there will be strong biases towards certain positions, like QB, which are prioritized in the early rounds of the draft. DVOE does not correct for these effects whatsoever.

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

The bottom recruits in DVOE are dominated by highly-ranked linemen who either went undrafted (Trenton Thompson), or went late in the draft where actual Draft Value is low (Ronald Powerll). These lowest DVOE players highlight the high Expected Draft Value of highly-ranked recruits. The fact that they received litte to no actual draft value is the exception and not the norm

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

