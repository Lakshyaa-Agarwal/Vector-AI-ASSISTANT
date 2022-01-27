'''
Before using this one must ensure that he has a good quality mic.
one can also modify the code as per the needs, but this the way we made it.
This is an extremely primitive version of AI Assistant which includes some of the very popular apps/urls to perform operations at.
'''

import datetime #Python Datetime module supplies classes to work with date and time. These classes provide a number of functions to deal with dates, times and time intervals.
                #Python Datetime module comes built into Python, so there is no need to install it externally. 

import mysql.connector as msc #MySQL Connector/Python enables Python programs to access MySQL databases, using an API that is compliant with the Python Database API Specification.
                              #It is written in pure Python and does not have any dependencies except for the Python Standard Library.

import speech_recognition as sr #Speech recognition is an important feature used in house automation and in artificial intelligence devices.
                                #The main function of this library is it tries to understand whatever the humans speak and converts the speech to text.

import pyttsx3 #It is a text to speech conversion library in python. This package supports text to speech engines on Mac os x, Windows and on Linux.

import wikipedia #Python provides the Wikipedia module (or API) to scrap the data from the Wikipedia pages. This module allows us to get and parse the information from Wikipedia.

import webbrowser #This is an in-built package in python. It extracts data from the web.

import os #This module is a standard library in python and it provides the function to interact with operating system.

import time #Python has defined a module, “time” which allows us to handle various operations regarding time, its conversions and representations.

import subprocess #This is a standard library use to process various system commands like to log off or to restart your PC.

import pyjokes #Pyjokes is a python library that is used to create one-line jokes for programmers. Informally, it can also be referred as a fun python library which is pretty simple to use.

import wolframalpha #Wolfram Alpha is an API which can compute expert-level answers using Wolfram’s algorithms, knowledge base and AI technology.
                    #It is made possible by the Wolfram Language.

import pyaudio #PyAudio provides Python bindings for PortAudio, the cross-platform audio I/O library.
               #With PyAudio, you can easily use Python to play and record audio on a variety of platforms, such as GNU/Linux, Microsoft Windows, and Apple Mac OS X / macOS.

import requests #The request module is used to send all types of HTTP request. Its accepts URL as parameters and gives access to the given URL’S.

import pywhatkit #pywhatkit is a Python library for sending WhatsApp messages at a certain time, play a YouTube video perform a Google search, Get information on particular topic. 

import smtplib #Simple Mail Transfer Protocol (SMTP) is a protocol, which handles sending e-mail and routing e-mail between mail servers.
               #Python provides smtplib module, which defines an SMTP client session object that can be used to send mail to any Internet machine with an SMTP.



print('Loading your personal AI assistant - VECTOR')

engine=pyttsx3.init('sapi5')    #pyttsx3 is a text-to-speech conversion library in Python. Unlike alternative libraries, it works offline and is compatible with both Python 2 and 3.
                                #An application invokes the pyttsx3.init() factory function to get a reference to a pyttsx3.
                                #The pyttsx3 module supports two voices first is female and the second is male which is provided by “sapi5” for windows.

voices=engine.getProperty('voices')     #Gets the current value of an engine property.
engine.setProperty('voice', voices[0].id)   #Queues a command to set an engine property.

    
def speak(text): 
    engine.say(text)    #Queues a command to speak an utterance. 
    engine.runAndWait() #Blocks while processing all currently queued commands.

def wishMe():
    hour=datetime.datetime.now().hour
    if hour>=0 and hour<12:
        speak("Hello,Good Morning sir")
        print("Hello,Good Morning sir")
    elif hour>=12 and hour<18:
        speak("Hello,Good Afternoon sir")
        print("Hello,Good Afternoon sir")
    else:
        speak("Hello,Good Evening sir")
        print("Hello,Good Evening sir")

def takeCommand():      #It takes microphone command from user and return string output.
    r=sr.Recognizer()   #Tries to recognize speech even when user is not speaking. 
    with sr.Microphone() as source: #Use the default microphone as the audio source.
        print("Hearing...")
        r.pause_threshold=2 #Seconds of non speaking audio before a phrase is considered complete
        r.energy_threshold=400 #To reduce the effect of background noise(300-3500 are acceptable)
        audio=r.listen(source)

    try:
        print("Analyzing...") 
        statement=r.recognize_google(audio,language='en-in')    #Performs speech recognition on audio data     
        print(f"{statement}\n")     #The f means Formatted string literals and it's new in Python 3.6.
                                    #A formatted string literal or f-string is a string literal that is prefixed with 'f' or 'F'.
                                    #These strings may contain replacement fields, which are expressions delimited by curly braces{}.
        return statement
    except Exception as e:
        speak("Could you repeat please...")
        return "None"
    

