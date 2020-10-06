# nasa_dashlink_flight_data

What is the data about? How much data and what kind of data? 5 bullets with numbers to support inference
The NASA DASH link site has information about 36 aircrafts. Each plane is identified by a three digit tail number. For the initial data analysis, I have used tail 687. For each aircraft, there are 8-9 zip files. Each zip file contains 450-650 .mat files (nearly 800 MB of data). Each .mat contains information during a flight.

In the '687200403251438.mat' file,
687 represents a plane's tail ID
2004 is the year
03 is the month
25 is the day
14 is the hour and
38 is the min [2]
Each .mat file has 186 feature variables. Along with the data, it contains the sampling rate of each variable, units of a few particular variables, description (full form) of the variables.

Different feature variable has different sampling rate.
sampling rate = number of samples / time
Thus, sample time = number of samples / sampling rate
When the sampling rate = 4 and the number of samples = 16272, sample time = 16272/4 = 4068 seconds
The dataset can be used for anomaly detection such as engine failures. From [3] if exhaust gas temperature (EGT_1) drops significantly, we know the engine has shutdown. Exhaust gas temperature (EGT_1) depends on fan speed (N1_1), core speed (N2_1) and total air temperature ('TAT). Here, total air temperature has sampling rate 1 and other variables have sampling rate 4.
From [3], if fuel flow (FF_1) drops significantly, we know the engine has shutdown. Fuel flow (FF_1) depends on power lever angle (PLA_1). Here, all variables have same sampling rate 4.
references:
[1] https://c3.nasa.gov/dashlink/projects/85/resources/
[2] https://github.com/alacer/flightdata
[3] R. Lee, M. Rajabi, "Assessing NuPIC and CLA in a Machine Learning Context using NASA Aviation Datasets,"
