# ENIGMA SCOREBOARD  
for Tulane University Live Action Gaming  
10/12/2020 - 10/18/2020 

### Import Packages  
Here we will import the packages that we need to access Google Sheets.


```python
# Install packages using sys
import sys
!{sys.executable} -m pip install gspread oauth2client df2gspread

# Import libraries
import pandas as pd
import numpy as np
import gspread
from df2gspread import df2gspread as d2g
from oauth2client.service_account import ServiceAccountCredentials
```

    Requirement already satisfied: gspread in /opt/conda/lib/python3.8/site-packages (3.6.0)
    Requirement already satisfied: oauth2client in /opt/conda/lib/python3.8/site-packages (4.1.3)
    Requirement already satisfied: df2gspread in /opt/conda/lib/python3.8/site-packages (1.0.4)
    Requirement already satisfied: requests>=2.2.1 in /opt/conda/lib/python3.8/site-packages (from gspread) (2.24.0)
    Requirement already satisfied: google-auth>=1.12.0 in /opt/conda/lib/python3.8/site-packages (from gspread) (1.22.1)
    Requirement already satisfied: google-auth-oauthlib>=0.4.1 in /opt/conda/lib/python3.8/site-packages (from gspread) (0.4.1)
    Requirement already satisfied: pyasn1-modules>=0.0.5 in /opt/conda/lib/python3.8/site-packages (from oauth2client) (0.2.8)
    Requirement already satisfied: six>=1.6.1 in /opt/conda/lib/python3.8/site-packages (from oauth2client) (1.15.0)
    Requirement already satisfied: rsa>=3.1.4 in /opt/conda/lib/python3.8/site-packages (from oauth2client) (4.6)
    Requirement already satisfied: pyasn1>=0.1.7 in /opt/conda/lib/python3.8/site-packages (from oauth2client) (0.4.8)
    Requirement already satisfied: httplib2>=0.9.1 in /opt/conda/lib/python3.8/site-packages (from oauth2client) (0.18.1)
    Requirement already satisfied: pandas in /opt/conda/lib/python3.8/site-packages (from df2gspread) (1.1.0)
    Requirement already satisfied: google-api-python-client==1.6.7 in /opt/conda/lib/python3.8/site-packages (from df2gspread) (1.6.7)
    Requirement already satisfied: argparse>=1.3.0 in /opt/conda/lib/python3.8/site-packages (from df2gspread) (1.4.0)
    Requirement already satisfied: idna<3,>=2.5 in /opt/conda/lib/python3.8/site-packages (from requests>=2.2.1->gspread) (2.10)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/conda/lib/python3.8/site-packages (from requests>=2.2.1->gspread) (1.25.10)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/conda/lib/python3.8/site-packages (from requests>=2.2.1->gspread) (2020.6.20)
    Requirement already satisfied: chardet<4,>=3.0.2 in /opt/conda/lib/python3.8/site-packages (from requests>=2.2.1->gspread) (3.0.4)
    Requirement already satisfied: cachetools<5.0,>=2.0.0 in /opt/conda/lib/python3.8/site-packages (from google-auth>=1.12.0->gspread) (4.1.1)
    Requirement already satisfied: setuptools>=40.3.0 in /opt/conda/lib/python3.8/site-packages (from google-auth>=1.12.0->gspread) (49.6.0.post20200814)
    Requirement already satisfied: requests-oauthlib>=0.7.0 in /opt/conda/lib/python3.8/site-packages (from google-auth-oauthlib>=0.4.1->gspread) (1.3.0)
    Requirement already satisfied: numpy>=1.15.4 in /opt/conda/lib/python3.8/site-packages (from pandas->df2gspread) (1.19.1)
    Requirement already satisfied: pytz>=2017.2 in /opt/conda/lib/python3.8/site-packages (from pandas->df2gspread) (2020.1)
    Requirement already satisfied: python-dateutil>=2.7.3 in /opt/conda/lib/python3.8/site-packages (from pandas->df2gspread) (2.8.1)
    Requirement already satisfied: uritemplate<4dev,>=3.0.0 in /opt/conda/lib/python3.8/site-packages (from google-api-python-client==1.6.7->df2gspread) (3.0.1)
    Requirement already satisfied: oauthlib>=3.0.0 in /opt/conda/lib/python3.8/site-packages (from requests-oauthlib>=0.7.0->google-auth-oauthlib>=0.4.1->gspread) (3.0.1)