def sendEmail(to,content):
    server=smtplib.SMTP("smtp.gmail.com",587)
    server.ehlo()
    server.starttls()   #An email protocol command that tells an email server that an email client,
                        #including an email client running in a web browser, wants to turn an existing insecure connection into a secure one.
    server.login("abcd@gmail.com","1234567890")#Enter your email-id and password
    server.sendmail("abcd@gmail.com",to,content)#Enter your mail-id
    server.close()

def options():
    speak('Press 1 to add records')
    print('Press 1 to add records')
    speak('Press 2 to modify records')
    print('Press 2 to modify records')
    speak('Press 3 to delete records')
    print('Press 3 to delete records')

def url_stor():
    options()
    cho= int(input('Please enter your choice: '))
    speak('Please enter your choice')
    
    if cho==1:
        speak('Enter name of site')
        SiteName= input('Enter name of site: ')
        speak('Enter url of site')
        SiteUrl= input('Enter url of site: ')
        query="insert into URL(SiteName,SiteUrl) values('{}','{}')".format(SiteName,SiteUrl)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Data Saved")
        speak('Data saved')
        
    if cho==2:        
        speak('Enter site name to be modified')
        sname= input('Enter site name to be modified: ')
        query="select*from URL where SiteName='{}'".format(sname)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
            print('Invalid site name')
            speak('Invalid site name')
        else:
            SiteName=record[1]
            SiteUrl=record[2]
            print('Current Details Are:')
            print('Site Name: ',record[1])
            print('Site URL: ',record[2])
            
        speak('Would you like to change Site URL? press y or n')
        my_choice=input("Would you like to change Site URL <y/n>: ")
        if my_choice=='y':
            speak('Enter new Site URL')
            SiteUrl=input('Enter new Site URL: ')
        query="update URL set SiteUrl='{}'where SiteName='{}'".format(SiteUrl,SiteName)
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Record updated")
        speak("Record updated")
        
        
    if cho==3:
        speak("Enter site record to be deleted")
        sname=input("Enter site record to be deleted: ")
        query="select * from URL where SiteName='{}'".format(sname)        
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
                print("Invalid Site Name")
                speak("Invalid Site Name")
        else:
                print("Current Details Are:")
                speak("Current Details Are:")
                print("Site Name: ",record[1])
                print("Site URL: ",record[2])                
                
        speak('would you like to delete any records? press y or n')
        my_choice=input("Would you like to delete any records?<y/n> ")
        if my_choice=="y":
            query="delete from URL where SiteName='{}'".format(sname)
            cursor.execute(query)
            conn.commit()
            conn.close()
            print("Record deleted")
            speak("Record deleted")
            
       
        
    
def loc_stor():
    options()
    speak('Enter choice')
    cho= int(input('Enter choice: '))
    
    if cho==1:
        speak('Enter name of the app')
        app_name= input('Enter name of the app: ')
        speak('Enter location of app')
        app_loc=input('Enter location of app: ')
        query="insert into locations(app_name,app_loc) values('{}','{}')".format(app_name,app_loc)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Data Saved")
        speak("Data Saved")
        
    if cho==2:        
        speak('Enter app name to be modified')
        aname= input('Enter app name to be modified: ')
        query="select*from locations where app_name='{}'".format(aname)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
            print('Invalid app name')
            speak('Invalid app name')
        else:
            app_name=record[1]
            app_loc=record[2]
            print('Current Details Are:')
            speak('Current Details Are:')
            print('App Name: ',record[1])
            print('App Location: ',record[2])
            
        speak("Would you like to change App location press y or n")
        my_choice=input("Would you like to change App location <y/n>: ")
        if my_choice=='y':
            speak('Enter new location')
            app_loc=input('Enter new location: ')
        query="update locations set app_loc='{}'where app_name='{}'".format(app_loc,app_name)
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Record updated")
        speak("Record updated")
        
        
    if cho==3:
        speak("Enter app name to be deleted")
        aname=input("Enter app name to be deleted: ")
        query="select * from locations where app_name='{}'".format(aname)        
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
                print("Invalid App Name")
                speak("Invalid App Name")
        else:
                print("Current Details Are:")
                speak("Current Details Are:")
                print("App Name: ",record[1])
                print("App Locaion: ",record[2])
                
        speak("Would you like to delete the record press y or n")
        my_choice=input("Would you like to delete the record <y/n>: ")
        if my_choice=="y":
            query="delete from locations where app_name='{}'".format(aname)
            cursor.execute(query)
            conn.commit()
            conn.close()
            print("Record deleted")
            speak("Record deleted")
            
       


