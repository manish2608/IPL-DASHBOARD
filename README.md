**find season wise champions**
   ** find winner team logo**
Season Match Winner Logo = 
VAR SelectedSeasion= SELECTEDVALUE(ipl_matches_data[season])

VAR FinalMatchDate = CALCULATE(MAX(ipl_matches_data[match_date]),ipl_matches_data[season]=SelectedSeasion)

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),ipl_matches_data[season]=SelectedSeasion,ipl_matches_data[match_date]=FinalMatchDate)

RETURN
   LOOKUPVALUE(teams_data[Logo],teams_data[Team_Name],FinalMatchWinner)
   
 **find winner team**
Champions = 
VAR SelectedSeasion= SELECTEDVALUE(ipl_matches_data[season])

VAR FinalMatchDate = CALCULATE(MAX(ipl_matches_data[match_date]),ipl_matches_data[season]=SelectedSeasion)

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),ipl_matches_data[season]=SelectedSeasion,ipl_matches_data[match_date]=FinalMatchDate)

RETURN FinalMatchWinner

**find runerup wise team**
  **find renner up team**
RuneUp = 
Var  SelectSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR FinalDate =CALCULATE(MAX(ipl_matches_data[match_date]),ipl_matches_data[season]=SelectSeason)

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),ipl_matches_data[season]=SelectSeason,ipl_matches_data[match_date]=FinalDate)

VAR Team1=CALCULATE(MAX(ipl_matches_data[team1]),ipl_matches_data[match_winner]=FinalMatchWinner,ipl_matches_data[season]=SelectSeason,ipl_matches_data[match_date]=FinalDate)

VAR Team2=CALCULATE(MAX(ipl_matches_data[team2]),ipl_matches_data[match_winner]=FinalMatchWinner,ipl_matches_data[season]=SelectSeason,ipl_matches_data[match_date]=FinalDate)

RETURN
 IF(FinalMatchWinner=Team1,Team2,Team1)

**find runnerup team logo**
 RunnerUplogo = 
Var  SelectSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR FinalDate =CALCULATE(MAX(ipl_matches_data[match_date]),ipl_matches_data[season]=SelectSeason)

VAR FinalMatchWinner = CALCULATE(MAX(ipl_matches_data[match_winner]),ipl_matches_data[season]=SelectSeason,ipl_matches_data[match_date]=FinalDate)

VAR Team1=CALCULATE(MAX(ipl_matches_data[team1]),ipl_matches_data[match_winner]=FinalMatchWinner,ipl_matches_data[season]=SelectSeason,ipl_matches_data[match_date]=FinalDate)

VAR Team2=CALCULATE(MAX(ipl_matches_data[team2]),ipl_matches_data[match_winner]=FinalMatchWinner,ipl_matches_data[season]=SelectSeason,ipl_matches_data[match_date]=FinalDate)

RETURN
 IF(FinalMatchWinner=Team1,LOOKUPVALUE(teams_data[Logo],teams_data[Team_Name],Team2),LOOKUPVALUE(teams_data[Logo],teams_data[Team_Name],Team1))

 
 **find half centuries in seasnon wise whit kpi**
 
Half-Centuries = 
VAR SelectSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonData = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]))
VAR BatterRun = SUMMARIZE(SeasonData,ball_by_ball_data[match_id],ball_by_ball_data[batter],"Total Runs",SUM(ball_by_ball_data[batter_runs]))
VAR HalfCenturiesCount =FILTER(BatterRun,[Total Runs]>=50 && [Total Runs]<100)
RETURN COUNTROWS(HalfCenturiesCount)

**find  centuries in seasnon wise whit kpi**
Total Centuries = 
VAR SelectSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonData = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]))
VAR BatterRun = SUMMARIZE(SeasonData,ball_by_ball_data[match_id],ball_by_ball_data[batter],"Total Runs",SUM(ball_by_ball_data[batter_runs]))
VAR CenturiesCount =FILTER(BatterRun,[Total Runs]>=100)
RETURN COUNTROWS(CenturiesCount)

**find total match in seasnon wise whit kpi**
Total Matchs = CALCULATE(DISTINCTCOUNT(ipl_matches_data[match_id]))

**find total 4 in seasnon wise whit kpi**
Total 4's = 
 CALCULATE(COUNTROWS(ball_by_ball_data),ball_by_ball_data[batter_runs]=4,KEEPFILTERS(VALUES(ipl_matches_data[season])))

