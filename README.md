FitBit Surge Website
http://web.csulb.edu/~jcover/448/

John Lloyd Cover, Allan Dominguez, Peyton Leon Drouhard, Erwin Dale Gumabao Guillermo, and Ryan Michael Fritz,

Computer Engineering &amp; ComputerScience, User Interface Design Class

California State University, Long Beach 250 Bellflower Boulevard, Long Beach, California 90840

# 1 Abstract
The purpose of this report is to describe the process of creating an interactive interface for the fitness super watch, FitBit Surge. The interactive interface which was developed and presented within this report is a web based platform. Special attention was given to the usability of the interface as it relates to user interaction and general usability heuristics. A description of the FitBit is given. Data acquisition and implementation in the design is described along with a proposed plan to evaluate overall system usability.



**Keywords** : Fitbit, Surge, Website, Webpage, Health Tracker, Watch, Smartwatch

1. 1Introduction

## 1.1 Background

FitBit started selling fitness tracking devices in 2009. Since then the company has sold millions of tracking devices and successfully increased revenues each year. Currently eight different models are sold; every model is intended to help users track their health, but each watch has specific features to attract all types of customers. For example, the Blaze is a large, full color, LCD touch screen watch face; the device is sold with a standard wristband, but FitBit sells a myriad of bands giving users the ability to customize their look at will. Bands come in multiple shapes, sizes, colors, and even materials! Furthermore, the Blaze can sync with mobile devices and allows the user to interact with the synced device via their watch. Users who want a simple device should consider the Zip, a clip-on device which tracks fitness metrics, but doesn&#39;t sync with devices and has limited features. All eight models have pros and cons, but FitBit has made a point to be as versatile as possible.

## 1.2 The FitBit Surge

This project focused on the FitBit Surge watch which tracks many different aspects of its wearer: heart rate, steps, distance travelled, calories burned, floors climbed, active minutes, hourly activity, and stationary time. Many of these are also split into subcategories or used in conjunction for more descriptive information. For example, heart rate is broken down into ranges labeled to indicate how hard the wearer is working while stationary time and heart rate are used together track different levels of sleep. The FitBit Surge also features multiple settings designed to organize information based on the activity the user does. For example, if the user does yoga, the Surge can be set to a &quot;Yoga&quot; setting. The user can use this feature to see how their health metrics vary during different types of exercise and more comprehensively track their fitness.

In addition to tracking the wearer across multiple dimensions, the FitBit Surge can be paired with mobile devices to alert the wearer of external notifications (e.g., text messages). Personal notifications and reminders can be programmed into the watch in addition to the preprogrammed metric relevant alerts (e.g., the watch vibrates when a goal has been met).

Our goal was to create an interactive website that displays health metrics to allow users to see, better understand, and analyze their health information. A website platform was chosen to fully utilize the skills of our team.

1. 2Data

## 2.1 Data acquisition

Generating data was one of the major obstacles on the development side of this project.  FitBit didn&#39;t make our data as available as we had initially anticipated.  We struggled quite a bit with implementing the OAuth 2.0 identity and policy architecture FitBit utilizes for their authentication.  We needed an implementation that was designed for a client-side web application so we attempted to develop one using a JavaScript runtime built on Chrome&#39;s V8 JavaScript engine: Node.js.

After several builds of our Node.js OAuth client failed, we had to – in the interest of time - scrap the project and take advantage of an existing community development tool that allowed us to pull the more detailed data that we were looking to access from their API – intraday data they called it.  With this tool we were able to pull intraday data over three categories: Activity, Heart Rate, and Sleep.

## 2.2 Data Format

The Activity category was the most robust including minute by minute data of the User&#39;s steps taken, floors traversed, distance traveled, and calories burned. The Sleep category provides  data about when the user went to sleep, and  minute by minute data of their &quot;sleep state&quot;, which is classified as either Really Awake, Awake, or Asleep.  It also includes a Sleep Log ID which is repeated for each row in the data.  Lastly,  the Heart Rate category  provides a BPM value every 5 seconds for 24-hours of the selected  day

The API only allowed a scope of 24 hours when pulling detailed intraday data so we decided to focus on one week&#39;s worth of data over the 3 categories. Each query returned a large JSON array (about 65 kb of data), depending on the amount of activity recorded for the requested day.

The JSON array was delimited in such a way that it could be stored in a csv file and interpreted by spreadsheet programs. Despite this, our first instinct was to store all of our data in an SQL database so we could manipulate it with the appropriate SQL queries; for example we knew we wanted to relate sleep to heart rate in some of our visualizations and a SQL inner join would have been an effective way to generate that data that was currently stored  in separate tables. However, after becoming more familiar with the Google visualization API we found that it had a high level of compatibility with its own Google Spreadsheets service.  Because of this,  we decided to go  with the CSV format that was originally returned from FitBit and store it directly on our Google Drive. From there, all we needed to do was publish the csv files so the website client could access them remotely.

