import requests
import urllib3
import time
import datetime as dt
import math

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)   # disable_warnings

auth_url = 'https://www.strava.com/oauth/token'
activities_url = 'https://www.strava.com/api/v3/athlete/activities'

# -----------------------------------------------------------------------------
# lets get a actual access token, refresh token is known
params = {
    'client_id': 'XXXX',
    'client_secret': 'XXXX',
    'refresh_token': 'XXXX',
    'grant_type': 'refresh_token',
    'f': 'json'
}
print('Requesting Token...\n')
response = requests.post(auth_url, data=params, verify=False)
access_token = response.json()['access_token']
print('Access Token = {}\n'.format(access_token))

# -----------------------------------------------------------------------------
# lets get json data of activities to my_dataset
header = {'Authorization': 'Bearer ' + access_token}
param = {'per_page': 100, 'page': 1}
my_dataset = requests.get(activities_url, headers=header, params=param).json()
# print(my_dataset)

# -----------------------------------------------------------------------------
# lets find the last week running distance
week_distance = 0
count_week_activities = 0
for i in range(20):
    # 1) transform last 20 activities 'start_date_local'(str) >> class 'datetime.datetime'
    (my_dataset[i]['start_date_local']) = dt.datetime.strptime(my_dataset[i]['start_date_local'], '%Y-%m-%dT%H:%M:%SZ')
    # 2) activities with ['type'] == 'Run' in current week
    if int(time.strftime('%W', time.localtime())) == my_dataset[i]['start_date_local'].isocalendar()[1] and my_dataset[i]['type'] == 'Run':
        week_distance += my_dataset[i]['distance']
        count_week_activities += 1
week_distance = round((week_distance/1000), 2)

# -----------------------------------------------------------------------------
# output
print(week_distance, 'km in this week\nyou have ran ', count_week_activities, ' time(s) this week')
if math.ceil(25-week_distance) > 0:
    print(math.ceil(25-week_distance), 'km left')
else:
    print('you have completed the weekly goal')