def id_stor():
    options()
    speak('Enter choice')
    cho= int(input('Enter choice: '))
    
    if cho==1:
        speak('Enter name of app or site')
        name= input('Enter name of app/site: ')
        speak('Enter your id in that app or site')
        i_d= input('Enter your id in that app/site: ')
        speak('Enter your password in that app or site')
        password= input('Enter your password in that app/site: ')
        speak('Enter api code')
        api_code= input('Enter api code: ')
        query="insert into id(name,id,pwd,api_code) values('{}','{}','{}','{}')".format(name,i_d,password,api_code)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Data Saved")
        speak('Data saved')
        
    if cho==2:
        speak('Enter name to be modified')
        name= input('Enter name to be modified: ')
        query="select*from id where name='{}'".format(name)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
            print('Invalid name')
            speak('Invalid name')
        else:
            name=record[1]
            i_d=record[2]
            password=record[3]
            api_code=record[4]
            print('Current Details Are:')
            speak('Current Details Are:')
            print('Name: ',record[1])
            print('ID: ',record[2])
            print('Password: ',record[3])
            print('API Code: ',record[4])
            
        speak("Would you like to change ID press y or n")
        my_choice=input("Would you like to change ID <y/n>: ")
        if my_choice=='y':
            speak('Enter new ID')
            i_d=input('Enter new ID: ')            
        speak("Would you like to change password press y or n")
        my_choice=input("Would you like to change password <y/n>: ")
        if my_choice=='y':
            speak('Enter new password')
            pwd=input('Enter new password: ')
        speak("Would you like to change API Code press y or n")
        my_choice=input("Would you like to change API Code <y/n>: ")
        if my_choice=='y':
            speak('Enter new API Code')
            api_code=input('Enter new API Code: ')
        query="update id set ='{}'where id='{}',pwd='{}', api_code='{}')".format(i_d,pwd,api_code)
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Record updated")
        speak("Record updated")
        
    if cho==3:
        speak("Enter name to be deleted")
        name=input("Enter name to be deleted: ")
        query="select * from id where id='{}'".format(name)        
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
                print("Invalid Name")
                speak("Invalid Name")
        else:
            print("Current Details Are:")
            speak("Current Details Are:")
            print("Name: ",record[1])
            print('ID: ',record[2])
            print('Password: ',record[3])
            print('API Code: ',record[4])
            
                
        speak("Would you like to delete the record press y or n")
        my_choice=input("Would you like to delete the record <y/n>: ")
        if my_choice=="y":
            query="delete from id where id='{}'".format(name)
            cursor.execute(query)
            conn.commit()
            conn.close()
            print("Record deleted")
            speak("Record deleted")
            
       


def gc_stor():
    options()
    speak('Enter choice')
    cho= int(input('Enter choice: '))
    
    if cho==1:
        speak('Enter name of that person or company')
        name= input('Enter name of that person/company: ')
        speak('Enter e-mail id of that person or company')
        i_d= input('Enter e-mail id of that person/company: ')
        query="insert into gmail_con(name,id) values('{}','{}')".format(name,i_d)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Data Saved")
        speak("Data Saved")
        
    if cho==2:
        speak('Enter username to be modified')
        name= input('Enter username to be modified: ')
        query="select*from gmail_con where name='{}'".format(name)
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
            print('Invalid username')
            speak('Invalid username')
        else:
            name=record[1]
            i_d=record[2]
            print('Current Details Are:')
            speak('Current Details Are:')
            print('User Name: ',record[1])
            print('ID: ',record[2])
            
        speak("Would you like to change ID press y or n")
        my_choice=input("Would you like to change ID <y/n>: ")
        if my_choice=='y':
            speak('Enter ID')
            i_d=input('Enter ID: ')
        query="update gmail_con set id='{}'where name='{}'".format(i_d,name)
        cursor.execute(query)
        conn.commit()
        conn.close()
        print("Record updated")
        speak("Record updated")
        
        
    if cho==3:
        speak("Enter name to be deleted")
        name=input("Enter name to be deleted: ")
        query="select * from gmail_con where id='{}'".format(name)        
        conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
        cursor=conn.cursor()
        cursor.execute(query)
        record=cursor.fetchone()
        if record==None:
                print("Invalid Name")
                speak("Invalid Name")
        else:
            print("Current Details Are:")
            speak("Current Details Are:")
            print('User Name: ',record[1])
            print('ID: ',record[2])
            
        speak("Would you like to delete the record press y or n")
        my_choice=input("Would you like to delete the record <y/n>: ")
        if my_choice=="y":
            query="delete from gmail_con where name='{}'".format(name)
            cursor.execute(query)
            conn.commit()
            conn.close()
            print("Record deleted")
            speak("Record deleted")
            