**find total 6 in seasnon wise whit kpi**
 Total 6's = 
 CALCULATE(COUNTROWS(ball_by_ball_data),ball_by_ball_data[batter_runs]=6,KEEPFILTERS(VALUES(ipl_matches_data[season])))

**find total teams in seasnon wise whit kpi**
 Total Teams = CALCULATE(DISTINCTCOUNT(ipl_matches_data[team1]))

**find total venues in seasnon wise whit kpi**
 Total Venues = CALCULATE(DISTINCTCOUNT(ipl_matches_data[venue]))


**find orange cap holder**
  **find orange ccap holder name**
 Orange Cap Holder = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data , RELATED(ipl_matches_data[season])= SelectedSeason)
VAR RunSummary =   
                  SUMMARIZE(SeasonDataOnly,ball_by_ball_data[batter],"Total Run",SUM(ball_by_ball_data[batter_runs]))
VAR MaxRuns = 
                  MAXX(RunSummary,[Total Run])
VAR TopScorer = 
                  CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(RunSummary,[Total Run]=MaxRuns))
RETURN 
        MAXX(TopScorer ,ball_by_ball_data[batter])  

 **find orange ccap holder name image**

Orange Cap Image = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data , RELATED(ipl_matches_data[season])= SelectedSeason)
VAR RunSummary =   
                  SUMMARIZE(SeasonDataOnly,ball_by_ball_data[batter],"Total Run",SUM(ball_by_ball_data[batter_runs]))
VAR MaxRuns = 
                  MAXX(RunSummary,[Total Run])
VAR TopScorer = 
                  CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(RunSummary,[Total Run]=MaxRuns))
RETURN 
 LOOKUPVALUE('players-data-updated'[player_image],'players-data-updated'[player_name], MAXX(TopScorer ,ball_by_ball_data[batter]) )

 **find orange ccap holder run**
Orange Cap Runs = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data , RELATED(ipl_matches_data[season])= SelectedSeason)
VAR RunSummary =   
                  SUMMARIZE(SeasonDataOnly,ball_by_ball_data[batter],"Total Run",SUM(ball_by_ball_data[batter_runs]))
VAR MaxRuns = 
                  MAXX(RunSummary,[Total Run])

RETURN MaxRuns

 **find orange cap team name**
Orange Cap Team Name = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data , RELATED(ipl_matches_data[season])= SelectedSeason)
VAR RunSummary =   
                  SUMMARIZE(SeasonDataOnly,ball_by_ball_data[batter],"Total Run",SUM(ball_by_ball_data[batter_runs]))
VAR MaxRuns = 
                  MAXX(RunSummary,[Total Run])
VAR TopScorer = 
                  CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(RunSummary,[Total Run]=MaxRuns))
VAR FullTeamName = 
                  CALCULATE(MAX(ball_by_ball_data[team_batting]),FILTER(SeasonDataOnly,ball_by_ball_data[batter] = MAXX(TopScorer , ball_by_ball_data[batter])))
RETURN  FullTeamName

 **find purple cap holder name**
purple cap Holder = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
--filter : wickets in selected , exclude non-bowler dismissals
VAR SeasonWickets=
                 FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season])=SelectedSeason && ball_by_ball_data[is_wicket]=TRUE() &&
                 NOT ball_by_ball_data[wicket_kind] IN {"run out","retired hurt","obstructing the field","retired out"})

-- Summarize bowler and count wickets
VAR WicketSummary=
      ADDCOLUMNS(  SUMMARIZE(SeasonWickets,ball_by_ball_data[bowler]),"WicketCount",COUNTROWS(FILTER(SeasonWickets,ball_by_ball_data[bowler] =EARLIER(ball_by_ball_data[bowler]))
        ))
--find highest wicket count 
VAR MaxWickets = MAXX(WicketSummary,[WicketCount]) 

-- Get the bowler(s) with that wicket count 
VAR TopBowler = 
   CALCULATETABLE(VALUES(ball_by_ball_data[bowler]),FILTER(WicketSummary,[WicketCount]=MaxWickets))
--Return the name ( IF MULTIPLE , IT PICKS ONE ALPHABETICALLY)
RETURN 
  MAXX(TopBowler,ball_by_ball_data[bowler])


 **find purple cap holder image**
  Purple Cap Image = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data , RELATED(ipl_matches_data[season])= SelectedSeason)
VAR WicketSummary =   
                  SUMMARIZE(SeasonDataOnly,ball_by_ball_data[bowler],"Total Wicket",SUM(ball_by_ball_data[is_wicket1]))
VAR MaxWicket = 
                  MAXX(WicketSummary,[Total Wicket])
