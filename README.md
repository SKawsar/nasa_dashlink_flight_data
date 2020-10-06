# nasa_dashlink_flight_data

1. What is the data about? How much data and what kind of data? 5 bullets with numbers to support inference

- The NASA DASH link site has information about 36 aircrafts. Each plane is identified by a three digit tail number. For the initial data analysis, I have used tail 687. For each aircraft, there are 8-9 zip files. Each zip file contains 450-650 .mat files (nearly 800 MB of data). Each .mat contains information during a flight. 

- In the '687200403251438.mat' file, 
    <br>687 represents a plane's tail ID
    <br>2004 is the year
    <br>03 is the month
    <br>25 is the day
    <br>14 is the hour and
    <br>38 is the min [2] 
<br> Each .mat file has 186 feature variables. Along with the data, it contains the sampling rate of each variable, units of a few particular variables, description (full form) of the variables.


- Different feature variable has different sampling rate. 
<br>sampling rate = number of samples / time
<br>Thus, sample time = number of samples / sampling rate 
<br>When the sampling rate = 4 and the number of samples = 16272, sample time = 16272/4 = 4068 seconds


- The dataset can be used for anomaly detection such as engine failures. From [3] if exhaust gas temperature (EGT_1) drops significantly, we know the engine has shutdown. Exhaust gas temperature (EGT_1) depends on fan speed (N1_1), core speed (N2_1) and total air temperature ('TAT). Here, total air temperature has sampling rate 1 and other variables have sampling rate 4.


- From [3], if fuel flow (FF_1) drops significantly, we know the engine has shutdown. Fuel flow (FF_1) depends on power lever angle (PLA_1). Here, all variables have same sampling rate 4.

2. Use any representative sample file to generate some visualizations. Give 5 inference

- From the histogram, we can see that fan speed, core speed, total air temperature, and exhaust gas temperature are not normally distributed.


- From the violinplots, we can the density of the variables. Violinplots also represents the median, Interquartile range (IQR) like boxplots. Fan speed and core speed variables have unit in %RPM  and temperature variables are in DEG. The values of the core speed and fan speed ranges from 0 to 95% RPM. Total air temperature ranges from 13 to 29 DEG. Exhaus gas temperatures ranges from 73 to 605 DEG.


- From the boxplots, we can see that core_speed_3, and exhaust gas temperature variables has few outliers.


- From the heatmap of the Pearson correlation coefficients, the exhaust_gas_temp variables have strong positive linear correlation with the fan speed and core speed variables. The exhaust gas temperature variables have moderate negative correlation with the total air temperature


- We can verify the strong positive correlation of the fan speed, core speed and exhaust gas temperature variables by using the scatterplots and drawing a regression line over the points. For the fan speed greater than 25% RPM, the exhaust gas temperature variables are positively correlated. For the core speed greater than 50% RPM, the exhaust gas temperature variables are positively correlated. 

3. Develop a logic such that you can filter out flight data that looks incorrect. Develop the logic and then validate the logic with different data file.

flight data 1: '687200403251602.mat'
<br>flight data 2: '687200403261438.mat'

- From [3] if exhaust gas temperature (EGT) drops significantly, we know the engine has shutdown. Exhaust gas temperature (EGT) depends on fan speed (N1), core speed (N2) and total air temperature ('TAT'). 
At first, I sampled both the datasets in a same sampling rate 4. Now, let's compare their time-series plot of the variables. 

[**No anomaly**] In flight data 1, the sampling time is 2944 seconds. The aircraft has 4 engines each associated with a fan speed, core speed and exhaust gas temperature. All the four exhaust gas temperature variables drops simultaneously nearly at the end of the sampling time which indicates a safe landing phase.

[**Anomaly**] In flight data 2, the sampling time is 4528 seconds. The aircraft has 4 engines each associated with a fan speed, core speed and exhaust gas temperature. At sampling time 3750 seconds, the fan speed 1 and core speed 1 suddenly significantly reduced and thus exhaust gas temperature 1 is also decreasing drastically. That means engine 1 has shut down before the 800 seconds (nearly 14 minutes) of landing or the aircraft was forced to do landing due to an anomaly in the engine. Then, engine 3, engine 2 and engine 4 shut down respectively.

references: 
<br>[1] https://c3.nasa.gov/dashlink/projects/85/resources/
<br>[2] https://github.com/alacer/flightdata
<br>[3] R. Lee, M. Rajabi, "Assessing NuPIC and CLA in a Machine Learning Context using NASA Aviation Datasets," 
