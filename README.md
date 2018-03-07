
#Your objective is to build a Bubble Plot that showcases the relationship between four key variables:

##Average Fare ($) Per City
##Total Number of Rides Per City
##Total Number of Drivers Per City
##City Type (Urban, Suburban, Rural)

#In addition, you will be expected to produce the following three pie charts:

##% of Total Fares by City Type
##% of Total Rides by City Type
##% of Total Drivers by City Type

#As final considerations:

##You must use the Pandas Library and the Jupyter Notebook.
##You must use the Matplotlib libraries.
##You must include a written description of three observable trends based on the data.
##You must use proper labeling of your plots, including aspects like: Plot Titles, Axes Labels, Legend ##Labels, Wedge Percentages, and Wedge Labels.
##Remember when making your plots to consider aesthetics!
###You must stick to the Pyber color scheme (Gold, Light Sky Blue, and Light Coral) in producing your ###plot and pie charts.
###When making your Bubble Plot, experiment with effects like alpha, edgecolor, and linewidths.
###When making your Pie Chart, experiment with effects like shadow, startangle, and explosion.

#You must include an exported markdown version of your Notebook called  README.md in your GitHub ##repository.
#See Example Solution for a reference on expected format.


```python
#Import Dependencies
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import plotly.plotly as py
import plotly as pl
import cufflinks as cf
import plotly.tools as tls
from plotly.graph_objs import Scatter, Layout
from plotly.offline import plot
import plotly.graph_objs as go
pl.offline.init_notebook_mode(connected=True)
pl.tools.set_credentials_file(username='davidhoebbel', api_key='obcfaofD1NR2Oa9kiS3D')
```


<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>



```python
# Read CSV
city_data = pd.read_csv("Resources/city_data.csv")
ride_data = pd.read_csv("Resources/ride_data.csv")
```