## 2.3 Data Selection and Cleaning

Once we had all of our data accessible to the google visualization API used in our web client the next challenge was formatting or cleaning it in a way that would allow us to get the type of visualizations we were shooting for. One common method we used was to have the &quot;raw&quot; data in one spreadsheet, and then create a second derivative spreadsheet that would perform operations on that data using specific formulas.

For example, our sleep cycle graph needed to know the percentage of time a user&#39;s heart rate was in a certain BPM range. We did this with a formula that operated on the cells of the &quot;raw data&quot; file, and populated its own cells with the result. We were then able to construct a google visualization using javascript to pull just that specially formatted csv file.

## 2.4 Data Exclusions

Some data entries that were in the &quot;raw&quot; data were not used in our visualizations. One such category of information was a whole table or column of data that was reported in the original JSON array labeled &quot;Sleep Log ID&quot;. It contained a string of numbers, specifically 12938363977, repeated for each minute of sleep recorded. This data was redundant, and esoteric; we didn&#39;t understand what it represented and as such did not include it in our visualizations.

1. 3Design

## 3.1 Identifying Users

Our first task for designing was to identify user groups of FitBit watches. According to Nielsen.com, the average user is between 25 and 34 years old and lives with a household income of $50,000 to $99,999 per year. Furthermore, 3.3 million people are using the FitBit mobile app to view their health data. Additionally, more women use mobile apps than men despite the use ownership of health tracking watches being equal for each sex.

Though millions of FitBits have been sold, the abandonment of these fitness trackers is relatively high with roughly a third of owners ditching their watches after only six months of usage. Because of this issue, we chose three specific groups who regularly check their health metrics because of their lifestyles and therefore would be less likely to stop using their activity tracker. With this in mind, we chose to design our interface for people with Type 1 diabetes, people with heart disease, and athletes training for competition. Identifying specific groups also allowed out team to narrow the focus of our design. This was especially important due to the enormity of information collected by the Surge and the many different ways to display the collected data. We then identified several health metrics that these groups could monitor using the FitBit watch: caloric intake, body weight, steps taken, distance travelled, and floors climbed.

## 3.2 Website

With our specific metrics chosen, we next identified the general layout and functional mapping we wanted the website to have. Inspired by the FitBit website&#39;s user homepage, we chose to present graphs on the homepage with the graphs acting as links to additional pages disclosing greater detail and descriptions of the data.

To aid navigation between pages, and follow design heuristics outlined by Jakob Nielsen, we placed links to every page on every page. The masthead is identical on every page and navigates back to the homepage when clicked. Furthermore links are consistently marked by the same image and located at the bottom on each graph page. Another navigational menu was added to the top of each graph page. This redundancy will allow users to quickly move around the website. To ensure users know which page is currently active, both menus highlight the active page&#39;s relative link.

We chose a very colorful background for each page that is associated with the information displayed (e.g., the page regarding diet has a picture of food in the background). A transparent, black box outlines the information so users can quickly and easily distinguish the graphs and menus from the background. This black box utilizes the Gestalt principle of common region while also increasing the contrast ratio between the background and the information we want users to attend to; transparency allows users to fully view the beautiful pictures in the background.

## 3.3 Graphs

Four different types of graphs were used to display health data. (1) Donut graphs present information as a percentage of a whole; these graphs are perfect for displaying goal-based data like number of steps taken. (2) A bar graph presents the percentage of sleep for a given night that consisted of each stage. Optimally, this would be a stacked bar graph presenting the total number of hours slept and the amount of time spent in each stage for that given night. (3) Line graphs present heart rates over time for each given day and the user&#39;s body weight. (4) A scatter plot presents the number of calories burned. This type of graph was also used to present steps taken over time. Additionally, we used a radial gauge to represent how many calories a person has taken in compared to how many they have set as a goal. These graphics were chosen because we believe they best represent the metrics we selected. Additionally, the graphs are attractive, clear, and provide valuable &quot;at a glance&quot; information.

1. 4Implementation

We chose to go with a web client for our implementation. We felt that it was a good platform for the direction we wanted to take our software which emphasizes data visualization.  It was also the platform that, as a group, we felt the most confident developing in – given our timeframe and assignment criteria.

## 4.1 Frameworks