def retrieve_url(entry):
    query="select SiteUrl from URL where SiteName='{}'".format(entry)
    conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
    cursor=conn.cursor()
    cursor.execute(query)
    record=cursor.fetchone()
    if record==None:
        print("Invalid Entry")
        speak("Invalid Entry")
    else:
        results=record[0]
        return(results)
    
def retrieve_loc(entry):
    query="select App_loc from locations where App_name='{}'".format(entry)
    conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
    cursor=conn.cursor()
    cursor.execute(query)
    record=cursor.fetchone()
    if record==None:
        print("Invalid Entry")
        speak("Invalid Entry")
    else:
        results=record[0]
        return(results)
    

def retrieve_id(entry):
    query="select id,pwd,api_code from id where name='{}'".format(entry)
    conn=msc.connect(host="localhost",user="root",password="india@123",database="Vector")
    cursor=conn.cursor()
    cursor.execute(query)
    record=cursor.fetchone()
    if record==None:
        print("Invalid Entry")
        speak("Invalid Entry")
    else:
        results_1=record[0]
        results_2=record[1]
        results_3=record[2]
        return[results_1,results_2,results_3]
    

def retrieve_gm_con(entry):
    query="select id from gmail_con where name='{}'".format(entry)
    conn=msc.connect(host="localhost",user="root",password="india@123" database="Vector")
    cursor=conn.cursor()
    cursor.execute(query)
    record=cursor.fetchone()
    if record==None:
        print("Invalid Entry")
        speak("Invalid Entry")
    else:
        results=record[0]
        return(results)

def choice(ch):
    if ch==1:
        url_stor()
                
    if ch==2:
        loc_stor()
                
    if ch==3:
        id_stor()
                
    if ch==4:
        gc_stor()
                

    

    
speak("Loading your personal AI Assistant Vector")
wishMe()


