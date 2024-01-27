# Eva-Voice assistant
 Meet Eva, your ultimate voice assistant! From navigating maps, fetching Wikipedia info, and tackling complex calculations to playing your favorite songs and YouTube videos, Eva does it all with ease. Stay effortlessly connected to your digital world, making tasks a breeze through simple voice commands.

import speech_recognition as sr
import calendar
import datetime
import subprocess
import warnings
import webbrowser
import wolframalpha
import pyjokes
import pyttsx3
import pywhatkit
import requests
import wikipedia

import json
import subprocess





warnings.filterwarnings("ignore")
listener = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)


def talk(text):
    engine.say(text)
    engine.runAndWait()


def take_command():
    try:
        with sr.Microphone() as source:
            print('listening...')
            listener.adjust_for_ambient_noise(source,duration=1)
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            if 'eva' in command:
                command = command.replace('eva', '')
                print(command)
    except:
        pass
    return command

def today_date():
    now = datetime.datetime.now()
    date_now = datetime.datetime.today()
    week_now = calendar.day_name[date_now.weekday()]
    month_now = now.month
    day_now = now.day

    months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
    ]

    ordinals = [
        "1st",
        "2nd",
        "3rd",
        "4th",
        "5th",
        "6th",
        "7th",
        "8th",
        "9th",
        "10th",
        "11th",
        "12th",
        "13th",
        "14th",
        "15th",
        "16th",
        "17th",
        "18th",
        "19th",
        "20th",
        "21st",
        "22nd",
        "23rd",
        "24th",
        "25th",
        "26th",
        "27th",
        "28th",
        "29th",
        "30th",
        "31st",
    ]

    return "Today is " + week_now + ", " + months[month_now - 1] + " the " + ordinals[day_now - 1] + "."

def note(text):
    date = datetime.datetime.now()
    file_name = str(date).replace(":", "-") + "-note.txt"
    with open(file_name, "w") as f:
        f.write(text)

    subprocess.Popen(["notepad.exe", file_name])


def run_eva():
    command = take_command()
    print(command)
    if 'play' in command:
        song = command.replace('play', '')
        print('playing ' + song)
        talk('playing ' + song)
        pywhatkit.playonyt(song)
    elif 'time' in command:
        time = datetime.datetime.now().strftime('%I:%M %p')
        print(time)
        talk('Current time is ' + time)
    if "date" in command or "day" in command or "month" in command:
        get_today = today_date()
        print(get_today)
        talk(get_today)
    elif 'wikipedia' in command:
        talk('Searching Wikipedia...')
        statement = statement.replace("wikipedia", "")
        results = wikipedia.summary(statement, sentences=3)
        talk("According to Wikipedia")
        print(results)
        talk(results)

    elif 'info' in command:
        person = command.replace('info', '')
        inform = wikipedia.summary(person,2)
        print(inform)
        talk(inform)
    elif 'where is' in command:
        ind = command.lower().split().index("is")
        location = command.split()[ind + 1:]
        url = "https://www.google.com/maps/place/" + "".join(location)
        loc = "This is where " + str(location) + " is."
        talk(loc)
        webbrowser.open(url)
    elif 'are you single' in command:
        talk('I am in a relationship with wifi')
    elif 'joke' in command:
        talk(pyjokes.get_joke())

    elif "who are you" in command or "define yourself" in command:
        yourself='Hello, I am an Eva. Your Assistant. I am here to make your life easier. You can command me to perform various tasks such as asking questions or get updates etcetera'
        print(yourself)
        talk(yourself)

    elif "your name" in command:
        name="My name is eva"
        print(name)
        talk(name)

    elif "who am i" in command or "Who am I" in command:
        iam="You must probably be a human"
        print(iam)
        talk(iam)

    elif "why do you exist" in command or "why did you come to this word" in command:
        exist="I was created to assist you"
        print(exist)
        talk(exist)

    elif "how are you" in command:
        print("I am awesome, Thank you")
        talk("I am awesome, Thank you")
        print("\nHow are you?")
        talk("\nHow are you?")

    elif "fine" in command or "good" in command:
        print("It's good to know that your fine")
        talk("It's good to know that your fine")
    elif "search" in command.lower():
        ind = command.lower().split().index("search")
        search = command.split()[ind + 1:]
        webbrowser.open(
            "https://www.google.com/search?q=" + "+".join(search))

        print("Searching " + str(search)+ " on google")
        talk("Searching " + str(search)+ " on google")

    elif "google" in command.lower():
        ind = command.lower().split().index("google")
        search = command.split()[ind + 1:]
        webbrowser.open(
            "https://www.google.com/search?q=" + "+".join(search))
        print("Searching " + str(search) + " on google")
        talk("Searching " + str(search) + " on google")
    elif "talk to you later" in command or "see you later" in command or "see you later" in command:
        talk('good bye')
        exit()

    elif "make a note" in command:
        print("What would you like me to write down?")
        talk("What would you like me to write down?")
        note_text = take_command()
        note(note_text)
        print(("I have made a note of that."))
        talk("I have made a note of that.")
    elif "what is the weather in" in command:
        key = "d75533e1568523b84f985c1412a9562a"
        weather_url = "https://api.openweathermap.org/data/2.5/weather?"
        ind = command.split().index("in")
        location = command.split()[ind + 1:]
        location = "".join(location)
        url = weather_url + "appid=" + key + "&q=" + location
        js = requests.get(url).json()
        if js["cod"] != "404":
            weather = js["main"]
            temperature = weather["temp"]
            temperature = round((temperature) - 273.15)
            humidity = weather["humidity"]
            desc = js["weather"][0]["description"]
            weatherResponse=" The temperature in Celcius is " + str(temperature) + " The humidity is " + str(
                humidity) + " and The weather description is " + str(desc)
            print(weatherResponse)
            talk(weatherResponse)
        else:
            print('City Not Found')
            talk('City Not Found')
    elif 'news' in command:
        url = ('https://newsapi.org/v2/everything?'
               'q=Apple&'
               'from=2023-02-26&'
               'sortBy=popularity&'
               'apiKey=5bded03dcc3a408b99fc1327c28a99b0')

        try:
            response = requests.get(url)
        except:
            talk("Please check your connection")

        news = json.loads(response.text)

        for new in news["articles"]:
            print(str(new["title"]), "\n")
            talk(str(new["title"]))
            engine.runAndWait()

            print(str(new["description"]), "\n")
            talk(str(new["description"]))
            engine.runAndWait()
            time.sleep(2)
    elif "calculate" in command or 'send a message' in command:
        app_id = "JLYLAG-XWQVPT7EQQ"
        client = wolframalpha.Client(app_id)
        ind = command.lower().split().index("calculate")
        text = command.split()[ind + 1:]
        res = client.query(" ".join(text))
        answer = next(res.results).text
        print(answer)
        talk(answer)

    elif "what is" in command or "who is" in command:
        app_id = "JLYLAG-XWQVPT7EQQ"
        client = wolframalpha.Client(app_id)
        ind = command.lower().split().index("is")
        text = command.split()[ind + 1:]
        res = client.query(" ".join(text))
        answer= next(res.results).text
        print(answer)
        talk(answer)


    else:
        talk(' ')


while True:
    run_eva()