Our early stages of development were spent researching possible frameworks to use in the manipulation and visualization of our &quot;raw&quot; FitBit data.  Some light research into the FitBit API gave us an idea of what the data was going to look like (a JSON array), and pushed us towards a framework that would be compatible with a web client.

## Data-Driven Documents

Given this criteria, we first considered a JavaScript library for manipulating documents based on data called D3, or Data-Driven Documents. First impressions of D3 were positive, it was described as a framework that helps you bring data to life using HTML, SVG, and CSS: perfect for our web-based approach.  However,  actual attempts to build a D3 application proved exceedingly difficult, and required a good bit of &quot;hacking&quot; to get the type of functionality we were looking for. D3 was designed, apparently, not as a monolithic framework that seeks to provide every conceivable feature, but instead focuses on minimal overhead with support of large datasets.

## Vega-Light

After deciding that D3 wasn&#39;t the best fit for the scope of our project we moved on to Vega, a visualization grammar built on top of the D3 framework. Vega was a much higher level visualization grammar that provided a concise JSON syntax that supported rapid generation of visualizations: much more conducive to our timeframe.

With Vega you can describe the visual appearance and interactive behavior of a visualization in a JSON format, and generate views using HTML5 Canvas or SVG. Vega was also able to automatically produce visualization components including axis, legends, and scales. It then determines properties of these components based on a set of carefully designed rules. This approach was especially appealing because it allowed Vega specifications to be succinct and expressive, but also provide user control. Which was largely what we were aiming for.

However, despite all these promising features, we again struggled to use the framework in our implementation. It frankly was a little bit over our heads.  We were able to adapt portions of our data to Vega specifications, however it was overly time consuming and often didn&#39;t produce our desired visualization or level of interactivity.

## Google Visualization API

At this point we were a bit more versed in data visualization, and had a better idea of our capabilities and timeframes when we selected our 3

# rd
 and final visualization framework: the Google Visualization API.  This framework is a JavaScript API that is freely available for the public to use.  It is used internally by google as well for services such as Google Docs, AdWords, AdSense, and YouTube Analytics, etc.

Google Visualization API was actually very similar to what we worked with in Vega.  It shared its&#39; high-level approach of automatically producing visualization components as well as the ability to adapt to different data sets based on a system of carefully designed rules. However, the distinguishing factor was certainly the amount of community support and direct developer support from Google that we were able to get.  This was the primary factor that helped us overcome some of the major challenges in adapting our data to an interactive visualization; a process that had previously stumped us with our first two frameworks.

It was also one of the few frameworks that the developers explicitly expressed an intent to provide support for mobile platforms. Things like touch screen interaction, and android integration support.  This was important to us because we knew we could easily adapt our web client to function well on mobile devices using CSS techniques.

## 4.2 Challenges and Design

As discussed in previous sections there were several challenges that we ran into during implementation. But there were also some higher level challenges that we experienced in the development process such as organizing the team, changes in scope, and the clear assignment of roles.  I like to think that our design practices in Agile development (specifically Scrum), helped mitigate or even prevent some of these challenges.

We had our product owner (designated as our human factors team) create a product backlog, which was basically a prioritized wish list. During what was called sprint planning, the development team pulled a few things from the top of the product backlog and decided how to complete it.

We gave ourselves a set timeframe to complete our set of feature requests, called sprints. These sprints helped us break down the project into manageable goals, as well as decide when a particular approach was taking too long and should be scrapped.  I think this helped overcome quite a few of the potentially disastrous road blocks we ran into through our development process. One such example was our attempt to include Oauth2.0 integration in our client so we could pull data on demand from the API.  This ended up taking much too long, and we got to the point where we had to scrap it and take a different approach in data storage and acquisition.

We also held Scrum meetings in our lab sessions where our Scrum Master would help keep the team on track and focus on the current sprint.  At the end of each sprint we conducted a sprint review and determined if our work was potentially shippable, or more specifically &quot;ready to turn in&quot;.  We would also hold a sort of retrospective examination of our sprint process to try and determine if we could do anything better for the next sprint. After the retrospective, we selected the next chunk of requirements from our backlog and  repeated the cycle.

1. 5 Evaluation

To assess the interaction design of our system we propose conducting a usability evaluation. The process of completing this evaluation will include identifying the target user population, conducting a heuristic evaluation, implementing a user testing evaluation and correcting issues identified. This will be an iterative process, excluding identifying target user population, in identifying usability issues based on heuristic violations and user comments or suggestions. Violations and issues identified will be corrected and implemented in subsequent versions of our interface which in turn will be tested.

## 5.1 Identifying Target User

