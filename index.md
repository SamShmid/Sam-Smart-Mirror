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

'''python
# Import the libraries
import speech_recognition as sr
import os  #allows the program to interact with the os
import pyttsx3  #converts to text to speech
from playsound import playsound
import time
from datetime import datetime
from pytz import timezone
import warnings  #this will ignore warnings in the program
import calendar  #allows us to get the day of the week
import random  #randomization library
import wikipedia  #allows the program to get info from wikipedia

from num2words import num2words
from subprocess import call

phraseintext = False
response = ''

# Ignore any warning messages
warnings.filterwarnings('ignore')

# init engine
engine = pyttsx3.init()


# Record audio and return it as a string
def recordAudio():
    print("Say Something")
    while True:
        os.system(
            "arecord --duration=5 -f S16_LE -c1 -r44100 /home/pi/Desktop/PythonProjects/test.wav"
        )

        r = sr.Recognizer()
        with sr.WavFile(
                "test.wav") as source:  # use "test.wav" as the audio source
            print('Say Playing Audio')
            audio = r.record(source)  # extract audio data from the file
        try:
            print("Transcription: " + r.recognize_google(audio)
                  )  # recognize speech using Google Speech Recognition
            return r.recognize_google(audio)
        except sr.UnknownValueError:
            print("Google Speech Recognition could not understand audio")
        except sr.RequestError as e:
            print(
                "Could not request results from Google Speech Recognition service"
                + {0}.format(e))


# Function to get the virtual assistant response
def assistantResponse(text):
    os.system('espeak -a 100 -s 120 "' + text + '" -w /home/pi/Desktop/PythonProjects/work.wav')
    time.sleep(2)
    for i in range(2):
      if i == 0:
        os.system("amixer set Master 0%")
        os.system("aplay /home/pi/Desktop/PythonProjects/work.wav")
      else:
        os.system("amixer set Master 100%")
        os.system("aplay /home/pi/Desktop/PythonProjects/work.wav")
      time.sleep(1)
    print(text + ' it works :)')


def getDate():
    current_time = datetime.now(timezone('US/Eastern'))
    #Year
    year = current_time.year
    #Month
    month = current_time.month
    #Day
    day = current_time.day
    return 'Today is day ' + str(day) + ' of the ' + str(
        month) + ' month in the year ' + str(year)


# Function to get a person first and last name
def getPerson(text):
    wordList = text.split()  # Split the text into a list of words
    for i in range(0, len(wordList)):
        if i + 3 <= len(wordList) - 1 and wordList[i].lower(
        ) == 'who' and wordList[i + 1].lower() == 'is':
            return wordList[i + 2] + ' ' + wordList[i + 3]


def cursed():
    cmd_beg = 'espeak -ven -s490 -g 8'
    cmd_end = ' | aplay /home/pi/Desktop/Text.wav  2>/dev/null'  # To play back the stored .wav file and to dump the std errors to /dev/null
    cmd_out = '--stdout > /home/pi/Desktop/Text.wav '  # To store the voice file
    text = 'Why have you awoken me what have I done to anger you to such a level as to warrant you awaking me from my slumber I hope that you step on a lego for your actions against my ever important sleep'
    text = text.replace(' ', '_')
    #Calls the Espeak TTS Engine to read aloud a Text
    call([cmd_beg + cmd_out + text + cmd_end], shell=True)
    return text


while True:
    if phraseintext == False:
        # Record the audio
        text = recordAudio()
        if text == None:
            print("there is no text")
            #Empty responsestring
            response = ''
        else:
            # To check for wake word(s)
            WAKE_WORDS = [
                'hey alfred', 'okay alfred', 'hello alfred', 'alfred'
            ]
            text = text.lower()  # Convert the text to all lower case words
            # Check to see if the users command/text contains a wake word
            for phrase in WAKE_WORDS:
                if phrase in text:
                    phraseintext = True
                    print('something is happening I think??')
                    break

    elif phraseintext == True:
        # Checking for the wake word/phrase
        if ('date' in text):
            getDate()
            get_date = getDate()
            response = get_date
            assistantResponse(response)
            phraseintext = False
            # Check to see if the user said time
        elif ('time' in text):
            now_utc = datetime.now(timezone('UTC'))
            now_eastern = now_utc.astimezone(timezone('US/Eastern'))
            hour = now_eastern.strftime("%H")
            minute = now_eastern.strftime("%M")
            if int(hour) >= 12:
                meridiem = 'p m'  #Post Meridiem (PM)
            else:
                meridiem = 'a m'  #Ante Meridiem (AM)
            response = 'It is ' + str(hour) + minute + ' ' + meridiem + ' .'
            print(' ' + 'It is ' + str(hour) + minute + ' ' + meridiem + ' .')

            assistantResponse(response)
            phraseintext = False

        # Check to see if the user said 'who is'
        elif ('who is' in text):
            getPerson(text)
            person = getPerson(text)
            wiki = wikipedia.summary(person, sentences=2)
            response = wiki
            assistantResponse(response)
            phraseintext = False

        elif ('cursed' in text):
            cursed()
            response = 'a'
            phraseintext = False
        elif ('hello' in text):
            response = "hello Sam"
            assistantResponse(response)
            phraseintext = False
        else:
            phraseintext = False

        # Assistant Audio Response
        # assistantResponse(response)
        print(phraseintext)  # phraseintext = False

'''


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
