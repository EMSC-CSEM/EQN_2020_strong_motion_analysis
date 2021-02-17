# Comparison of Earthquake Network's Detections to Strong Motion Accelerometer Data 

## Introduction

The 'Earthquake Network’ (EQN) is an app which detects earthquakes by creating an ad-hoc network of smartphones' accelerometer sensors and provides early warnings for earthquakes via the same smartphone app. Detections are not due to individual smartphone measurements but due to near-simultaneous trigger signals from clusters of smartphones running the app. Therefore detections are normally located in the closest populated regions to an earthquake's epicentre. In order to investigate the mechanisms of EQN's earthquake detection system, we searched for seismic accelerometer stations with publically available data that were close to the EQN detection locations (rather than close to the epicentre). This confirmed that EQN's detections followed strong shaking motions and also that detections could follow both P-phase or S-phase rather than consistantly being sensitive to only one particular phase. It also showed that detections generally occurred between 0 - 5 seconds after the strongest ground acceleration measured by the seismic station. 

This dataset and analysis is a supplement to the article Bossu et al. "Shaking in 5 seconds!" A Voluntary Smartphone-based Earthquake Early Warning System (2021) 

## Data Collection

Analysis was conducted on 550 detections made by the EQN system between 2017-12-15 and 2020-01-31 in Chile, Italy and the USA. Strong motion accelerometer data was collected from seismic stations via the FDSN protocol. The data was calibrated, detrended and a small time shift was applied to correct for differences in distances from the epicentre between the EQN detection and the strong motion seismic station. Calibrated waveform data was obtained for 410 EQN detections. Plots were made for each event and an analysis was carried out on the dataset to compare EQN detection times with the peak ground acceleration measured by the nearest seismic station.

Data was requested using the FDSN protocol via the obspy python library in order to find the closest accelerometer station to the EQN detection with available data. Only stations with 'HN?' channels were considered (see https://ds.iris.edu/ds/nodes/dmc/data/formats/seed-channel-naming/) and only stations up to 20 km from the EQN detection location were considered. Waveform data was retrieved for all 3 components of each accelerometer station for the 30s before and after the moment of the EQN detection. The station inventories were also downloaded via FDSN and their calibration curves were used to calibrate the downloaded waveforms which were converted into acceleration readings in m/s/s. These waveforms were also filtered between 0.5 - 12 Hz and the waveforms The dc offset of the waveform was removed by subtracting the mean value from the waveform. 

A small shift was applied to correct for differences in distances from the epicentre between the EQN detection and the strong motion seismic station. This time shift assumed a seismc phase velocity of 8.04km/s. This was done in order to improve the measured delay between the onset of strong motion and the EQN detection occurances. In fact, the corrections applied were less than 1 s in the majority of cases.

## Dataset

### images_with_time_shift_chosen/

Each file in the folder has a name with the following format:
    `<country>_<EQN_id>_<SM_network>_<SM_station>__<unit>_<start>_<end>.png`

where

 - country is a 3 letter code (iso 3166-1 alpha-3). one of {chl,usa,ita}
 - EQN_id is an random numerical id assigned to each EQN detection
 - SM_network is the network code of the strong motion station
 - SM_station is the station code of the strong motion station
 - unit is always 'acc' for acceleration
 - start is the start time of the waveform shown (UTC time zone iso8601)
 - end is the end time of the waveform shown (UTC time zone iso8601)

Each graph shows the (time shifted) calibrated accelerometer readings for that station, showing the magnitude and the 3 components (Z,N,E for vertical, North and East) of the acceleration measured. There are also labelled markers on the plots. An orange marker shows the time that the EQN detection occurred. Red markers show the time that either P phase or the S phase passed the EQN detection location; these times were calculated using the earthquake parameters from each country's catalogue (CSN for Chile, USGS for USA and INGV for Italy). The time of arrival was calculated using a simple ak135 model that uses only the depth and distance of the location from the epicentre to estimate seismic phases arrival times.

In addition, the graphs have a small green cross on the graph of the overall acceleration. This cross shows the maximum acceleration measured before the EQN detection occurred. This cross is nealy always less than 6s before the EQN detection occurred showing that EQN detection occurred soon after a location specific threshold was passed for ground motion. Ground motion magnitude can vary dramatically over short distances and so the seismic station measurements cannot be quantitatively related to the acceleration experienced at the EQN detection location even though they were less than 20km apart but qualitatively there is a strong correlation.

### strong_motion_analysis.csv 

The file strong_motion_analysis.csv contains a summary of the results of the analysis. The first row contains the field headers.

1. index column
2. peakid - EQN id for that detection
3. detectiontime - time of EQN detection (iso 8601, UTC time zone)
4. country - one of {usa,chl,ita} for USA, Chile or Italy respectively
5. net - seismic network code of strong motion accelerometer station
6. sta - station code of strong motion accelerometer station
7. loc - station location code of strong motion accelerometer station
8. unit - is always 'acc' for acceleration
9. sta_lat - strong motion seismic station latitude
10. sta_lon - strong motion seismic station longitude
11. sta_elv - strong motion seismic station elevation
12. sep_eqn_sta - separation between eqn detection and station location
13. strongest_motion - strongest measured ground motion (m/s/s) measured by the station before the EQN detection
14. strong_motion_time - time of the strongest measured ground motion before the EQN detection 
15. strongest_motion_eqn_delay - delay between the EQN detection and the strongest measured ground motion before the EQN detection
16. dt_correction - correction applied to the waveform to account for difference in radial distances from epicentre of station and EQN detection assuming a seismc phase velocity of 8.04km/s

### Summary Graphs

 - time_correction_applied.png - this histogram shows the time shift applied to the waveforms to account for the differences in radial distance between the seismic station and the EQN location. The histogram shows that the correction was less than 1 s in the majority of cases.

 - delay_strongest_motion_to_eqn.png - this histogram shows the time delay between the strongest ground acceleration measured before the EQN detection and the EQN detection. There is a strong peak for a delay between 2 - 4 s.
 
 - dist_strongest_motion_sta_to_eqn.png - this histogram shows the separation between the EQN detection and the seismic station that provided the strong motion waveform data.