```python
#Print Abridged CSV Data Tables
city_data.set_index('city')
city_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Print abridged CSV data tables
ride_data.set_index('city')
ride_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Merge data tables on city
#pd.join(other, lsuffix='city', rsuffix='city')
#Need to get into jupyter from earlier folder
uber_merge = pd.merge(city_data, ride_data, on="city")
uber_merge.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-19 04:27:52</td>
      <td>5.51</td>
      <td>6246006544795</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-04-17 06:59:50</td>
      <td>5.54</td>
      <td>7466473222333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-05-04 15:06:07</td>
      <td>30.54</td>
      <td>2140501382736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-01-25 20:44:56</td>
      <td>12.08</td>
      <td>1896987891309</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>2016-08-09 18:19:47</td>
      <td>17.91</td>
      <td>8784212854829</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Groupby to find average per city
##Average Fare ($) Per City
city_fare = uber_merge.groupby(['city'], as_index=False)
city_fare["fare"].mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>23.928710</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>20.609615</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>37.315556</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>23.625000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>21.981579</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Arnoldview</td>
      <td>25.106452</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Campbellport</td>
      <td>33.711333</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Carrollbury</td>
      <td>36.606000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Carrollfort</td>
      <td>25.395517</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Clarkstad</td>
      <td>31.051667</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Conwaymouth</td>
      <td>34.591818</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Davidtown</td>
      <td>22.978095</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Davistown</td>
      <td>21.497200</td>
    </tr>
    <tr>
      <th>13</th>
      <td>East Cherylfurt</td>
      <td>31.416154</td>
    </tr>
    <tr>
      <th>14</th>
      <td>East Douglas</td>
      <td>26.169091</td>
    </tr>
    <tr>
      <th>15</th>
      <td>East Erin</td>
      <td>24.478214</td>
    </tr>
    <tr>
      <th>16</th>
      <td>East Jenniferchester</td>
      <td>32.599474</td>
    </tr>
    <tr>
      <th>17</th>
      <td>East Leslie</td>
      <td>33.660909</td>
    </tr>
    <tr>
      <th>18</th>
      <td>East Stephen</td>
      <td>39.053000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>East Troybury</td>
      <td>33.244286</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Edwardsbury</td>
      <td>26.876667</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Erikport</td>
      <td>30.043750</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Eriktown</td>
      <td>25.478947</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Floresberg</td>
      <td>32.310000</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Fosterside</td>
      <td>23.034583</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Hernandezshire</td>
      <td>32.002222</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Horneland</td>
      <td>21.482500</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Jacksonfort</td>
      <td>32.006667</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Jacobfort</td>
      <td>24.779355</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Jasonfort</td>
      <td>27.831667</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>South Roy</td>
      <td>26.031364</td>
    </tr>
    <tr>
      <th>96</th>
      <td>South Shannonborough</td>
      <td>26.516667</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Spencertown</td>
      <td>23.681154</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Stevensport</td>
      <td>31.948000</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Stewartview</td>
      <td>21.614000</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Swansonbury</td>
      <td>27.464706</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Thomastown</td>
      <td>30.308333</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Tiffanyton</td>
      <td>28.510000</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Torresshire</td>
      <td>24.207308</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Travisville</td>
      <td>27.220870</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Vickimouth</td>
      <td>21.474667</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Webstertown</td>
      <td>29.721250</td>
    </tr>
    <tr>
      <th>107</th>
      <td>West Alexis</td>
      <td>19.523000</td>
    </tr>
    <tr>
      <th>108</th>
      <td>West Brandy</td>
      <td>24.157667</td>
    </tr>
    <tr>
      <th>109</th>
      <td>West Brittanyton</td>
      <td>25.436250</td>
    </tr>
    <tr>
      <th>110</th>
      <td>West Dawnfurt</td>
      <td>22.330345</td>
    </tr>
    <tr>
      <th>111</th>
      <td>West Evan</td>
      <td>27.013333</td>
    </tr>
    <tr>
      <th>112</th>
      <td>West Jefferyfurt</td>
      <td>21.072857</td>
    </tr>
    <tr>
      <th>113</th>
      <td>West Kevintown</td>
      <td>21.528571</td>
    </tr>
    <tr>
      <th>114</th>
      <td>West Oscar</td>
      <td>24.280000</td>
    </tr>
    <tr>
      <th>115</th>
      <td>West Pamelaborough</td>
      <td>33.799286</td>
    </tr>
    <tr>
      <th>116</th>
      <td>West Paulport</td>
      <td>33.278235</td>
    </tr>
    <tr>
      <th>117</th>
      <td>West Peter</td>
      <td>24.875484</td>
    </tr>
    <tr>
      <th>118</th>
      <td>West Sydneyhaven</td>
      <td>22.368333</td>
    </tr>
    <tr>
      <th>119</th>
      <td>West Tony</td>
      <td>29.609474</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Williamchester</td>
      <td>34.278182</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Williamshire</td>
      <td>26.990323</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Wiseborough</td>
      <td>22.676842</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Yolandafurt</td>
      <td>27.205500</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Zimmermanmouth</td>
      <td>28.301667</td>
    </tr>
  </tbody>
</table>
<p>125 rows × 2 columns</p>
</div>




```python
##Total Number of Rides Per City
ride_count = uber_merge.groupby(['city'], as_index=False)
ride_count["ride_id"].count()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>22</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>19</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Arnoldview</td>
      <td>31</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Campbellport</td>
      <td>15</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Carrollbury</td>
      <td>10</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Carrollfort</td>
      <td>29</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Clarkstad</td>
      <td>12</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Conwaymouth</td>
      <td>11</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Davidtown</td>
      <td>21</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Davistown</td>
      <td>25</td>
    </tr>
    <tr>
      <th>13</th>
      <td>East Cherylfurt</td>
      <td>13</td>
    </tr>
    <tr>
      <th>14</th>
      <td>East Douglas</td>
      <td>22</td>
    </tr>
    <tr>
      <th>15</th>
      <td>East Erin</td>
      <td>28</td>
    </tr>
    <tr>
      <th>16</th>
      <td>East Jenniferchester</td>
      <td>19</td>
    </tr>
    <tr>
      <th>17</th>
      <td>East Leslie</td>
      <td>11</td>
    </tr>
    <tr>
      <th>18</th>
      <td>East Stephen</td>
      <td>10</td>
    </tr>
    <tr>
      <th>19</th>
      <td>East Troybury</td>
      <td>7</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Edwardsbury</td>
      <td>27</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Erikport</td>
      <td>8</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Eriktown</td>
      <td>19</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Floresberg</td>
      <td>10</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Fosterside</td>
      <td>24</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Hernandezshire</td>
      <td>9</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Horneland</td>
      <td>4</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Jacksonfort</td>
      <td>6</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Jacobfort</td>
      <td>31</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Jasonfort</td>
      <td>12</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>South Roy</td>
      <td>22</td>
    </tr>
    <tr>
      <th>96</th>
      <td>South Shannonborough</td>
      <td>15</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Spencertown</td>
      <td>26</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Stevensport</td>
      <td>5</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Stewartview</td>
      <td>30</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Swansonbury</td>
      <td>34</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Thomastown</td>
      <td>24</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Tiffanyton</td>
      <td>13</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Torresshire</td>
      <td>26</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Travisville</td>
      <td>23</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Vickimouth</td>
      <td>15</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Webstertown</td>
      <td>16</td>
    </tr>
    <tr>
      <th>107</th>
      <td>West Alexis</td>
      <td>20</td>
    </tr>
    <tr>
      <th>108</th>
      <td>West Brandy</td>
      <td>30</td>
    </tr>
    <tr>
      <th>109</th>
      <td>West Brittanyton</td>
      <td>24</td>
    </tr>
    <tr>
      <th>110</th>
      <td>West Dawnfurt</td>
      <td>29</td>
    </tr>
    <tr>
      <th>111</th>
      <td>West Evan</td>
      <td>12</td>
    </tr>
    <tr>
      <th>112</th>
      <td>West Jefferyfurt</td>
      <td>21</td>
    </tr>
    <tr>
      <th>113</th>
      <td>West Kevintown</td>
      <td>7</td>
    </tr>
    <tr>
      <th>114</th>
      <td>West Oscar</td>
      <td>29</td>
    </tr>
    <tr>
      <th>115</th>
      <td>West Pamelaborough</td>
      <td>14</td>
    </tr>
    <tr>
      <th>116</th>
      <td>West Paulport</td>
      <td>17</td>
    </tr>
    <tr>
      <th>117</th>
      <td>West Peter</td>
      <td>31</td>
    </tr>
    <tr>
      <th>118</th>
      <td>West Sydneyhaven</td>
      <td>18</td>
    </tr>
    <tr>
      <th>119</th>
      <td>West Tony</td>
      <td>19</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Williamchester</td>
      <td>11</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Williamshire</td>
      <td>31</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Wiseborough</td>
      <td>19</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Yolandafurt</td>
      <td>20</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Zimmermanmouth</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