In identifying the target user, electronic questionnaires will be implemented to gather basic demographic information, device preference for accessing information via the internet, and information regarding specific use of the device including goals and tasks performed. After collecting the questionnaire responses the data will be analyzed to obtain summary information which will serve as the basis of identifying our target users. Once the target population and the common tasks performed are identified we can then begin developing use case scenarios which will guide us in creating task scenarios that will be used both in the heuristic evaluation and in the user testing evaluation.

## 5.2 Conducting a Heuristic Evaluation

Using the tasks identified within the questionnaire as being the most common we will develop use case scenarios for evaluating our website against a set of ten usability heuristics for user interactive interface systems developed by Nielsen Norman Group [1]. Each member of our group, five in all, will individually go through the website twice. The first pass will focus on the flow of the website while the second pass will focus on the individual elements of the website. Each of the evaluators will identify usability violations or issues based off the heuristics list and will record the location of the violation, the description of the violation, and the possible consequence of the violation to the user.

After each member has completed the heuristic evaluation individually, findings are then compiled as a group. Redundant violations are removed and a master list is generated. Severity ratings are assigned for each violation based off the frequency of encountering the violation and the consequence of the violation to the user. The severity ratings to be used by the evaluators are based on a scale developed by Nielsen Norman Group [1] ranging from 0 (I don&#39;t agree that this is a usability violation) to 4 (usability catastrophe).

## 5.3 User Testing Evaluation

After compiling the heuristic violations, a user testing evaluation will be implemented to determine the usability of the website. Participants will be recruited based off the demographic findings from the target population questionnaire. The user testing evaluation will take place in the Center for Usability in Design and Accessibility lab located on the second floor of the psychology building at California State University Long Beach. Participants will be completing the evaluation on a standard desktop computer. Participants responses and actions will be recorded using a screen and video recorder.

### 5.3.1 Procedure

Participants will be greeted and asked to fill out a form providing demographic information. Once the demographic form is completed, informed consent will be obtained and the participant will be informed about the evaluation and the instructions for its completion. Before each task the participant will fill out an expected difficulty form and after each task the participant will complete an experienced difficulty form and will provide verbal comments regarding the task, difficulties, likes and dislikes. During task completion the participant will be using the think aloud protocol. At the completion of the user testing evaluation the participant will complete a system usability scale report and will be asked to provide general comments regarding the website and its usability. The participant will then be debriefed concerning the purpose of the evaluation and will be thanked for participating.

### 5.3.2 Data

Both qualitative and quantitative data will be obtained from the evaluation. Participants age, gender, device preferences regarding accessing electronic information, and frequency of use will be obtained. Participants comments regarding the use of the website and any issues or difficulties encountered will be obtained. The results of the system usability scale will provide a measure of the usability of our website. Several measures of efficiency will be used including, time on task, lostness scores for both pages and clicks, number of errors, task completion, and the common industry format (CIF) which is a measure of efficiency based on task completion and mean time on task.

### 5.3.3 Redesign Recommendations, Implementation and Iteration

After the results of the user testing evaluation are gathered and analyzed the findings will be combined with those from the heuristic evaluation and design recommendations will be developed to correct the identified violations and issues. Alternate designs will be developed and tested to determine which design is the best at eliminating usability problems. Once recommendations are implemented into the new design choice testing can be repeated following the same process as outlined above until desired product performance, usability and satisfaction is obtained.

## 5.4 Individuals involved

Each group member will be involved in the process of assessing the usability of our website from the development of questionnaires, conducting heuristic evaluations, running participants in the user testing evaluation, and to coming up with and implementing alternate website redesign recommendations. Aside from group members, the only other individuals involved in the assessment of our website will be the actual participants that will be running in our user testing evaluation completing tasks and providing verbal feedback.

## 5.5 Anticipated Results

This being the first version of our website we anticipate finding some usability issues, however, we do not anticipate identifying any severe usability issues. Anticipated results are as follows:

### 5.5.1 Heuristic Evaluation

We anticipate finding heuristic violations which are minor in severity. These anticipated findings would probably be more prominent in regards to the heuristic of flexibility and efficiency of use. As it stands our website is not equipped with shortcuts or hotkeys that would allow for expert users to accelerate their interaction or to customize frequently used actions. Another possible area where we anticipate encountering usability issues or violation would be concerned with the heuristic of on-line help and additional documentation. Currently in this version of our website there is no on-line help functionality to assist users with problems encountered or to answer frequently asked questions. Also, there is no additional documentation that would provide the user with explanations regarding the use and purpose of the various graphs and information presented within our website.

### 5.5.2 User Testing Evaluation

We anticipate finding minor usability issues particularly in regards to the various graphs displayed throughout the website. We do not however anticipate finding any major usability issues or problems with our website. One possible usability issue that we anticipate is in regards the sleep cycles graph. We anticipate that users will not like that the stages of sleep are explained in a separate figure located below the actual graph displaying sleep cycle information across days of the week. The current setup of our website requires the user to scroll down to read the descriptions of the sleep stages. Possible confusion is anticipated in regards to the resting heart rate graph and the caloric intake graph which is not titled.

### 5.5.3 Overall

We anticipate overall findings to indicate that our website is usable with no major usability errors or violations being present. We anticipate users to identify our website as easy to use and to navigate. We further anticipate that the user&#39;s responses to the system usability scale will indicate that our website is operating within the range of good usability.

1. 6References

1. NN/g Nielsen Norman Group, [https://www.nngroup.com/articles/ten-usability-heuristics/](https://www.nngroup.com/articles/ten-usability-heuristics/)
2. Nielsen, [http://www.nielsen.com/us/en/insights/news/2014/hacking-health-how-consumers-use-smartphones-and-wearable-tech-to-track-their-health.html](http://www.nielsen.com/us/en/insights/news/2014/hacking-health-how-consumers-use-smartphones-and-wearable-tech-to-track-their-health.html)
3. iMore, [http://www.imore.com/what-fitbit-models-are-available](http://www.imore.com/what-fitbit-models-are-available)
4. MavenSpun, [http://www.mavenspun.com/javascript/learn-programming/how-to-build-graph-using-html5-canvas-and-json.htm](http://www.mavenspun.com/javascript/learn-programming/how-to-build-graph-using-html5-canvas-and-json.htm)
5. HTML5 Canvas Tutorials, [http://www.html5canvastutorials.com/labs/html5-canvas-graphing-an-equation/](http://www.html5canvastutorials.com/labs/html5-canvas-graphing-an-equation/)
6. W3schools, [http://www.w3schools.com/html/html5\_canvas.asp](http://www.w3schools.com/html/html5_canvas.asp)
7.  Vega-Lite, [https://vega.github.io/vega-lite/tutorials/getting\_started.html](https://vega.github.io/vega-lite/tutorials/getting_started.html)
8. Vega, [https://vega.github.io/vega/](https://vega.github.io/vega/)
9. PapaParse, [http://papaparse.com/](http://papaparse.com/)
10. Google Charts, [https://developers.google.com/chart/interactive/docs/reference](https://developers.google.com/chart/interactive/docs/reference)

1. 7Appendix

## 7.1 Group Member&#39;s Contributions

This class project was a group assignment and all individuals within the group collaborated as a unit to complete the various aspects that were required. Group members met weekly to discuss project progression, challenges, design layouts, and data limitations. All members of the group participated in determining the users of our website. Every aspect regarding the design, layout, coding, and functionality of the website was discussed and decided upon as a group. Two categories of work were created and they were design and development. Allan, John and Erwin handled the coding of the various elements of our website (development) while Peyton and Ryan handled layout design and usability requirements (design). Each group member contributed to the completion of the written report and the presentation slides.

## 7.2 Challenges Encountered

During the development and design process of our website we encountered many challenges, some of which were more difficult than others in overcoming.

### 7.2.1 Development Challenges

One major challenge we had to overcome was the ability to grab data from the Fitbit watch. There were two roads to take with this challenge, the first was to download the file directly from the source which contained a .csv file but the data was aggregated and would only give summary of the daily use. If we wanted the minute by minute or in some cases 5 second interval of data we would have to use the Fitbit API. Once we had access to the raw data we were able to create much more detailed graphs. Other development challenges we faced were accessing the Google Charts API and integrating the charts with the data we had. Since the raw data contained so much information in the 5 second interval, we had to design the proper graphs to show the best possible data.

### 7.2.2 Design Challenges

During the design process one challenge that we faced was in determining the best layout of each of the pages of our website. We wanted each page to be unique but to maintain consistency throughout the various pages of our website. We also wanted all of the most important content to be located above the fold of the screen without losing visibility of the information or having to shrink the graphs. Essentially, we wanted the layout and design of the website to follow the usability guidelines as demonstrated by Nielsen&#39;s list of ten usability heuristics. We overcame these challenges by utilizing unique icon graphics to represent each of the different pages and by displaying different background images relating to the page being viewed. This allowed for each page to be unique while maintaining consistency in formatting. In regards to the desire to have the most important information located above the fold, we overcame this challenge by determining which information was most relevant and had it be displayed at the top of the page. For the content which could not fit above the fold of the screen we allowed for it to be partially visible at the lower section of the page serving as an indication that there was more content further down on the page.
