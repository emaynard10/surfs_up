## Overview of the statistical analysis
Columbia DataBootcamp Challenge to analyze weather data in Oahu to determine the feasibility of a surf and ice cream shop using SQlite, Pandas, Python

### The purpose of the analysis 
This analysis looks at the weather and climate data from weather stations in Hawaii to better understand how well the idea of a ice cream and surf shop would be as a year round business. The analysis uses sqlalchemy to query the two tables in the SQLite file, the Measurements table and Stations table. The data is somewhat limited with the measurements including precipitation and temperature, but lacking cloud cover, wind speed, or any wave/surf data. 

### Results
* The differences between temperatures in Oahu in June and December are relatively small, with a December average temperature of 71 degrees F and a June mean temperature of 74.9F.
* The biggest difference between the two times of year is the minimum temperatures of 56 degrees F in December versus 64 in June.
* The maximum temperatures were quite close 83 versus 85, so the potential for temperature swings are greater in December when the minumum could be a full 83-56 degrees below the max. 

![Screen Shot 2022-06-11 at 12 10 05 PM](https://user-images.githubusercontent.com/99676466/173200829-80e4f153-263f-4f81-944f-8ba81044a97e.png)
![Screen Shot 2022-06-11 at 12 09 33 PM](https://user-images.githubusercontent.com/99676466/173200837-ce846ebc-8539-4f1b-a17d-e857db26b70e.png)

### Summary
The temperatures in Oahu are pretty consistent throughout the year making it ideal for a year round ice cream and surfing business. Some data on surf and waves at different times of the year would also better the analysis. The analysis would benefit from some visualization of the data and some additional information. The following two precipitation queries would add to the analysis:
~~~
june_prcp = session.query(Measurement.date, Measurement.prcp).\
    filter(func.strftime('%m', Measurement.date) == '12').all()

dec_prcp = session.query(Measurement.date, Measurement.prcp).\
    filter(func.strftime('%m', Measurement.date) == '12').all()
~~~

Additional data from the weather stations might narrow down some of this data. Two queries on the elevation of the weather stations might add to the analysis as well since it rains more at higher elevations in the mountains of Hawaii. From these two queries it is clear that the lower stations might be more useful in determining weather at the beach and the analysis could filter the previous two queries to only include data from weather stations at the  lowest elevation. 
~~~
lower = session.query(Station.elevation, Station.station).all()
print(lower)
[(3.0, 'USC00519397'), (14.6, 'USC00513117'), (7.0, 'USC00514830'), (11.9, 'USC00517948'), (306.6, 'USC00518838'), (19.5, 'USC00519523'), (32.9, 'USC00519281'), (0.9, 'USC00511918'), (152.4, 'USC00516128')]
~~~

~~~
june_prcp = session.query(Measurement.prcp).filter(func.strftime('%m', Measurement.date) == '06').\
filter(Measurement.station == 'USC00511918').all()
~~~
~~~
dec_prcp = session.query(Measurement.prcp).filter(func.strftime('%m', Measurement.date) == '12').\
filter(Measurement.station == 'USC00511918').all()
~~~
By comparing the precipitation measurements in June and December at the lowest elevation station, the statistics show that December has the potential for more rain with one day reporting a max 4 inches versus June with a max of .58 inches on one day. Overall though it appears the highest elevation station reported rain more consistently with and average of .64 inches of rain, the rain was spread out over more days, though the station reported less frequently. 