if __name__=='__main__':

    while True:
        speak("Tell me sir, how can I help you")
        statement= takeCommand().lower()
        if statement==0:
            continue

        if 'hello vector' in statement:
            speak('Hi Sir! Nice meeting you!!')

        elif 'who are you' in statement or 'what can you do' in statement:
            speak("I'm Vector - your persoanl AI assistant. I'm programmed to perform various tasks. What do you want me to help you with?")

        elif "who made you" in statement or "who created you" in statement or "who discovered you" in statement:
            speak("I was designed by Laksh")
            print("I was designed by Lakshya")

        elif 'time' in statement:
            strTime=datetime.datetime.now().strftime("%H:%M:%S")
            print(strTime)
            speak(f"The current time is{strTime}")
       
        
        elif 'wikipedia' in statement:
            speak('Searching Wikipedia...')
            print('Searching ---> Wikipedia')
            statement =statement.replace("wikipedia", "")
            results = wikipedia.summary(statement, sentences=3)
            speak("Wikipedia says...")
            print(results)
            speak(results)

        elif 'play' in statement:
            song = statement.replace('play', '')
            speak('playing ' + song)
            pywhatkit.playonyt(song)
               
        elif 'joke' in statement:
            print('Fetching---> Jokes')
            x=pyjokes.get_joke()
            print(x)
            speak(x)
            
        elif 'search'  in statement:
            speak('Searching!!!')
            statement = statement.replace("search", "")
            webbrowser.open_new_tab(statement)
            time.sleep(3)        
        
        elif 'open music app' in statement:            
            ma= retrieve_loc('music app') 
            speak('music app - open now!')
            print('Opening---> Music App')
            os.startfile(ma)
        
        elif 'open screen recorder' in statement:
            scp= retrieve_loc('screen recorder')
            os.startfile(scp) 
        
        elif 'open telegram' in statement:
            speak('telegram - open now!')
            print('Opening---> Telegram')
            tp= retrieve_loc('telegram')
            os.startfile(tp)

        elif 'open whatsapp' in statement:
            speak('whatsapp - open now!')
            print('Opening---> WhatsApp')
            wp= retrieve_loc('whatsapp')
            os.startfile(wp)

        elif 'open facebook' in statement:
            speak('telegram - open now!')
            print('Opening---> Facebook')
            fp= retrieve_loc('facebook')
            os.startfile(fp) 
        
        elif 'open snipping tool' in statement:
            speak('snipping tool - open now!')
            print('Opening---> Snipping Tool')
            st= retrieve_loc('snipping tool')
            os.startfile(st)

        elif 'r studio' in statement:
            speak('r studio - open now!')
            print('Opening---> R Studio')
            rs= retrieve_loc('r studio')
            os.startfile(rs)       
       

        elif 'open youtube' in statement:
            yt= retrieve_url('youtube')
            speak("youtube - open now!")
            print('Opening---> YouTube')
            webbrowser.open_new_tab(yt)
            time.sleep(3)

        elif 'open google' in statement:
            go= retrieve_url('google')
            speak("Google chrome - open now!")
            print('Opening---> Google Chrome')
            webbrowser.open_new_tab(go)
            time.sleep(3)

        elif 'open gmail' in statement:
            gm= retrieve_url('gmail')
            speak("Google Mail - open now!")
            print('Opening---> Gmail')
            webbrowser.open_new_tab(gm)
            time.sleep(3)

        elif "open stackoverflow" in statement:
            sof= retrieve_url('stackoverflow')
            speak("Here is your stackoverflow")
            print('Opening---> Stackoverflow')
            webbrowser.open_new_tab(sof)
            time.sleep(3)

        elif 'news' in statement:
            nws= retrieve_url('news')
            speak('Here are some headlines from your favourite newspaper,happy reading')
            print('Opening---> Newspaper')
            news = webbrowser.open_new_tab("")
            time.sleep(3)        
        
        elif 'open amazon' in statement:
            az= retrieve_url('amazon')
            speak('Amazon - open now !')
            print('Opening---> Amazon')
            webbrowser.open_new_tab(az)
            time.sleep(3)

        elif 'open instagram' in statement:
            it= retrieve_url('instagram')
            speak('Instagram - open now !')
            print('Opening---> Instagram')
            webbrowser.open_new_tab(it)
            time.sleep(3)

        elif 'send an email' in statement:
            try:
                speak("Whom do you want to mail sir?")
                to= takeCommand()
                speak("what do you want to mail sir?")
                content=takeCommand()
                sendEmail(to,content)
                speak("Email sent!")
            except Exception as e:
                print(e)
                speak("Sorry...there was some issue, email can't be sent")
                
        elif 'ask' in statement:
            speak('I can answer to computational and geographical questions. What questions do you want me to answer sir? ')
            question=takeCommand()
            api_id=retrieve_id('wolframalpha') #get api id from site of wolframalpha 
            client = wolframalpha.Client(api_id)
            res=client.query(question)
            answer=next(res.results).text
            speak(answer)
            print(answer)
        
        elif "give input" in statement:
            speak('directing you to the same...')
            speak("You can store url of various popular sites that you might want to use frequently, Press 1")
            print("You can store url of various popular sites that you might want to use frequently---> Press 1")
            speak("You can store location of various apps pre-installed on your device, Press 2")
            print("You can store location of various apps pre-installed on your device---> Press 2")
            speak("You can store your important id(s), passwords and api-codes, Press 3")
            print("You can store your important id(s), passwords and api-codes---> Press 3")
            speak("You can store your gmail contacts, Press 4")
            print("You can store your gmail contacts---> Press 4")
            speak('Please enter choice')
            ch=int(input('Enter choice: '))
            choice(ch)
            speak('do you want to give more inputs? press y or n')
            print('do you want to give more inputs? press <y/n>')
            ask=(input('Enter choice: '))
            if ask=='y':
                speak('Please enter choice')
                ch=int(input('Enter choice: '))
                choice(ch)

            
        elif "quiet now" or "stop" in  statement:
            speak('Press Enter to unmute')
            input('Press Enter to unmute...')
            
                
        elif "log off" in statement or "sign out" in statement:
            speak("Ok , your pc will log off soon, make sure you exit from all applications")
            print('Shutting down PC soon!!! Make sure you exit from all applications...')
            subprocess.call(["shutdown", "/l"])

        elif "shut vector" or "shut up" in statement:
            speak('Your AI assistant Vector is shutting down, thankyou, Good bye')
            print('Your AI assistant Vector is shutting down, thankyou, Good bye')
            break
            
time.sleep(2)
