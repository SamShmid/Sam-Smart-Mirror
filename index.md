# Sam's Smart Mirror
This will serve as a brief description of your project. Limit this to three sentences because it can become overly long at that point. This copy should draw the user in and make she/him want to read more.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Samuel S. | SAR High School | Electrical Engineering | Rising Junior |

![Headstone Image](https://bluestampengineering.com/wp-content/uploads/2016/05/improve.jpg)
  
# Final Milestone
My final milestone is the increased reliability and accuracy of my robot. I ameliorated the sagging and fixed the reliability of the finger. As discussed in my second milestone, the arm sags because of weight. I put in a block of wood at the base to hold up the upper arm; this has reverberating positive effects throughout the arm. I also realized that the forearm was getting disconnected from the elbow servo’s horn because of the weight stress on the joint. Now, I make sure to constantly tighten the screws at that joint. 

# Video Goes Here

# Second Milestone
My final milestone is the increased reliability and accuracy of my robot. I ameliorated the sagging and fixed the reliability of the finger. As discussed in my second milestone, the arm sags because of weight. I put in a block of wood at the base to hold up the upper arm; this has reverberating positive effects throughout the arm. I also realized that the forearm was getting disconnected from the elbow servo’s horn because of the weight stress on the joint. Now, I make sure to constantly tighten the screws at that joint.

# Video Goes Here


# First Milestone
  

My first milestone was setting up my Raspberri Pi and the Magic Mirror2 software. I installed the Raspberry Pi OS on a sd card than inserted it into my Raspberry Pi. After setting up the software I enabled SSH and VNC in order to be able to connect to the Raspberry Pi with my computer. I installed the Magic Mirror2 software, but I intially ran into problems starting up the software until i realzied you have to change directories to do so. After getting that working I installed modules, and I was doing so I ran into problems in the config file because there were errors in my code. Than with the help of my instructor we went through the code and found a missing "}" to be the culprit. After this everything was smooth sailing and I was able to configure the software without issue.

<iframe width="560" height="315" src="https://www.youtube.com/embed/WaIqpAlDoYg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



``` javascript
/* Magic Mirror Config Sample
 *
 * By Michael Teeuw https://michaelteeuw.nl
 * MIT Licensed.
 *
 * For more information on how you can configure this file
 * see https://docs.magicmirror.builders/getting-started/configuration.html#general
 * and https://docs.magicmirror.builders/modules/configuration.html
 */
let config = {
	address: "localhost", 	// Address to listen on, can be:
							// - "localhost", "127.0.0.1", "::1" to listen on loopback interface
							// - another specific IPv4/6 to listen on a specific interface
							// - "0.0.0.0", "::" to listen on any interface
							// Default, when address config is left out or empty, is "localhost"
	port: 8080,
	basePath: "/", 	// The URL path where MagicMirror is hosted. If you are using a Reverse proxy
					// you must set the sub path here. basePath must end with a /
	ipWhitelist: ["127.0.0.1", "::ffff:127.0.0.1", "::1"], 	// Set [] to allow all IP addresses
															// or add a specific IPv4 of 192.168.1.5 :
															// ["127.0.0.1", "::ffff:127.0.0.1", "::1", "::ffff:192.168.1.5"],
															// or IPv4 range of 192.168.3.0 --> 192.168.3.15 use CIDR format :
															// ["127.0.0.1", "::ffff:127.0.0.1", "::1", "::ffff:192.168.3.0/28"],

	useHttps: false, 		// Support HTTPS or not, default "false" will use HTTP
	httpsPrivateKey: "", 	// HTTPS private key path, only require when useHttps is true
	httpsCertificate: "", 	// HTTPS Certificate path, only require when useHttps is true

	language: "en",
	locale: "en-US",
	logLevel: ["INFO", "LOG", "WARN", "ERROR"], // Add "DEBUG" for even more logging
	timeFormat: 12,
	units: "imperial",
	// serverOnly:  true/false/"local" ,
	// local for armv6l processors, default
	//   starts serveronly and then starts chrome browser
	// false, default for all NON-armv6l devices
	// true, force serveronly mode, because you want to.. no UI on this device

	modules: [
		{
			module: "alert",
		},
		{
			module: "updatenotification",
			position: "top_bar"
		},
		{
			module: "clock",
			position: "top_left"
		},
		{
			module: "weather",
			position: "top_right",
			header: "Weather Forecast",
			config: {
				weatherProvider: "openweathermap",
				type: "forecast",
				location: "New York",
				locationID: "5128581", //ID from http://bulk.openweathermap.org/sample/city.list.json.gz; unzip the gz file and find your city
				apiKey: "ENTER API KEY HERE"
			}
		},
		{
			module: "MMM-Jast",
			position: "top_bar",
			config: {
				maxWidth: "100%",
				updateIntervalInSeconds: 300,
				fadeSpeedInSeconds: 60,
				scroll: "horizontal",
				useGrouping: false,
				currencyStyle: "code",
				showColors: true,
				showCurrency: true,
				showChangePercent: true,
				showChangeValue: false,
				ShowChangeValueCurrency: false,
				showDepot: false,
				showDepotGrowthPercent: false,
				showDepotGrowth: false,
				numberDecimalsValues: 2,
				numberDecimalsPercentages: 1,
				virtualHorizontalMultiplier: 2,
				stocks: [
					{ name: "Bitcoin USD", symbol: "BTC-USD"},
					{ name: "Civic USD", symbol: "CVC-USD"},
					{ name: "ProShares UltraPro Short Dow 30", symbol: "SDOW"},
					{ name: "Rackspace Technology, Inc.", symbol: "RXT"},
					{ name: "Host Hotels & Resorts, Inc.", symbol: "HST"},
					]
			}
		},

		{
			module: "newsfeed",
			position: "bottom_left",
			config: {
				feeds: [
					{
						title: "New York Times",
						url: "https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml"
					}
					],
				showSourceTitle: true,
				showPublishDate: true,
				broadcastNewsFeeds: true,
				broadcastNewsUpdates: true
			}
		},

		{
	module: 'MMM-PGA',
			position: "top_left",
			maxWidth: "100%",
			config: {
				colored: true,
				showBoards: true,
				showLocation: true,
				showRankings: true,
				numRankings: 10,
				numTournaments: 3,
				numLeaderboard: 5,
				maxLeaderboard: 10,
				includeTies: true,
				showLogo: true,
				showFlags: true,
				remoteFavoritesFile: "https://dl.dropboxusercontent.com/s/7my######/favorites.json" //"utilities/favorites.json" 
			}
		},

{
			module: "MMM-MyStandings",
			position: "top_left",
			config: {
				updateInterval: 60 * 60 * 1000, // every 60 minutes
				rotateInterval: 1 * 60 * 1000, // every 1 minute
				sports: [
					{ league: "NBA", groups: ["Atlantic", "Central", "Southeast", "Northwest", "Pacific", "Southwest"] },
					{ league: "MLB", groups: ["American League East", "American League Central", "American League West", "National League East", "National League Central", "National League West"] },
					{ league: "NFL", groups: ["AFC East", "AFC North", "AFC South", "AFC West", "NFC East", "NFC North", "NFC South", "NFC West"] },
					{ league: "NHL", groups: ["Atlantic Division", "Metropolitan Division", "Central Division", "Pacific Division"] },
					{ league: "MLS", groups: ["Eastern Conference", "Western Conference"] },
					{ league: "NCAAF", groups: ["American Athletic - East", "American Athletic - West", "Atlantic Coast Conference - Atlantic", "Atlantic Coast Conference - Coastal",
										"Big 12 Conference", "Big Ten - East", "Big Ten - West", "Conference USA - East", "Conference USA - West",
										"FBS Independents", "Mid-American - East", "Mid-American - West", "Mountain West - Mountain", "Mountain West - West",
										"Pac 12 - North", "Pac 12 - South", "SEC - East", "SEC - West", "Sun Belt - East", "Sun Belt - West"] },
					{ league: "NCAAM", groups: ["America East Conference", "American Athletic Conference", "Atlantic 10 Conference", "Atlantic Coast Conference", "Atlantic Sun Conference",
										"Big 12 Conference", "Big East Conference", "Big Sky Conference", "Big South Conference",
										"Big Ten Conference", "Big West Conference", "Colonial Athletic Association", "Conference USA",
										"Horizon League", "Ivy League", "Metro Atlantic Athletic Conference", "Mid-American Conference",
										"Mid-Eastern Athletic Conference", "Missouri Valley Conference", "Mountain West Conference", "Northeast Conference",
										"Ohio Valley Conference", "Pac-12 Conference", "Patriot League", "Southeastern Conference",
										"Southern Conference", "Southland Conference", "Southwestern Athletic Conference", "Summit League",
										"Sun Belt Conference", "West Coast Conference", "Western Athletic Conference"] },
					],
				nameStyle: "abbreviation",
				showLogos: true,
				useLocalLogos: true,
				showByDivision: true,
				fadeSpeed: 1000,
			},
		},


		{
			module: "MMM-page-indicator",
			position: "bottom_left",
			config: {
				pages: 3,
				activeBright: true,
		   		inactiveHollow: true,
			}
		},

		{
			module: "MMM-pages",
			config: {
				animationTime: 500,
				rotationDelay: 6000,
				modules:
				    [["alert", "updatenotification", "clock", "weather", "MMM-Jast", "newsfeed" ],["MMM-PGA"],["MMM-MyStandings"]],
				fixed: ["MMM-page-indicator"],
				hiddenPages: {
					"Sports": [ "MMM-MyStandings" ],
				}

		},
		},
	],
};

/*************** DO NOT EDIT THE LINE BELOW ***************/
if (typeof module !== "undefined") {module.exports = config;}

```
This is the code I used for my config.js file on my Smart Mirror. This code tells the modules what to do and how to utilize the abilities of each module.

Modules Include
| **Module** | **Author** | **Link** | **Usage** |
|:--:|:--:|:--:|:--:|
| Alert | default | Included by default | displays notifications from other modules |
| Update Notification | default | Included by default | displays notification when a update for the Magic Mirror 2 software is available |
| Clock | default | Included by default | displays the time |
| Weather | default | Included by default | uses the openweather api to display local weather |
| Newsfeed | default | Included by default | rotates through the newest headlines of the New York Times |
| MMM-Jast | Made By jalibu | https://github.com/jalibu/MMM-Jast | a stock ticker that uses the Yahoo Finance API |
| MMM-PGA | Made By mcl8on | https://github.com/mcl8on/MMM-PGA | displays upcoming PGA tournaments and when the tournament is ongoing displays information about it |
| MMM-MyStandings | Made By vincep5 | https://github.com/vincep5/MMM-MyStandings | displays and gets ESPN standings for all major US sports |
| MMM-pages | Made By edward-shen | https://github.com/edward-shen/MMM-pages | allows you to have multiple pages of modules |
| MMM-page-dindicator | Made By edward-shen | https://github.com/edward-shen/MMM-page-indicator | allows you to switch through the pages and tells you what page you are on |
