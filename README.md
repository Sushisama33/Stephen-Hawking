# Stephen-Hawking
An artificial assistant to make oue daily life easy
import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os
import smtplib
import googlesearch as gogle
print("Initiating stratup.......")
print("startup complete")

# This is the initialization of the pyttsx3 module
engine = pyttsx3.init('sapi5')
sound = engine.getProperty('voices')
engine.setProperty('voices',sound[1].id)
engine.setProperty('rate',120)

# This function converts speech to text
def speak(audio):
    engine.say(audio)
    engine.runAndWait()

speak("Hello i am Stephen hawking ")

def Greetings():
    hour = int(datetime.datetime.now().hour)
    if hour>=6 and hour<=12:
        speak("Good morning ")
    elif hour>12 and hour<=15:
        speak("Good afternoon")
    elif hour>=1 and hour<=4:
        speak("what the frick you doing up so late")
    else:
        speak("Good evening")

    speak("How can i be of assistance")


def Command():
    h = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        h.pause_threshold = 1
        audio = h.listen(source)
    try:
        print("Recognizing..")
        query = h.recognize_google(audio, language="en-in")
        print('user said:'+ query)
    except Exception as e:
        #print(e)
        print("Please repeat what you said")
        return "None"
    return query

def website(text):
    webbrowser.open(text+".com")

def sendEmail(email,msg):
    server = smtplib.SMTP('smtp.gmail.com',587)
    server.ehlo()
    server.starttls()
    server.login("email",password="")
    server.sendmail("email",email,msg)
    server.close()





if __name__ == '__main__':

    Greetings()
    while True:
        query = Command().lower()
        if "wikipedia" in query:
            speak("Searching wikipedia")
            query = query.replace("wikipedia", "")
            search = wikipedia.summary(query, sentences=3)
            speak("According to wikipedia")
            speak(search)

        elif "open youtube" in query:
            webbrowser.open("youtube.com")

        elif "website" in query:
            speak("which website do you wanna open")
            webi = Command().lower()
            website(webi)

        elif "the time" in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak("the time is"+strTime)


        elif "email" in query:
            speak("who do you want to send email")
            email = raw_input("enter your email")
            speak("what  do you wanna say")
            msg = Command().lower()
            print(msg)
            sendEmail(email,msg)


        elif "terminate" in query:
            speak("happy to help you byeeee")
            break

