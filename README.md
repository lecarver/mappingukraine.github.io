#Mapping Conflict and Social Media in the Russia-Ukraine Conflict
Lester Carver & Yao-Ting Yao
December 19, 2022

#Introduction   

Since the onset of Web 2.0, social media has seen a rapid expansion across the globe. In a 2022 survey of social media users, it was found that 58.4% of the population are social media users, which makes up 93.4% of all internet users (Chaffrey, 2022). Furthermore, users spend on average 2.5 hours on social platforms daily (Chaffrey, 2022). Social media has also demonstrated itself to be a powerful platform for shaping narratives around current events as well as disseminating and combatting misinformation—it is a tool used by politicians, global institutions, civil society, and insurgency groups alongside everyday citizens (Wooley, 2022). In particular, social media plays a key role as a communication channel, intelligence source, and propaganda platform during times of conflict; a role which has been highlighted during the ongoing Russo-Ukrainian War. Across several studies and new coverage, there are many examples of the important role social media has played throughout the conflict—spreading pro-Russia propaganda portraying Ukrainian leaders as fascists (Boucher, 2022; Paul, 2022), having content mined by Russian intelligence to inform missile strikes (Boyte, 2017), and uplifting national resistance efforts (Specia, 2022; Bernotand and Childs, 2022).

While understanding the role social media can play in conflict is a critical topic in its own right, this project explores and visualizes the relationship between conflict and social media activity by those directly impacted by conflict and demonstrates tools for scraping Twitter data outside the Twitter API environment​. The goal is to explore if there is a relationship between instances of conflict and when, where, and at what magnitude social media posting about the conflict occurs within the conflict zone of Ukraine. By mapping this phenomenon, we can bring insight into the behaviors of social media users during the conflict, specifically those users directly impacted by conflict. Understanding how populations living in conflict zones interact with social media can inform how to communicate important information to affected populations, such as humanitarian and relief aid or evacuation and other safety notifications, as well as utilize social media platforms to bridge between government and people, as we have seen in the active use of social media by Ukraine’s President Zelinsky throughout the conflict (Khan, 2022).  

#Data

This project utilizes two primary sources of data. The first is a dataset of political violence, demonstration events, and strategic developments of the conflict in Ukraine from the Armed Conflict Location & Event Data Project (ACLED). This dataset goes back to 2018 and contains spatial data disaggregated by different types of conflict, such as grenades, abductions, territory gains/losses, air/drone strikes, etc. The dataset is preprocessed to contain only data from February 2022 to October 2022, which marks Russia’s invasion into Ukraine and the most significant escalation in the Russo-Ukrainian War since its beginnings in 2014. It will also be filtered to focus only on armed clashes, so filtering out individual acts of violence (e.g. sexual violence and abductions) and general category of  “strategic developments” and territory changes.  

The second dataset is data scraped from Twitter. The method for data collection is Python-based using the snscrape library, which bypasses the Twitter API and some of the API’s associated limitations. Snscrape is a scraper for social networking services (SNS) designed for archival work so it focuses on scraping historical social media data. It scrapes information including user profiles, hashtags, or searches and returns the discovered items, e.g. the relevant posts. Tweets were filtered using hashtags about the Russian-Ukrainian war focusing on anti-war and pro-Ukraine hashtags. Tweets were collected and aggregated by geolocation on a monthly basis from February 2022 to October 2022.

Finally, subnational administrative boundaries and population data of Ukraine were be pulled from USAID’s Demographic and Health Survey Spatial Repository. Administrative boundaries were used as the spatial unit of aggregation for tweet activity.  

#Methodology

To collect the tweet data, the set of relevant hashtags was collected through a general search of social media postings about the conflict. A list of 21 relevant hastags was created, including hashtags in both English and Ukrainian. The tweets were filtered by location by extracting the coordinates within Ukraine boundary through the centroid of each Ukraine administration boundary with a 150-kilometer radius, which ensured that the search covered the entire country.  

Then a python script using the snscrape library was written, using a series of for loops to search in 1-month intervals for each hashtag and each administrative area. The first loop is to build up the search queries for date, hashtag and location, which we appended to an empty array. The second loop input each search query into the snscrape Twitter search function and pull the Twitter ID and date for each tweet in the search query.  The results were then converted to a pandas dataframe and exported to a csv file. The data was exported to csv for further data cleaning, like aggregating tweets by month and creating a new column of integer month in order to create a time slider bar.  Lastly, the csv file was joined to the administrative boundaries shapefile to create a geojson layer file.

The ACLED conflict data were filtered to just preserve armed clash and missile strike data from February 2022 to October 2022. The conflict and tweet data were transformed from csv files to point and polygon layers respectively to be displayed in the web map. Once the datasets were cleaned up and ready for use, they were uploaded to Mapbox to be hosted as tilesets.  

The webmap was written in HTML, CSS and JavaScript using the Mapbox GL JS API to design an interactive web map, including a time slider, mouse hover and temporal chart, and is hosted on a GitHub repository. For the time slider function, which was a component added to the map that was outside of the course material, and event listener was used that selected feature layers based on their month attribute. Since the time period for this project was from February to October, an array of those months, plus 2 empty items in the 0th and 1st position was used and then a query matching the month to the index position in the array was used to filter the tileset features for both the conflict events and tweet datasets.  

