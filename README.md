# SteamSpark-Games
## Analytics Portfolio

SteamSpark is a game analytics and marketing company that helps indie developers and publishers understand trends on the Steam platform.


### Market Insights

  What is the average price of games across the entire store?

**Input**
SELECT AVG (price) AS avg_price from games
WHERE price > 0;
**Output**
"avg"
8.922689670998388

  
  Which age rating (e.g., 0, 12, 18) is most common?

```sql
-- queries/most_common_age.sql
WITH required_age_counts AS (
    SELECT 
        required_age, 
        COUNT(*) AS mode_required_age
    FROM 
        games
    WHERE required_age > 0
    GROUP BY 
        required_age
)
SELECT * 
FROM required_age_counts
WHERE mode_required_age = (
    SELECT MAX(mode_required_age) 
    FROM required_age_counts
);

"mode_required_age"
354
  
  How many games support Windows vs. Mac vs. Linux?

SELECT 
	COUNT(*) FILTER (WHERE support_windows = TRUE) AS windows_count,
	COUNT(*) FILTER (WHERE support_mac = TRUE) AS mac_count,
	COUNT(*) FILTER (WHERE support_linux = TRUE) AS linux_count
FROM games
;

"windows_count"	"mac_count"	"linux_count"
111419	19376	13686


Which languages are most commonly supported?

SELECT 
    TRIM(language, '" ') AS language, 
    COUNT(*) AS count
FROM 
    games,
    UNNEST(string_to_array(TRIM(BOTH '{}' FROM supported_languages), ',')) AS language
GROUP BY 
    language
ORDER BY 
    count DESC
LIMIT 5;

"language"	"count"
"English"	99904
"Simplified Chinese"	27241
"German"	24350
"French"	23871
"Russian"	22759


### Performance Analysis

  Which games have the highest peak concurrent users (CCU)?
  
  What’s the average and median playtime for top-rated games (based on user score or metacritic score)?
  
  Do free-to-play games get more recommendations or longer playtime?
  
  How does playtime in the last 2 weeks compare to lifetime playtime?


### Review & Sentiment
  What’s the correlation between user score and number of positive vs. negative votes?
  
  Which games have the worst user score despite high recommendations?
  
  Are there any publishers consistently producing highly-rated games?


### Content Planning
  Which genres are most common in high-achievement games (games with many achievements)?
  
  Are there categories or tags associated with more playtime or better user scores?
  
Which games have detailed descriptions but low scores — are we missing marketing opportunities?

### Release & Pricing Strategy
What is the average price of games released in the last 2 years (based on release date string)?

How do prices differ by publisher or developer?

Do higher-priced games tend to receive better user reviews?

Is there a noticeable drop in user score or reviews for older games?