### Importing Sheets  
Now, we'll work some magic to import data from the google sheets. See https://towardsdatascience.com/how-to-integrate-google-sheets-and-jupyter-notebooks-c469309aacea for the tutorial that I used.  
  
First, let's get our service account's credentials:


```python
scope = ['https://spreadsheets.google.com/feeds'] 
credentials = ServiceAccountCredentials.from_json_keyfile_name('./enigma-scoreboard-tulag-281332669163.json', scope) 
gc = gspread.authorize(credentials)
```

Now, let's import the data from our Google sheet that contains player responses to puzzles!


```python
spreadsheet_key = '1HNhbYW-OlHOqR48ycrhM4NOEQtK7CR1MGrsSJgDFVrc' 
book = gc.open_by_key(spreadsheet_key) 
```


```python
# Now let's open up a the entire Scoresheet
scoresheet = gc.open_by_key(spreadsheet_key)
puzzle = scoresheet.get_worksheet(0) # Get sheet of responses from spreadsheet
puzzle_df = pd.DataFrame(puzzle.get_all_records()) # Convert to dataframe
puzzle_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Timestamp</th>
      <th>Your Answer:</th>
      <th>Your Name:</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10/2/2020 16:44:23</td>
      <td>test answer</td>
      <td>test player</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10/13/2020 14:03:20</td>
      <td>Post</td>
      <td>Benjamin Villalpando</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10/13/2020 15:38:30</td>
      <td>Post</td>
      <td>Matthew Shernicoff</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10/14/2020 14:14:03</td>
      <td>Post</td>
      <td>Sarah Smith</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10/14/2020 23:32:09</td>
      <td>Post</td>
      <td>Jared Kessler</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10/15/2020 13:39:59</td>
      <td>Post</td>
      <td>Benton Meldrum and Emma Brick-Hezeau</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10/15/2020 15:14:23</td>
      <td>Post</td>
      <td>Courtney Zaharia</td>
    </tr>
  </tbody>
</table>
</div>



### Building the Scoreboard  
Now that we have access to the g-sheet of player responses, let's put these together into a DataFrame that tallies the number of responses that each player has submitted.


```python
# Iterate over each sheet in the scoresheet and tally each player's total number of responses

players_df = pd.DataFrame(columns = ['Name', 'Total Responses'])

responses_list = [] # list of dataframe objects 
for sheet in scoresheet: # for each sheet of puzzle responses
    puzzle_df = pd.DataFrame(sheet.get_all_records()) # convert to dataframe
    players = puzzle_df['Your Name:'].unique() # players who have responded to the puzzle - use .unique to filter duplicate responses
    for player in players:
        # If player not yet in dataframe, add them 
        if player not in players_df['Name'].tolist():
            player = {'Name' : player, 'Total Responses' : 1}
            players_df = players_df.append(player, ignore_index = True)
        # If already in dataframe, update their score
        if player in players_df['Name'].tolist():
            idx = players_df[players_df['Name'] == player].index.item()
            players_df.at[idx, 'Total Responses'] += 1
            
        
players_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Total Responses</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>test player</td>
      <td>29</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Benjamin Villalpando</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Matthew Shernicoff</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarah Smith</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jared Kessler</td>
      <td>14</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Benton Meldrum and Emma Brick-Hezeau</td>
      <td>4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Courtney Zaharia</td>
      <td>10</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Liv G</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Jared Kessler</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Benton Meldrum and Emma Brick-Hezeau</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Matthew Shernicoff</td>
      <td>7</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Mads RingswaldEgan</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Lily Livaudais</td>
      <td>2</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Sarah Scism</td>
      <td>1</td>
    </tr>
    <tr>
      <th>14</th>
      <td>test answer</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Benjamin</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Reed Locke</td>
      <td>1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Grayce Mores</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Riley Juenemann</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Sarah Smith</td>
      <td>1</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Emily Krouk</td>
      <td>1</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Sydney Clayton</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### Cleaning Up the Scoreboard  
This scoreboard contains the data that we want, but you may notice that there are a few issues. First, the dummy responses used to test the Google Forms -> Sheets link need to be deleted.  

Second, some players show up more than once in the table. This is because the strings they entered into the Google Forms were not the same every time - there may be differences in spelling or whitespace. To fix this, we will need to identify duplicate players and combine their rows into one, tallying their score between the rows.


```python
# First, let's delete the test entries in the table:
players_df = players_df.drop(players_df[players_df['Name'] == 'test answer'].index.item())
players_df = players_df.drop(players_df[players_df['Name'] == 'test player'].index.item())
```