VAR TopScorer = 
                  CALCULATETABLE(VALUES(ball_by_ball_data[bowler]),FILTER(WicketSummary,[Total Wicket]=MaxWicket)) 
RETURN 
 LOOKUPVALUE('players-data-updated'[player_image],'players-data-updated'[player_name], MAXX(TopScorer ,ball_by_ball_data[bowler]) )

 **find purple cap team name**
 Purple Cap TeamName = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonDataOnly =
                    FILTER(ball_by_ball_data , RELATED(ipl_matches_data[season])= SelectedSeason)
VAR WicketSummary =   
                  SUMMARIZE(SeasonDataOnly,ball_by_ball_data[bowler],"Total Wicket",SUM(ball_by_ball_data[is_wicket1]))
VAR MaxWicket = 
                  MAXX(WicketSummary,[Total Wicket])
VAR TopScorer = 
                  CALCULATETABLE(VALUES(ball_by_ball_data[bowler]),FILTER(WicketSummary,[Total Wicket]=MaxWicket))

VAR FullTeamName = 
                  CALCULATE(MAX(ball_by_ball_data[team_bowling]),FILTER(SeasonDataOnly,ball_by_ball_data[bowler] = MAXX(TopScorer , ball_by_ball_data[bowler])))
RETURN 
 FullTeamName

 **find purple cap total wickets**
 purple cap Wickets = 
VAR SelectedSeason = 
                      SELECTEDVALUE(ipl_matches_data[season])
--filter : wickets in selected , exclude non-bowler dismissals
VAR SeasonWickets=
                 FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season])=SelectedSeason && ball_by_ball_data[is_wicket]=TRUE() &&
                 NOT ball_by_ball_data[wicket_kind] IN {"run out","retired hurt","obstructing the field","retired out"})

-- Summarize bowler and count wickets
VAR WicketSummary=
      ADDCOLUMNS(  SUMMARIZE(SeasonWickets,ball_by_ball_data[bowler]),"WicketCount",COUNTROWS(FILTER(SeasonWickets,ball_by_ball_data[bowler] =EARLIER(ball_by_ball_data[bowler]))
        ))
--find highest wicket count 
VAR MaxWickets = MAXX(WicketSummary,[WicketCount]) 


--Return the name ( IF MULTIPLE , IT PICKS ONE ALPHABETICALLY)
RETURN 
  MaxWickets

 **find total four in each player in each season**
   **find four**
