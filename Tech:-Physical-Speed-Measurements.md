While the core idea behind K40Nano was to provide a solid interface API. Some care was taken to exactly duplicate some of the math and speedcode numbers and methods of the Chinese software.

For completeness this needs to be checked with physical speed measurements. Rather than simply taking the word of the software. The conversion within K40Nano is precise with regard to correctly reflecting the speed requested to the Chinese Software and the speedcode it utilized. It is not however precise with regard the actual physical speeds. If the Chinese Software messed up the math, it is impossible to detect this in software alone. It's entirely reasonable to know that 2 mm/s is the same in both the Chinese Software and K40Nano, however without physical measuring the actual speed this is not actually something we can know.

# Physical Speed Tests

## Initial Test
Using a camera I conducted preliminary experiments on my Nano-M2 board, in each test I drew 1 inch square, the total travel distance is 4 inches:

* 2mm/s should take 50.8 seconds.
    * Experimental result: 55.1 ± 0.13 seconds
    * This means it was travelling at 1.84392015 mm/s
    * This is an error of 7.8039925 %
* 5mm/s should take 20.32 seconds.
    * Experimental result: 22.1 ± 0.13 seconds
    * This means it was travelling at 4.59728507 mm/s
    * This is an error of 8.0542986 %
* 10mm/s should take 10.16 seconds.
    * Experimental result: 10.28 ± 0.13 seconds
    * This means it was travelling at 9.88326848 mm/s
    * This is an error of 1.1673152 %
* 20 mm/s should take 5.08 seconds.
    * Experimental result: 5.13 ± 0.13 seconds.
    * This means it was travelling at 19.8050682 mm/s
    * This is an error of 0.974659 %


Since I am using a camera at 30 frames per second, I have an error range of about 4 frames. That's about 0.13 seconds. In the case of the 10mm/s and 20mm/s times these are within those ranges. However the 2mm/s and 5mm/s values are so far outside the correct range and with much more time to average it out, that these speeds are almost certainly in error. The speed code equation changes at 7mm/s at the start of suffix-C notation. This means that speeds at that range are almost certainly in error. And seem to be ~8% slower than the advertised speed.

This means further research is required.

## Substantive Test

Conducting a much high sample size test.

Speed requested, speedcode parse, time taken, mils/s, mm/s.

* 50 ,  (54260, 2, 51, 31, 0) , 220.908967018 ,  1810.70060396 , 45.9917705050564, 0.919835410101127
* 40 ,  (52719, 2, 41, 49, 0) , 276.245445967 ,  1447.98767125 , 36.7788669892975, 0.919471674732436
* 30 ,  (50155, 2, 31, 86, 0) , 368.197537899 ,  1086.3733698 , 27.593868692272, 0.919795623075735
* 20 ,  (45023, 1, 21, 191, 0) , 551.818667173 ,  724.875803947 , 18.4118354778444, 0.920591773892217
* 10 ,  (29632, 1, 11, 730, 0) , 1102.64568996 ,  362.76385392 , 9.21419691389303, 0.921419691389303
* 6 ,  (61253, 0, 7, 159, 0) , 92.182819128 ,  216.960168817 , 5.5107853121128, 0.918464218685466
* 5 ,  (60398, 0, 6, 223, 0) , 110.673449993 ,  180.711814814 , 4.5900776176376, 0.918015523527519
* 4 ,  (59114, 0, 5, 335, 0) , 138.305717945 ,  144.607181085 , 3.67302041612884, 0.91825510403221
* 3 ,  (56976, 0, 4, 558, 0) , 184.20176506 ,  108.576592594 , 2.75784396264297, 0.91928132088099
* 2 ,  (52701, 0, 3, 1116, 0) , 276.145143032 ,  72.425680859 , 1.83961130042956, 0.919805650214782

The average of these average speeds is 0.919493599053179 which is 8.0506400946821% slower than they should be.