In mapping the tweet data, the quantile classification method was used to create choropleth map of tweets. However, the number of tweets between administrations had extreme amount of difference. Half of administrations have less than 100 tweets in total, which also causes the challenge of mapping spatial trend in hot spot analysis. Finally, the temporal chart was created using the C3 library, which is a based in D3, to show the total number of tweets across Ukraine for each month. Given that this was a fairly small dataset, the data was entered directly into the code.

Users can interact with the final web map in several ways. They can use the time slider to see where conflict events have occurred since February 2022 as well as see where twitter activity has been the highest across Ukraine. They can also take a closer look at the data within each administrative area. When the user hovers their mouse over a boundary, the region name and tweet count is shown on the web map console and the administrative area is highlighted. Finally, the user can open the temporal line chart to see total tweet activity over time and can hover over the chart to see the total tweet count for each month. This interactive web map allows users to explore how the Russo-Ukrainian Conflict influences tweeting activity by Ukrainians temporally and the degree to which responses differed by geographic area within Ukraine.

#Results & Discussion

Overall, the final product is successful in creating a web map where users can explore social media activity and conflict events in the Russian-Ukrainian conflict.  Those interested in how social media plays a key role during times of conflict will benefit through these three main functions in this interactive map:

- Click the “Total Tweets by Month” button to explore the chart of total tweets numbers between February and October 2022

- Move the time slider to display conflict point and quantile choropleth tweet map changes along different months.

- Hover the cursor to explore the number of tweets of the administrative area popup in the console.

Based on the data collected, there was no relationship between conflicts and tweet activity. Central Ukraine, namely Cherkasy, Kirovohrad, Mykolayiv had the highest number of tweets while most conflict events occurred in the east. Cherkasy as an important economic center of Ukraine and Mykolayiv as a transportation hub for Ukraine, but it is unclear why those administrative areas returned the highest number of tweets compared to other areas.  

Related to the project’s second objective, it was successful in showing a method for scraping Twitter data without creating a Twitter API key, however there were several challenges and limitations in this aspect of the project. In the beginning, several methods were attempted for scraping Twitter data. Using the Twitter API comes with several limitations, the most significant being that it only returns the most recent 3200 tweets. Next, the twint package was used, which does not require a Twitter API key and seemed to be a popular option for other users. While the script using twint ran successfully, the resulting dataset changed every time the script was run and it was later discovered that the twint package has been archived and is not compatible with version 2 of the Twitter API. In the final version of the scraping script, 30,895 tweets were successfully scraped in total. However, since 2019 Twitter stopped geotagging all tweets, meaning it will be challenge to do spatial-based Twitter scraping, which is reflected in the highly mixed amount of tweets that were collected for each administrative area in this project.

#Future Work

In the future research and web mapping, we would like to look at pre-2019 conflicts to compare the limitations of scraping Twitter for geodata post-2019. For the data pre-processing, like aggregating tweets numbers by each month and joining to administrative boundary, we would like to create an automation process in python script instead of manually processing. Last but not least, we would like to apply word cloud and sentiment analysis to better understand the context of each message associated. By mapping this phenomenon, we can bring insight into the behaviors of social media users during conflict, specifically those users directly impacted by conflict.   

#References  

Project github: https://github.com/lecarver/mappingukraine.github.io

Boyte, K. (2017). An Analysis of the Social-Media Technology, Tactics, and Narratives Used to Control Perception in the Propaganda War Over Ukraine. Journal of Information Warfare, 16(1), 88–111. https://www.jstor.org/stable/26502878

Loyle, C. E., & Bestvater, S. E. (2019). #rebel: Rebel communication strategies in the age of social media. Conflict Management and Peace Science, 36(6), 570–590. https://doi.org/10.1177/0738894219881430

Boucher, Jean-Christophe. (2022). Disinformation and Russia-Ukrainian War on Canadian Social Media. The School of Public Policy Publications, 15(1). https://doi.org/10.11575/sppp.v15i1.75449

Kyrgos, Z. (2022). Strategic Social Media Management in Conflict Zones through the Analysis of the Intelligence Cycle: Lessons Learned from the Russo-Ukrainian Conflict. HAPSc Policy Briefs Series, 3(1), 114-120. https://doi.org/10.12681/hapscpbs.31000

Navya Sonal Agarwal, Narinder Singh Punn, and Sanjay Kumar Sonbhadra. 2022. Exploring Public Opinion Dynamics on the Verge of World War III using Russia-Ukraine war - Tweets Dataset. In KDD-UC, 2022. ACM, New York, NY, USA, 4 pages. https://www.kdd.org/kdd2022/papers/27_Navya%20Sonal%20Agarwal.pdf

snscrape package in github, https://github.com/JustAnotherArchivist/snscrape

twint package in github, https://github.com/twintproject/twint

Appendix  

Lester: Introduction, Data, Methodology, Future work, References, conflict data visualization, tweet scraping script, About Me section, time slider, mouse over event listeners.
Yao-Ting: Data, Methodology, Results, Future work, References, tweet scraping script, tweets choropleth map, popup form, Explore the Map section.
