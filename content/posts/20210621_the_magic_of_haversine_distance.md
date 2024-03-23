+++
title = "The Magic of Haversine Distance"
date = 2021-06-21T00:00:00+07:00
description = ""
type = "post"
showTableOfContents = "true"
+++

*NOTE: This post is originally written for Ninja Van Tech. You can read the original post [here](https://medium.com/ninjavan-tech/the-magic-of-haversine-distance-1c4e1641d880).*

In Ninja Van, customer happiness is a must. We have to make sure that customer receives their parcel in-time, depending on which shipping they choose (either one-day / two-day shipping). Therefore, we need to have a robust service-level agreement (SLA) system to make sure no shipments are delayed.

Part of our SLA system is estimation when the parcel will be arrived. To estimate when will the shipments will reach destination warehouse, we use Haversine Distance formula to calculate distance between 2 locations, so that we can find the fastest way to reach destination warehouse. But of course, you readers are going to ask these questions:
1) **Why not use road distance instead?** Because use road distance is too complex (at least in the implementation, either by using OpenStreetMap API or Google Maps API). Also, it doesn't make too much difference in terms of time estimation, because there's only 1 warehouse each city. Example: distance between Jakarta city and Surabaya city directly is 800 kilometers, while distance between Jakarta city and Surabaya city through Yogyakarta city is more or less 950 kilometers. Those numbers aren't exact road distances, but in terms of SLA calculation, the rough distance is enough to determine which way to go.
2) **Why not use the simple Pythagoras theorem?** This will be answered in the rest of the article.

# Earth Is Round, But Our Map is Flat