![requested-v-actual](https://user-images.githubusercontent.com/3302478/58852548-dbcfab00-864b-11e9-8e97-ce7161ee36c1.png)

Plugging this back into the original equation gives us an effective slope 11144.2624205245 rather than 12120.

11148 is the closest value evenly divisible by 12.

## Conclusion
This research shows that the effective speeds given by the original chinese software are an error and as such implies that we should correct them. In order to be properly backwards compatible this means not touching the "A", "B", "B1", "B2", "M", "M1", "M2" designations. So the new corrected speeds will be called BOARD-XX where XX is the board designation. Without a physical A or B board to work with, these values will remain unknown.

## inpain's Logical Analyzer

Inpain hooked this stuff directly to a logical analyzer.

Speedcode -- analysis -- speed range -- calculated speed by laserspeed.
* 255 000C - 65.700µs to 66.000µs - 15.221kHz to 15.152kHz" -  96.65335 mm/s to 96.2152 mm/s - 95.14758064516128 mm/s
* 254 000C - 134.800µs to 135.100µs - 7.418kHz to 7.402kHz" - 47.1043 mm/s to 47.0027 mm/s - 46.81865079365079 mm/s
* 250 000C - 411.100µs to 411.400µs - 2.432kHz to 2.431kHz" - 15.4432 mm/s to 15.43685 mm/s -  15.442801047120417 mm/s

These are testing speeds in speed accel-1 with suffix-C set.

***

# d_ratio tests

The theory that the d_ratio should be `sqrt(2) - 1' rather than the default Chinese software value needs to be tested.

## Initial Test

The initial physical test was conducted by cutting hourglass shapes stacked sheets of paper and observing the difference in cut depth. This showed after a number of tests that the difference was very minor but that with the d_ratio at `sqrt(2) -1` rather than default, the cut depths were more consistent.

## Scorch's Methodology

However, Scorch devised a different test measuring the laser-on time given lines set to different angles. So not simply purely orthogonal or diagonal but actual angles normal people would cut, or various curves as simulated by line draw algorithms. It's clear that this will have a series of errors overall because the difference between a line simulated by a diagonals and orthogonal movements is going to be different than a true vector line.


Speed 10, default.
* speed, angle, speed_code_values, time in seconds, mils / sec, mm/s, error.
* 10 , 0 , (29632, 1, 11, 730, 0) , 109.895877838 , 358.248196152 
* 10 , 5 , (29632, 1, 11, 730, 0) , 110.627748966 , 355.878162287
* 10 , 10 , (29632, 1, 11, 730, 0) , 110.283801794 , 356.988055903
* 10 , 15 , (29632, 1, 11, 730, 0) , 109.135720968 , 360.743482067
* 10 , 20 , (29632, 1, 11, 730, 0) , 109.855480909 , 358.37993402
* 10 , 25 , (29632, 1, 11, 730, 0) , 122.59345603 , 321.142753251
* 10 , 30 , (29632, 1, 11, 730, 0) , 120.207882881 , 327.515958657
* 10 , 35 , (29632, 1, 11, 730, 0) , 113.850861073 , 345.80326955
* 10 , 40 , (29632, 1, 11, 730, 0) , 106.641429901 , 369.181096282 
* 10 , 45 , (29632, 1, 11, 730, 0) , 98.5734059811 , 399.397784911 
* 10 , 50 , (29632, 1, 11, 730, 0) , 106.618573904 , 369.260238234 
* 10 , 55 , (29632, 1, 11, 730, 0) , 113.85829401 , 345.780694698 
* 10 , 60 , (29632, 1, 11, 730, 0) , 120.247364044 , 327.4084244
* 10 , 65 , (29632, 1, 11, 730, 0) , 122.533529043 , 321.299813263
* 10 , 70 , (29632, 1, 11, 730, 0) , 109.798930883 , 358.564511359
* 10 , 75 , (29632, 1, 11, 730, 0) , 109.152378082 , 360.688431088
* 10 , 80 , (29632, 1, 11, 730, 0) , 110.337400913 , 356.814640132
* 10 , 85 , (29632, 1, 11, 730, 0) , 110.568897963 , 356.067580716 
* 10 , 90 , (29632, 1, 11, 730, 0) , 110.175065994 , 357.340380464

Speed 10, sqrt(2) -1
* 10 , 0 , (29632, 1, 11, 1159, 0) , 110.024923086 , 357.828016559 
* 10 , 5 , (29632, 1, 11, 1159, 0) , 111.170078993 , 354.14205294 
* 10 , 10 , (29632, 1, 11, 1159, 0) , 111.398823977 , 353.414861976 
* 10 , 15 , (29632, 1, 11, 1159, 0) , 110.891434193 , 355.031930885 
* 10 , 20 , (29632, 1, 11, 1159, 0) , 113.619389057 , 346.507760046 
* 10 , 25 , (29632, 1, 11, 1159, 0) , 136.000452995 , 289.484329889
* 10 , 30 , (29632, 1, 11, 1159, 0) , 134.742800951 , 292.186296575
* 10 , 35 , (29632, 1, 11, 1159, 0) , 127.671055794 , 308.370599391
* 10 , 40 , (29632, 1, 11, 1159, 0) , 119.456996918 , 329.574667168 
* 10 , 45 , (29632, 1, 11, 1159, 0) , 110.331727028 , 356.832989572
* 10 , 50 , (29632, 1, 11, 1159, 0) , 119.469056129 , 329.541399886 
* 10 , 55 , (29632, 1, 11, 1159, 0) , 127.561619043 , 308.635154487 
* 10 , 60 , (29632, 1, 11, 1159, 0) , 134.789241076 , 292.085627056 
* 10 , 65 , (29632, 1, 11, 1159, 0) , 135.900583982 , 289.697062708 
* 10 , 70 , (29632, 1, 11, 1159, 0) , 113.581404924 , 346.623639901 
* 10 , 75 , (29632, 1, 11, 1159, 0) , 110.70959115 , 355.615078973
* 10 , 80 , (29632, 1, 11, 1159, 0) , 111.324310064 , 353.651416993 
* 10 , 85 , (29632, 1, 11, 1159, 0) , 111.289330959 , 353.762572392
* 10 , 90 , (29632, 1, 11, 1159, 0) , 110.087687969 , 357.624006156 

![chinese-sqrt2](https://user-images.githubusercontent.com/3302478/58851638-6adac400-8648-11e9-8977-4712a90a5bdc.png)
## Conclusion

From this we can see that while my number is better at 45 degrees being perfectly able to touch the midpoint line at the center, it is not better overall. The chinese value actually normalizes the error between the positive and negative amounts. Giving overall error below 2% whereas my version gives errors above 3%. Just because all the lines drawn are mixes of orthogonal and diagonal cuts, doesn't mean perfect orthogonal and diagonal line cuts are going to be best. The chinese value properly minimizes the overall error.

If you are only going to make diagonal and orthogonal cuts there's something to using my given value. However if we are agnostic to the cut angles we will use, the Chinese values are correct.

There is room for some future research. The Chinese value is derived by measuring and averaging the value it must be using and calculating it accordingly. However, there is some ground for rederiving this value, or making an additional improvement. This problem relates closely to the problem of using bezier curves to simulate an arc. 

See:
* Spencer Mortensen, [Approximate a circle with cubic Bézier curves](http://spencermortensen.com/articles/bezier-circle/)
* And my own research on the Bézier problem to improve on Mortensen's: [Approximating an arc with cubic bezier curves, yet another metric.](http://godsnotwheregodsnot.blogspot.com/2017/06/approximating-arc-with-cubic-bezier.html)

We could, if we properly had the errors calculated, find a way to minimize the overall error rather than just the error-extrema. Which might be judged to be a superior value. However, these tests amply demonstrate what the Chinese value is doing, and as such, it's much more clever than I originally gave it credit for and it should remain in place.