<p>125 rows × 2 columns</p>
</div>




```python
##Total Number of Drivers Per City
city_drivers = uber_merge.groupby(['city'], as_index=False)
city_drivers["driver_count"].mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alvarezhaven</td>
      <td>21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alyssaberg</td>
      <td>67</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anitamouth</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Antoniomouth</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aprilchester</td>
      <td>49</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Arnoldview</td>
      <td>41</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Campbellport</td>
      <td>26</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Carrollbury</td>
      <td>4</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Carrollfort</td>
      <td>55</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Clarkstad</td>
      <td>21</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Conwaymouth</td>
      <td>18</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Davidtown</td>
      <td>73</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Davistown</td>
      <td>25</td>
    </tr>
    <tr>
      <th>13</th>
      <td>East Cherylfurt</td>
      <td>9</td>
    </tr>
    <tr>
      <th>14</th>
      <td>East Douglas</td>
      <td>12</td>
    </tr>
    <tr>
      <th>15</th>
      <td>East Erin</td>
      <td>43</td>
    </tr>
    <tr>
      <th>16</th>
      <td>East Jenniferchester</td>
      <td>22</td>
    </tr>
    <tr>
      <th>17</th>
      <td>East Leslie</td>
      <td>9</td>
    </tr>
    <tr>
      <th>18</th>
      <td>East Stephen</td>
      <td>6</td>
    </tr>
    <tr>
      <th>19</th>
      <td>East Troybury</td>
      <td>3</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Edwardsbury</td>
      <td>11</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Erikport</td>
      <td>3</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Eriktown</td>
      <td>15</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Floresberg</td>
      <td>7</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Fosterside</td>
      <td>69</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Hernandezshire</td>
      <td>10</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Horneland</td>
      <td>8</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Jacksonfort</td>
      <td>6</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Jacobfort</td>
      <td>52</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Jasonfort</td>
      <td>25</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>South Roy</td>
      <td>35</td>
    </tr>
    <tr>
      <th>96</th>
      <td>South Shannonborough</td>
      <td>9</td>
    </tr>
    <tr>
      <th>97</th>
      <td>Spencertown</td>
      <td>68</td>
    </tr>
    <tr>
      <th>98</th>
      <td>Stevensport</td>
      <td>6</td>
    </tr>
    <tr>
      <th>99</th>
      <td>Stewartview</td>
      <td>49</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Swansonbury</td>
      <td>64</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Thomastown</td>
      <td>1</td>
    </tr>
    <tr>
      <th>102</th>
      <td>Tiffanyton</td>
      <td>21</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Torresshire</td>
      <td>70</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Travisville</td>
      <td>37</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Vickimouth</td>
      <td>13</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Webstertown</td>
      <td>26</td>
    </tr>
    <tr>
      <th>107</th>
      <td>West Alexis</td>
      <td>47</td>
    </tr>
    <tr>
      <th>108</th>
      <td>West Brandy</td>
      <td>12</td>
    </tr>
    <tr>
      <th>109</th>
      <td>West Brittanyton</td>
      <td>9</td>
    </tr>
    <tr>
      <th>110</th>
      <td>West Dawnfurt</td>
      <td>34</td>
    </tr>
    <tr>
      <th>111</th>
      <td>West Evan</td>
      <td>4</td>
    </tr>
    <tr>
      <th>112</th>
      <td>West Jefferyfurt</td>
      <td>65</td>
    </tr>
    <tr>
      <th>113</th>
      <td>West Kevintown</td>
      <td>5</td>
    </tr>
    <tr>
      <th>114</th>
      <td>West Oscar</td>
      <td>11</td>
    </tr>
    <tr>
      <th>115</th>
      <td>West Pamelaborough</td>
      <td>27</td>
    </tr>
    <tr>
      <th>116</th>
      <td>West Paulport</td>
      <td>5</td>
    </tr>
    <tr>
      <th>117</th>
      <td>West Peter</td>
      <td>61</td>
    </tr>
    <tr>
      <th>118</th>
      <td>West Sydneyhaven</td>
      <td>70</td>
    </tr>
    <tr>
      <th>119</th>
      <td>West Tony</td>
      <td>17</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Williamchester</td>
      <td>26</td>
    </tr>
    <tr>
      <th>121</th>
      <td>Williamshire</td>
      <td>70</td>
    </tr>
    <tr>
      <th>122</th>
      <td>Wiseborough</td>
      <td>55</td>
    </tr>
    <tr>
      <th>123</th>
      <td>Yolandafurt</td>
      <td>7</td>
    </tr>
    <tr>
      <th>124</th>
      <td>Zimmermanmouth</td>
      <td>45</td>
    </tr>
  </tbody>
</table>
<p>125 rows × 2 columns</p>
</div>




```python
#uber_merge
#city_fare
#ride_count
#city_drivers
#merge into single table
#pd.merge(uber_merge, city_fare, ride_count, city_drivers, on=["Index"], how='inner',suffixes=('_fare','_count','_drivers'))

