<p align="center">
  <img width="500" height="300" src="https://cycle2day.nl/CK/image/2021/12/stravalogo.webp">
</p>

# StravaDashboard

As a side project, I built this dashboard to gain insights into my Strava activity, focusing on my rides. It's also a way to practice my Python skills and explore libraries like Pandas, Seaborn, and Plotly Express.

## Project Goals

* Analyze ride distance, pace, and other metrics.
* Visualize trends and patterns in my cycling activity.
* Learn how to use Strava API.
* Explore pandas, seaborn, plotly express.
* Utilize Flake8 for linting.
* Use a virtual environment for running the project.
* Utilize Postman for API testing.

## Getting Started

1. **Prerequisites:** Python 3.x, libraries in `requirements.txt`.
2. **Clone the repository:** `git clone https://github.com/kolipoki/StravaDashboard.git`
3. **Setup your Strava app** in <https://www.strava.com/settings/api>. 
4. **Setup your virtual environment:** `python -m venv <folder_name>` activate it `source <folder_name>/Scripts/activate`
5. **Install dependencies:** `pip install -r requirements.txt`
6. **Get CLIENT_ID, CLIENT_SECRET, REFRESH_TOKEN** from <https://www.strava.com/settings/api>.
7. **Run the notebook:** Open `strava.ipynb` in a Jupyter Notebook environment.

   Change:
   
      ```python
        CLIENT_ID = "your_client_id"
        CLIENT_SECRET = "your_client_secret"
        REFRESH_TOKEN = "your_refresh_token"
      ```


     

## How to authenticate the Strava API

Test the API in Postman: <https://www.strava.com/api/v3/athlete>.

Select Authentication: Bearer Token and add your access token from <https://www.strava.com/settings/api>.

Problem: It will expire every 6 hours.

1. Obtain Authorization Code(This is a one time, manual step.):

   * Visit this link (replace `your_client_id` with your actual ID): <https://www.strava.com/oauth/authorize?client_id=your_client_id&redirect_uri=http://localhost&response_type=code&scope=activity:read_all>
   * This will prompt you to log in to Strava and authorize your app. Ignore the displayed error, as it's part of the process.
   * Copy the "code" parameter from the resulting URL.
  
2. Exchange Authorization Code for Access Token & Refresh Token:

   * Use Postman or another tool to send a POST request to: <https://www.strava.com/oauth/token>
      * Include the following parameters:
          * `client_id`: Your Strava client ID.
          * `client_secret`: Your Strava client secret.
          * `code`: The authorization code you obtained in step 1.
          * `grant_type`: Set to authorization_code.
    * This will retrieve an access token with limited lifespan and a refresh token for obtaining new access tokens later.
      <https://www.strava.com/oauth/token?client_id=your_client_id&client_secret=your_client_secret&code=your_code_from_previous_step&grant_type=authorization_code>
    * With the new POST request in Postman we will exchange our authorization code to a refresh token and an access token.
    * Got a new access token and refresh token.

3. Check if it is working right:

   * Copy the newly acquired ACCESS_TOKEN and paste it in this link: <https://www.strava.com/api/v3/athlete/activities?access_token=access_token_from_previous_step>
  
4. Use Refresh Token to get new Access Tokens:

   * Use refresh token to get new access tokens
   * Use Postman or another tool to send a POST request to: <https://www.strava.com/oauth/token>
      * Include the following parameters:
          * `client_id`: Your Strava client ID.
          * `client_secret`: Your Strava client secret.
          * `refresh_token`: The refresh token you obtained in step 2.
          * `grant_type`: Set to refresh_token.
      * <https://www.strava.com/oauth/token?client_id=your_client_id&client_secret=your_client_secret&refresh_token=your_refresh_token_from_previous_step&grant_type=refresh_token>
   * This will provide a new access token for continued API calls.
   * So the newly generated refresh token won’t be expire, but the access_token will. So in our app we have to run this request to use our not expiring refresh token to get the access_token.
   * These two request should be in our code.
     * <https://www.strava.com/api/v3/athlete/activities?access_token=access_token_from_previous_step>
     * <https://www.strava.com/oauth/token?client_id=your_client_id&client_secret=your_client_secret&refresh_token=your_refresh_token_from_previous_step&grant_type=refresh_token>

