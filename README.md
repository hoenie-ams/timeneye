# timeneye


Python 3 code snippet for retrieving entries for a specific day from the Timeneye API.
It returns a dictionary with the the employee's name and a tuple (total hours, billable hours)


```
import requests

# Static data used for calls to Timeney API
employees = ['employee 1', 'employee 2', 'employee 3']  # replace with your own team members
headers = {'Bearer':'XXXXXX'}  # replace the XXXXXX with your token
entries = 'https://track.timeneye.com/api/3/entries/'


def get_data_timeney(date):
    params = {'dateFrom': date, 'dateTo': date, 'limit': 1000}
    r = requests.get(entries, params=params, headers=headers)
    d = r.json()
    return d


def convert_to_dict(d):
    # Returns a dictionary with the the employee's name and a tuple (total hours, billable hours)
    result = {}  # initialize a dictionary
    for e in employees:
        result[e] = (sum([int(x['minutes']) for x in d['entries'] if x['userName'] == e]),
                     sum([int(x['minutes']) for x in d['entries'] if
                          (x['projectName'] != 'XXXXXXX' and x['userName'] == e)]))  # replace XXXXXXX with the non-billable project name
    return result

# 
date = '2017-12-23'
d = get_data_timeney(date)
result = convert_to_dict(d)
```