Now let's try to solve for players appearing more than once. To do this, we will use Levenshtein distance. First, let's install the associated library:


```python
!{sys.executable} -m pip install python-Levenshtein
import Levenshtein as lev
```

    Requirement already satisfied: python-Levenshtein in /opt/conda/lib/python3.8/site-packages (0.12.0)
    Requirement already satisfied: setuptools in /opt/conda/lib/python3.8/site-packages (from python-Levenshtein) (49.6.0.post20200814)


Levenshtein distance is defined as the number of characters that must be changed or dropped for one string to match another. For example, the Levenshtein distance between "Sarah" and "Sara" is one - as there is only one character's difference. 


```python
lev.distance('Benton Meldrum and Emma Brick-Hezeau', 'Benton Meldrum and Emma Brick-Hezeau')
```




    0



Let's say that 2 is an acceptable Lev distance to match players. We will need to do an n^2 operation, iterating through our df to compare player names.


```python
matched = []
for name_x in players_df['Name']:
    for name_y in players_df['Name']:
        if name_y not in matched:
            if lev.distance(name_x, name_y) <= 2:
                if lev.distance(name_x, name_y) != 0:
                    print(name_x, name_y)
                    idx_x = players_df[players_df['Name'] == name_x].index.item() #index of player x
                    idx_y = players_df[players_df['Name'] == name_y].index.item() #index of player y
                    players_df.at[idx_x, 'Total Responses'] += players_df.at[idx_y, 'Total Responses'] # add scores together in row of name_x
                    matched.append(name_x)
                    print(matched)
                    players_df = players_df.drop(players_df[players_df['Name'] == name_y].index.item()) # drop row of name_y
players_df
```

    Matthew Shernicoff Matthew Shernicoff 
    ['Matthew Shernicoff']
    Sarah Smith Sarah Smith 
    ['Matthew Shernicoff', 'Sarah Smith']
    Jared Kessler Jared Kessler 
    ['Matthew Shernicoff', 'Sarah Smith', 'Jared Kessler']
    Benton Meldrum and Emma Brick-Hezeau Benton Meldrum and Emma Brick-Hezeau 
    ['Matthew Shernicoff', 'Sarah Smith', 'Jared Kessler', 'Benton Meldrum and Emma Brick-Hezeau']





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Total Responses</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Benjamin Villalpando</td>
      <td>11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Matthew Shernicoff</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarah Smith</td>
      <td>16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jared Kessler</td>
      <td>15</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Benton Meldrum and Emma Brick-Hezeau</td>
      <td>7</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Courtney Zaharia</td>
      <td>10</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Liv G</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Mads RingswaldEgan</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Lily Livaudais</td>
      <td>2</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Sarah Scism</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Benjamin</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Reed Locke</td>
      <td>1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Grayce Mores</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Riley Juenemann</td>
      <td>1</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Emily Krouk</td>
      <td>1</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Sydney Clayton</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



It worked! Hooray!

Notably, this whole thing would break down if we have two people that just happen to share a name, i.e. two separate people named "Sarah Smith." This would've caused problems for our team even if we hadn't tried to make an automated scoreboard. If it ever becomes a problem, we'll fix it in a future iteration of the game.

Now let's clean up our dataframe a little bit to rank the players:


```python
players_df = players_df.sort_values(by = 'Total Responses', ascending = False).reset_index()
del players_df['index']
players_df.index += 1
```


```python
# do some datetime stuff
from datetime import datetime
```

# SCOREBOARD  
This is the most recent scoreboard for Enigma!  
  
A few notes:
* This scoreboard does NOT account for correctness of responses submitted. This only counts the number of puzzles that the player has responded to. Our team will check for correctness at the end of the game.
* In case of ties, at the end of the game, we will check to see which player solved their puzzles more quickly. This scoreboard does NOT rank players according to their most recent response; if there is a tie, their ranking is entirely abritrary. 


```python
print("LAST UPDATED: ", datetime.now())
players_df
```

    LAST UPDATED:  2020-10-16 22:22:38.087776





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Total Responses</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Matthew Shernicoff</td>
      <td>16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarah Smith</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Jared Kessler</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Benjamin Villalpando</td>
      <td>11</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Courtney Zaharia</td>
      <td>10</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Benton Meldrum and Emma Brick-Hezeau</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Lily Livaudais</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Liv G</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Mads RingswaldEgan</td>
      <td>1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Sarah Scism</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Benjamin</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Reed Locke</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Grayce Mores</td>
      <td>1</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Riley Juenemann</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Emily Krouk</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Sydney Clayton</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
