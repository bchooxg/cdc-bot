# CDC-BOT

This is a bot for [ComfortDelGro Driving Centre](https://www.cdc.sg/) to help you chope those hard-to-get sessions and 
speedrun your way to your license!

Features:
  - Periodically fetch available sessions for Simulator, BTT, RTT, FTT, Practical Lesson, and Practical Test
  - Automatically solves any CAPTCHAs
  - Compare with booked dates for each available session
  - Notify user (via Telegram & E-mail) if earlier session is found
  - Attempt to reserve the session if possible

Full credit goes to [cdc-helper](https://github.com/mfjkri/cdc-helper) by [mfjkri](https://github.com/mfjkri) where I 
forked this repository from. I refactored the code, added the ability to book Simulator sessions, fixed some bugs, and 
then some.

---

## ⚠️ DISCLAIMER: USE AT YOUR OWN RISK! ⚠️
Recently, CDC has tried to clamp down on bots. If you are flagged for possible botting behaviour, they will disable the 
"Stored Value" of your account for 5 days, and you will only be able to log in to your account after that. I am not sure 
what they look at to determine if you are botting (e.g. number of requests per day, time between requests, CAPTCHAv3, 
etc.).

As I have already obtained my Class 3A license through the Standard team, I am unable to test and maintain the bot 
anymore as my account has been closed. I have tried to make it compatible with other licenses (e.g. Class 2B) and other 
teams (e.g. One Team, Elite Team), but there is no guarantee that it will work as I cannot test it.

However, feel free to fork the project and add more features! For instance,
  - Vary the time between requests to avoid bot detection(?)
  - Bot on the CDC app as CAPTCHA is currently not implemented there
  - Ability to book specific sessions when available instead of just reserving them
  - Support the booking of more types of sessions (e.g. Circuit Enhancement Practice, etc.)

---

# Prerequisites

## Python 3
If you do not already have Python 3, install it from the [official website](https://www.python.org/downloads).

# Setting Up

## Clone the repository
```bash
$ git clone https://github.com/Zhannyhong/cdc-bot.git
$ cd cdc-bot
```

## Install dependencies

### Create and activate virtual environment
On Linux/MacOS:
```bash
$ python3 -m venv venv
$ source venv/bin/activate
```

On Windows:
```bash
$ python3 -m venv venv
$ venv\Scripts\activate
```

### Install requirements
```bash
$ pip install -r requirements.txt
```

## Initialise configurations
Create `config.yaml` from the template and fill in the required fields.
```bash
$ cp config.yaml.example config.yaml
```

### 1) TwoCaptcha
This project uses a third party API that is unfortunately a **paid** service.  
As of writing, the rates of using this API are *relatively cheap* (SGD$5 can last you for about a month of the program 
runtime). To continue using this project, head over to [2captcha.com](https://2captcha.com/)

  - Create an account
  - Top up your account with sufficient credits
  - Copy your API Token and paste it into `config.yaml`

### 2) Notifications

#### Email
If you wish to enable Email notifications:
  1. Set `email_notification_enabled` to `True`
  2. Set `smtp_server` and `smtp_port` accordingly (Default values are for gmail)
  3. Set `smtp_user` to your email address
  4. Set `smtp_pw` to your email account password (you may have to create an App password if your account has 2FA enabled; for [gmail](https://www.nucleustechnologies.com/supportcenter/kb/how-to-create-an-app-password-for-gmail))
  5. Set `recipient_address` to the email address to send notifications to (should be the same as `smtp_user`)

#### Telegram
If you wish to enable Telegram notifications:
  1. Set `telegram_notification_enabled` to `True` (Already True by default)
  2. Follow this [guide](https://www.teleme.io/articles/create_your_own_telegram_bot?hl=en) to create your telegram bot
  3. Copy the API Token generated and paste it into `telegram_bot_token`
  4. Send your bot `\start` on Telegram
  5. Visit `https://api.telegram.org/botBOT_TOKEN/getUpdates`, replacing `BOT_TOKEN` with your API Token from (3)
  6. Send a test message to your bot on Telegram
  7. There should be a new entry in the JSON on the URL you opened from (5)
  8. Copy the chat ID from the JSON (result::[X]::message::chat::id) and paste it into `telegram_chat_id`

### 3) Program
  - Fill in your CDC username and password under `username` and `password` respectively
  - Set type in `monitored_types` to `True` for the types you want the bot to be checking for

By default, the program will run every 30 minutes as that is the maximum duration CDC will keep a reservation. 
The program will not run from 3am to 6am as it is unlikely other people will cancel their bookings during that time, 
and the user will also likely be asleep and unable to book the session. This is to reduce requests to 2captcha.

# Run it!
Run the program from the working directory `cdc-bot` so that the directories are in the correct path.
```bash
$ python src/main.py
```


# Todos
    1. Add support for slot selections
    2. Bypass cloudflare or hCaptcha
    3. Add check for slots at least 48 hours apart 
