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

 

  #### Total distance by Month (km/month)

  Which month is the most active?

  
