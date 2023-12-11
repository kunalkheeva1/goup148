# StudyZoom (VOIP Application)
Group Study Video calling application 

# Table of Contents

1. [Starting the app in Docker](#starting-the-app-in-docker)
2. [How to use it](#how-to-use-it)
   - [Set Up for Unix, MacOS](#set-up-for-unix-macos)
   - [Set Up for Windows](#set-up-for-windows)
3. [Create Study Room](#create-study-room)
4. [Video Calling API](#video-calling-api)

<br />

## Starting the app in Docker<a name="starting-the-app-in-docker"></a>

To start the StudyZoom app in Docker, follow these steps:

**Step 1 - Download the code from the GitHub repository (using `git`)**

```bash
$ # Get the code
$ git clone https://github.com/jagz97/Zoom_148.git
$ cd <YOUR_BUILD_ID>
```

**Step 2 - Edit `.env` file to activate SQLite persistence**

Open the `.env` file and remove or comment out all the `DB_*` settings to activate the `SQLite` persistence. The modified `.env` file should look like this:

```txt
DEBUG=True

# Deployment SERVER address
SERVER=.zoomstudy.us

# For MySql Persistence
# DB_ENGINE=mysql            <-- REMOVE or comment for Docker
# DB_NAME=appseed_db         <-- REMOVE or comment for Docker  
# DB_HOST=localhost          <-- REMOVE or comment for Docker 
# DB_PORT=3306               <-- REMOVE or comment for Docker
# DB_USERNAME=appseed_db_usr <-- REMOVE or comment for Docker
# DB_PASS=<STRONG_PASS>      <-- REMOVE or comment for Docker

```

**Step 3 - Start the app in Docker**

```bash
$ docker-compose up --build 
```

After running this command, the StudyZoom app will start in Docker. You can access it by visiting `http://localhost:5085` in your browser.

<br />

## How to use it<a name="how-to-use-it"></a>

To use the StudyZoom app, follow the instructions below:

**Step 1 - Download the code**

```bash
$ # Get the code
$ git clone https://github.com/jagz97/Zoom_148.git
$ cd Zoom_148
```

### Set Up for Unix, MacOS<a name="set-up-for-unix-macos"></a>

**Step 2 - Install modules via `venv`**

```bash
$ virtualenv env
$ source env/bin/activate
$ pip3 install -r requirements.txt
```

**Step 3 - Set Up the Database**

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

**Step 4 - Start the app**

```bash
$ python manage.py runserver
```

After running this command, the app will start running at `http://127.0.0.1:8000/`.

### Set Up for Windows<a name="set-up-for-windows"></a>

**Step 2 - Install modules via `venv` (Windows)**

```bash
$ virtualenv env
$ .\env\Scripts\activate
$ pip3 install -r requirements.txt
```

**Step 3 - Set Up the Database**

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

**Step 4 - Start the app**

```bash
$ python manage.py runserver
```

After running this command, the app runs at `http://127.0.0.1:8000/`.

##  Create Study Room

By default, the app land on the create Study Group Page: 
<img width="740" alt="image" src="https://github.com/jagz97/Zoom_148/assets/68725607/10b1c140-672b-463e-8ae7-5956e6dc7d89">


- Create the Room Name for study group and share it with others for them to join
  **Note If you are joining a study group created by another user, enter that room name in this feild
- Enter your name to be displayed as your username
- Press join stream button and it will take you to the video chat room where you collaborate with your friends, classmates or group to study.


<br />

##  Video Calling API

The app uses Agora API for real-time video calling that provides VOIP services for real time video calling which uses RTN protocol.

The app uses simple workflow where the javascript client creates a token and the Dnajno server makes the token request and send it to the Aroga API for verificaiton and once the secure connection is established it lets users connect to the video calling services in real time.

<img width="950" alt="image" src="https://github.com/jagz97/Zoom_148/assets/68725607/05ce8936-39df-4611-9ede-fb8f3cda72e4">

The example below provides how the server creates a token for javascript client, sends json respone to aroga API.
(updated)
```py
def getToken(request):
    appId = '123e5eb9bd184d4187091c6b3a45cf89'
    appCertificate = '8a7a9fc2e8ab45a0bf3a5daf28439a53'
    channelName = request.GET.get('channel')
    uid = random.randint(1, 230)
    expirationTimeInSeconds = 3600*24
    currentTimeStamp = time.time()
    privilegeExpiredTs = currentTimeStamp + expirationTimeInSeconds
    role = 1

    token = RtcTokenBuilder.buildTokenWithUid(
        appId, appCertificate, channelName, uid, role, privilegeExpiredTs)
    return JsonResponse({'token': token, 'uid': uid}, safe=False)
```

<br />