Well, we know that the earth is round. But, most of our maps are projected in 2D. Why? The reason is that because [2D map format is already used since the beginning of navigation era](https://en.wikipedia.org/wiki/History_of_cartography). Land and sea travelers draw map in paper (there is even a knowledge field for it called [cartography](https://en.wikipedia.org/wiki/Cartography)) and use them to navigate between cities or kingdoms in the past, either for trade, war, or for other purposes. Even after earth is found out to be a sphere, 2D map in a paper is still used, because it's the most practical way to be brought in travel. There is a round map though, which is called globe, but simply it is not practical.

## Projection

When it comes to projection, there has to be a carefully-thought method to do that, otherwise we're doomed with wrong projection. Wrong projection can cause us to use the wrong route, or it could mislead anyone on other occasions.

So, how to project? There are many projection methods, but the most popular is [Mercator Projection](https://www.youtube.com/watch?v=kIID5FDi2JQ). If we want to preserve distance and angle, this is the best method to project / convert a 3D sphere planet into 2D. Mercator Projection is used in Google Maps, because it is good for in-city navigation. But, this projection is known for stretching some area, like Greenland & Antartica.

## Coordinate System

![Cartesian coordinate system. Source: Wikipedia.](/images/cartesian-coordinate.png)

Now that we project maps into 2D, usually we use a simple coordinate system, which is Cartesian coordinate. It has 2 axes (x and y). Distance between 2 points can be calculated with Pythagoras' theorem. It's simple and mostly accurate in small scales.

![Latitude and longitude coordinate representation. Source: Wikipedia.](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*c14AiYwMnLU0G6Yv8alJfg.png)

But of course, logistics companies like Ninja Van didn't send parcels in the city only. Ninja Van operates in large countries (e.g. Indonesia, Philippines, and Malaysia) & has a cross-country shipping. So, we resort to the "official" coordinate system used internationally, which called [Geographic coordinate system](https://en.wikipedia.org/wiki/Geographic_coordinate_system). This coordinate system uses latitude and longitude instead of x and y. Wait, what's the difference? The difference lies in how to measure the latitude-longitude vs x-y coordinate. In Cartesian system, as mentioned, a coordinate is measured by calculating the distance from the central point. But, in latitude-longitude coordinate system, the coordinate is measured as angle from the equator.

With the use of latitude and longitude, there is a challenge to **accurately calculate distance between 2 cities**, because latitude and longitude takes into account the spherical aspect of the earth.

# How To Measure The Distance Between Two Cities?

Well, there's a specific way for that. It's called Haversine Distance. Haversine Distance is a mathematical way to calculate distance between 2 cities given the latitude and longitude coordinate of each city. Since Haversine Distance uses latitude and longitude, it already takes into account the spherical aspect of the earth. So, [how does it work](https://www.youtube.com/watch?v=nsVsdHeTXIE)?

![Central angle formula. Generated by CODECOGS.](https://miro.medium.com/v2/resize:fit:444/format:webp/1*74tsZ1-Y4oP1U_tkF9R9uQ.png)

Look at above formula. That formula defines the relation between [central angle (Θ)](https://en.wikipedia.org/wiki/Central_angle), distance between 2 points (d), and radius (r). If we want to find the distance, we need to re-formulate that equation into this:

![Distance formula given central angle and radius. Generated by CODECOGS.](https://miro.medium.com/v2/resize:fit:480/format:webp/1*KcCPMpAzHn4T1g-bYJ1EYA.png)

Now, we need to find the value of Θ and r, in this case earth's radius. We know that earth's radius (on average) is 6371 km. Now, we need to find the central angle.

To find out central angle, we need to know about Haversine Formula, which is to calculate haversine of central angle. The Haversine Formula is defined like this:

![Haversine Formula. Generated by CODECOGS.](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*Ro6J2d2B_5PVrAs8ajcUQw.png)

Wait, what are those symbols? The φ (phi) symbol is latitude, while λ (lambda) symbol is longitude. Remember, there are 2 cities (or 2 points). We need to fill the equation with latitude and longitude of those points.

But, why there's another `hav()` function in the formula? "Is that some sort of recursive function?" LOL! That's not recursive, but rather the haversine function (defined below).

![Haversine function. Source: Wikipedia.](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*3bFk3q7G4_zK1_Cl9bxSJA.png)

Now, after finding haversine of central angle, now, we need to get the central angle by using archaversine.[Archaversine](https://en.wikipedia.org/wiki/Versine#ahav) of hav(θ) is defined as:

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*PBVqvm-pxNdLoya9RQcybg.png)

If we use central angle, then the formula becomes:

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*yDZBP21VPYfdgsE7_9Nmeg.png)

And that's how you get the central angle.

**NOTE:** there are 2 similar terms in this subsection, which is *formula* and *function*. Although similar, they are not the same. There is a post about that [here](https://www.calculushowto.com/formula-vs-function/).

# The Implementation

Now, after we know how to calculate Haversine Distance, it's time to implement it.

After doing a little bit of Googling on converting degree to radians, using Go Language's `math` library, the implementation is finished. It looks like this:
```go
package main

import (
	"fmt"
	"math"
)

const earthRadiusKM = 6371.0

type Location struct {
	Latitude  float64
	Longitude float64
}

func degreeToRadians(angle float64) float64 {
	return angle * (math.Pi / 180.0)
}

func NewLocation(latDegree float64, longDegree float64) Location {
	return Location{
		Latitude:  degreeToRadians(latDegree),
		Longitude: degreeToRadians(longDegree),
	}
}

func haversineFn(angleRad float64) float64 {
	return (1 - math.Cos(angleRad)) / 2.0
}

func haversineFormula(firstCity Location, secondCity Location) float64 {
	// first, find the difference between latitudes and longitudes
	latitudeDiff := firstCity.Latitude - secondCity.Latitude
	longitudeDiff := firstCity.Longitude - secondCity.Longitude

	// then, compute the haversine of those variables
	havLatitude := haversineFn(latitudeDiff)
	havLongitude := haversineFn(longitudeDiff)

	// last, put the results into formula
	return havLatitude + math.Cos(firstCity.Latitude)*math.Cos(secondCity.Latitude)*havLongitude
}

func archaversine(havAngle float64) float64 {
	var sqrtHavAngle float64 = math.Sqrt(havAngle)
	return 2.0 * math.Asin(sqrtHavAngle)
}

func calculatHaversineDistance(firstCity Location, secondCity Location) float64 {
	var havCentralAngle float64 = haversineFormula(firstCity, secondCity)
	var centralAngleRad float64 = archaversine(havCentralAngle)
	return earthRadiusKM * centralAngleRad
}

func main() {
	var lat float64
	var long float64
	fmt.Scanf("%f %f", &lat, &long)
	var firstCity = NewLocation(lat, long)

	fmt.Scanf("%f %f", &lat, &long)
	var secondCity = NewLocation(lat, long)

	fmt.Printf("distance between 2 cities = %f km\n", calculatHaversineDistance(firstCity, secondCity))
}
```

Now, how to run this program? Simple. First, save this code as `main.go`. Then go to this file directory, and run using this command in terminal: `go run main.go`. Then, input in terminal with this format:
```
{lat (in deg) of 1st city} {long (in deg) of 1st city}
{lat (in deg) of 2nd city} {long (in deg) of 2nd city}
```

For example, if I want to measure distance (in kilometers) between Soekarno-Hatta International Airport and Chang'i International Airport, then the input will be like this, more or less:
```
-6.120379 106.660107
1.3644 103.9915
```

When I press enter, and the program will give output like this:
```
distance between 2 cities = 883.429186 km
```

Compare the result with this distance calculator from MovableType. You will be amazed with the result. But, don't just be limited with these two locations. Try other locations as well.

# Conclusion

I hope by now, you understand how amazing (and complex) the mathematics of calculating distance between 2 cities. Where's the magic? The magic is that by using trigonometry, people before digital (and internet) age can calculate distance between 2 cities without launching any satellite.

See you next time!