```


```python
#Consolidate groupby values into single table for charting purposes

f = {'fare':['mean'], 'ride_id':['count'], 'driver_count':['mean']}

merged_df = uber_merge.groupby('city').agg(f)
merged_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th></th>
      <th>mean</th>
      <th>count</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.928710</td>
      <td>31</td>
      <td>21</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.609615</td>
      <td>26</td>
      <td>67</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37.315556</td>
      <td>9</td>
      <td>16</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.625000</td>
      <td>22</td>
      <td>21</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.981579</td>
      <td>19</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Use DataFrame.plot() in order to create a bar chart of the data
merged_df.iplot(kind='bubble', x='fare', y='ride_id', size='driver_count', xTitle='Avg Fare', yTitle='Rider Count',
             filename='bubblechart')

# Set a title for the chart
plt.title("Avg Price vs. Ride Count vs. Driver Count")

axes = plt.gca()
axes.set_xlim([0,70])
axes.set_ylim([0,70])

plt.show()
```

    High five! You successfully sent some data to your account on plotly. View your plot in your browser at https://plot.ly/~davidhoebbel/0 or inside your plot.ly account where it is named 'bubblechart'
    


![png](output_11_1.png)



```python
#Consolidate groupby values into single table for charting purposes

m = {'fare':['sum'], 'ride_id':['count'], 'driver_count':['mean']}