Top Fours Count = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonFours = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs]=4)
VAR FourSummary = SUMMARIZE(SeasonFours,ball_by_ball_data[batter],"FoursCount",COUNTROWS(FILTER(SeasonFours,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxFours = MAXX(FourSummary,[FoursCount])


RETURN 
    MaxFours

 **find four player image**
Top Fours Player Image = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonFours = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs]=4)
VAR FourSummary = SUMMARIZE(SeasonFours,ball_by_ball_data[batter],"FoursCount",COUNTROWS(FILTER(SeasonFours,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxFours = MAXX(FourSummary,[FoursCount])

Var TopFoursPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(FourSummary,[FoursCount] =MaxFours))
RETURN 
 LOOKUPVALUE('players-data-updated'[player_image],'players-data-updated'[player_name],MAXX(TopFoursPlayer,ball_by_ball_data[batter]))

**find four player name**
 Top Fours Player Name = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonFours = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs]=4)
VAR FourSummary = SUMMARIZE(SeasonFours,ball_by_ball_data[batter],"FoursCount",COUNTROWS(FILTER(SeasonFours,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxFours = MAXX(FourSummary,[FoursCount])

Var TopFoursPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(FourSummary,[FoursCount] =MaxFours))
RETURN 
MAXX(TopFoursPlayer,ball_by_ball_data[batter])


**find four player team name**
Top Fours Player Team Name = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonFours = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs]=4)
VAR FourSummary = SUMMARIZE(SeasonFours,ball_by_ball_data[batter],"FoursCount",COUNTROWS(FILTER(SeasonFours,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxFours = MAXX(FourSummary,[FoursCount])

Var TopFoursPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(FourSummary,[FoursCount] =MaxFours))
VAR BatterTeam = CALCULATE(MAX(ball_by_ball_data[team_batting]),FILTER(SeasonFours,ball_by_ball_data[batter] = MAXX(TopFoursPlayer,ball_by_ball_data[batter])))
RETURN BatterTeam


**find total sixs in each player in each season**
**find total sixs**
Top Sixs Count = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonSixs = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] =6)
VAR SixSummary = SUMMARIZE(SeasonSixs,ball_by_ball_data[batter],"SixsCount",COUNTROWS(FILTER(SeasonSixs,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxSix = MAXX(SixSummary,[SixsCount])

Var TopSixsPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(SixSummary,[SixsCount] =MaxSix))
RETURN 
       MaxSix

**find player image**
Top Sixs Player Image = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonSixs = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs]=6)
VAR SixSummary = SUMMARIZE(SeasonSixs,ball_by_ball_data[batter],"SixsCount",COUNTROWS(FILTER(SeasonSixs,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxSixs = MAXX(SixSummary,[SixsCount])

Var TopSixsPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(SixSummary,[SixsCount] =MaxSixs))
RETURN 
 LOOKUPVALUE('players-data-updated'[player_image],'players-data-updated'[player_name],MAXX(TopSixsPlayer,ball_by_ball_data[batter]))

**find player name**
 Top Sixs Player Name = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonSixs = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] =6)
VAR SixSummary = SUMMARIZE(SeasonSixs,ball_by_ball_data[batter],"SixsCount",COUNTROWS(FILTER(SeasonSixs,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxSix = MAXX(SixSummary,[SixsCount])

Var TopSixsPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(SixSummary,[SixsCount] =MaxSix))
RETURN 
MAXX(TopSixsPlayer,ball_by_ball_data[batter])

**find player team name**
Top Sixs Player Team Name = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])
VAR SeasonSixs = FILTER(ball_by_ball_data,RELATED(ipl_matches_data[season]) = SelectedSeason && 
                        ball_by_ball_data[batter_runs] =6)
VAR SixSummary = SUMMARIZE(SeasonSixs,ball_by_ball_data[batter],"SixsCount",COUNTROWS(FILTER(SeasonSixs,ball_by_ball_data[batter] = EARLIER(ball_by_ball_data[batter]))))
VAR MaxSix = MAXX(SixSummary,[SixsCount])

Var TopSixsPlayer = CALCULATETABLE(VALUES(ball_by_ball_data[batter]),FILTER(SixSummary,[SixsCount] =MaxSix))

VAR BatterTeam = CALCULATE(MAX(ball_by_ball_data[team_batting]),
                      FILTER(SeasonSixs,ball_by_ball_data[batter] = MAXX(TopSixsPlayer,ball_by_ball_data[batter])))

RETURN 
    BatterTeam

 
 **cearte point table in season wise**
  **find total point each team**  

Total Point = 
 VAR Win = [Won]

 VAR NR = [NR]

 RETURN (Win*2)+NR

 Total Teams = CALCULATE(DISTINCTCOUNT(ipl_matches_data[team1]))


**find total played matched each team**
 Ptd = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team1],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20")

VAR Team2Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team2],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20")

RETURN Team1Matches + Team2Matches

**find no result matched**
NR = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team1],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20",ipl_matches_data[result]="no result")

VAR Team2Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team2],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20",ipl_matches_data[result]="no result")

RETURN Team1Matches + Team2Matches


**find total lost team**
Lost = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team1],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20", NOT ISBLANK(ipl_matches_data[match_winner]), ipl_matches_data[match_winner] <> ipl_matches_data[team1])

VAR Team2Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team2],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20",NOT ISBLANK(ipl_matches_data[match_winner]),ipl_matches_data[match_winner] <> ipl_matches_data[team2])

RETURN Team1Matches + Team2Matches

**find tie match each team**

   Tie = 
VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR Team1Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team1],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20",ipl_matches_data[result]="tie")

VAR Team2Matches = 
                   CALCULATE(COUNTROWS(ipl_matches_data),USERELATIONSHIP(ipl_matches_data[team2],teams_data[Team_Name]),ipl_matches_data[season] = SelectedSeason,ipl_matches_data[match_type] ="T20",ipl_matches_data[result]="tie")

RETURN Team1Matches + Team2Matches

**find total win**
Won = 

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR CurrentTeam = SELECTEDVALUE(teams_data[Team_Name])

RETURN
CALCULATE(COUNTROWS(ipl_matches_data),ipl_matches_data[season] = SelectedSeason, ipl_matches_data[match_winner] = CurrentTeam,ipl_matches_data[match_type] = "T20")

VAR SelectedSeason = SELECTEDVALUE(ipl_matches_data[season])

VAR CurrentTeam = SELECTEDVALUE(teams_data[Team_Name])

RETURN
CALCULATE(COUNTROWS(ipl_matches_data),ipl_matches_data[season] = SelectedSeason, ipl_matches_data[match_winner] = CurrentTeam,ipl_matches_data[match_type] = "T20")