## Analyzing the data

  #### Total Distance by Year (km/year)

   I would like to know how much I ride.

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/total_distance.png)


  #### Total distance by Month (km/month)

   Which month is the most active?

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/total_distance_month.png)

  #### Total distance (km) by Month

   Showing the same information in a line chart to be able to compare my active years.
  
   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/total_distance_month_2.png)

  #### Total distance (km) by Weekday

   Checking the most active day of the week.

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/total_distance_month_3.png)

  #### Distance distribution (km)

   Check the average lenght of a ride.

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/distance_distribution.png)

  #### Distance distribution (km) by Year

   Showing the main statistical values for each active year.

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/distance_distribution_2.png)

  #### Average elevation gain (m) distribution

   Checking the average elevation gain per ride.

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/elevation.png)

  #### Distibution of average Heart Rate

   What is the most common activity intensity?

   I would like to spend the most training time in Zone2.

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/hr.png)

  #### Creating a Grid with Seaborn

   Create a pair plot to visualize the relationships between performance metrics, using the specified performance columns.

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/grid.png)

  #### Is there a relationship between how far I ride and my average speed?

   ![alt text](https://github.com/kolipoki/StravaDashboard/blob/main/Sample%20Pictures/speed.png)

  #### Top 10 activity by distance

  <div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>name</th>
      <th>distance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3080</th>
      <td>2022-06-19</td>
      <td>Balaton kör</td>
      <td>204.42</td>
    </tr>
    <tr>
      <th>3425</th>
      <td>2023-05-21</td>
      <td>Pilis vissza</td>
      <td>138.90</td>
    </tr>
    <tr>
      <th>3459</th>
      <td>2023-06-24</td>
      <td>Wolfsberg round 2 HC climb</td>
      <td>134.12</td>
    </tr>
    <tr>
      <th>3438</th>
      <td>2023-06-03</td>
      <td>Buba</td>
      <td>119.75</td>
    </tr>
    <tr>
      <th>3537</th>
      <td>2023-09-08</td>
      <td>Morning Ride</td>
      <td>114.58</td>
    </tr>
    <tr>
      <th>3137</th>
      <td>2022-08-14</td>
      <td>Kékes (Tour de Hongrie)</td>
      <td>104.61</td>
    </tr>
    <tr>
      <th>3591</th>
      <td>2023-11-01</td>
      <td>Pilis classic</td>
      <td>101.47</td>
    </tr>
    <tr>
      <th>2823</th>
      <td>2021-10-05</td>
      <td>Dobogókő</td>
      <td>96.39</td>
    </tr>
    <tr>
      <th>2769</th>
      <td>2021-08-13</td>
      <td>Balaton Nyugat (Linda+Hanni)</td>
      <td>83.56</td>
    </tr>
    <tr>
      <th>2768</th>
      <td>2021-08-13</td>
      <td>Morning Ride</td>
      <td>82.32</td>
    </tr>
  </tbody>
</table>
</div>

  #### Top 10 activity by elevation gain

  <div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>name</th>
      <th>elev_gain</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3459</th>
      <td>2023-06-24</td>
      <td>Wolfsberg round 2 HC climb</td>
      <td>2901.1</td>
    </tr>
    <tr>
      <th>3591</th>
      <td>2023-11-01</td>
      <td>Pilis classic</td>
      <td>1847.9</td>
    </tr>
    <tr>
      <th>3537</th>
      <td>2023-09-08</td>
      <td>Morning Ride</td>
      <td>1723.3</td>
    </tr>
    <tr>
      <th>3154</th>
      <td>2022-08-30</td>
      <td>Sella Ronda</td>
      <td>1720.3</td>
    </tr>
    <tr>
      <th>3425</th>
      <td>2023-05-21</td>
      <td>Pilis vissza</td>
      <td>1557.8</td>
    </tr>
    <tr>
      <th>3137</th>
      <td>2022-08-14</td>
      <td>Kékes (Tour de Hongrie)</td>
      <td>1410.4</td>
    </tr>
    <tr>
      <th>3157</th>
      <td>2022-09-01</td>
      <td>Sella Ronda with Canazei</td>
      <td>1300.9</td>
    </tr>
    <tr>
      <th>3570</th>
      <td>2023-10-11</td>
      <td>Afternoon Ride</td>
      <td>1213.4</td>
    </tr>
    <tr>
      <th>1748</th>
      <td>2018-10-28</td>
      <td>Tour de Buda</td>
      <td>1208.7</td>
    </tr>
    <tr>
      <th>3640</th>
      <td>2023-12-18</td>
      <td>Zwift - Steeper &amp; Steeper on Tour of Fire and ...</td>
      <td>1171.0</td>
    </tr>
  </tbody>
</table>
</div>