merged_df2 = uber_merge.groupby(['type']).agg(m)
merged_df2.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th></th>
      <th>sum</th>
      <th>count</th>
      <th>mean</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
      <td>125</td>
      <td>5.816000</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
      <td>657</td>
      <td>14.809741</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
      <td>1625</td>
      <td>39.692923</td>
    </tr>
  </tbody>
</table>
</div>




```python
##City Type (Urban, Suburban, Rural)
#In addition, you will be expected to produce the following three pie charts:
##% of Total Fares by City Type
##% of Total Rides by City Type
##% of Total Drivers by City Type
merged_df2['fare %'] = (merged_df2['fare'])/(merged_df2['fare'].sum())
merged_df2['ride %'] = (merged_df2['ride_id'])/(merged_df2['ride_id'].sum())
merged_df2['driver %'] = (merged_df2['driver_count'])/(merged_df2['driver_count'].sum())
merged_df2
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>fare %</th>
      <th>ride %</th>
      <th>driver %</th>
    </tr>
    <tr>
      <th></th>
      <th>sum</th>
      <th>count</th>
      <th>mean</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
      <td>125</td>
      <td>5.816000</td>
      <td>0.065798</td>
      <td>0.051932</td>
      <td>0.096421</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
      <td>657</td>
      <td>14.809741</td>
      <td>0.314458</td>
      <td>0.272954</td>
      <td>0.245525</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
      <td>1625</td>
      <td>39.692923</td>
      <td>0.619745</td>
      <td>0.675114</td>
      <td>0.658054</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Fare % by city type
plt.pie(merged_df2['fare %'])
plt.show()
```


![png](output_14_0.png)



```python
# ride % by city type
plt.pie(merged_df2['ride %'])
plt.show()
```


![png](output_15_0.png)



```python
# driver % by city type
plt.pie(merged_df2['driver %'])
plt.show()
```


![png](output_16_0.png)



```python
#Observable Trends
#1. Higher average fares associated with cities that have smaller amounts of Uber drivers
#2. Rider count and driver count are positively correlated
#3. Urban cities have more riders, drivers and a higher share of the overall fares. This is followed by Suburban then Rural cities.
```
