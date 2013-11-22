# Highlights

This rakefile can be deployed on Heroku to automatically downlad your Amazon Kindle highlights, then regularly email one to you.

## Changes from the Original

Tobi's original [Kindle highlights rakefile](https://github.com/tobi/highlights) was intended to run as a local cron job. I've updated this implementation with instructions to run on Heroku as a scheduled job. Changes include:
1. Updating the default rake task, from download and print, to download and email.
2. Change the SMTP environment varaible to default to Heroku's Mailgun ENV variables.

## Heroku Setup Instructions

1. Clone this repository.

2. [Install the Heroku toolbelt.](https://toolbelt.heroku.com/)

3. In the highlights repository, create a new Heroku app:
```
heroku apps:create
```

4. Push your Ruby app to Heroku:
```
git push heroku master
```

5. Add the Heroku Scheduler add on to your app:
```
heroku addons:add scheduler
```

6. Add the free Heroku Mailgun add on to your app. This will automatically set your email environment variables as well:
```
heroku addons:add mailgun:free
```

7. Set your additional environment variables in Heroku:
```
heroku config:set AMAZON_USER="youramazonusername@email.com"
heroku config:set AMAZON_PASS="youramazonpassword"
heroku config:set TO="youraddress@email.com"
```

8. On your [Heroku scheduler dashboard](https://scheduler.heroku.com/dashboard), schedule the default rake task daily (or at an interval of your choosing):
```
bundle exec rake
```
![Heroku Scheduler Task screenshot](https://dl.dropboxusercontent.com/u/891448/heroku-kindle-scheduler.png "Heroku Scheduler Kindle notifier task")

## Notes
The default rake task first downloads and updated list of your highlights, then emails a random one to the TO address. This is because the file system on the Heroku Cedar stack is ephemeral. Each time a dyno is spun up, the file system is wiped, including any previous copies of data.json, the JSON file containing your highlights. For this reason, the script naively downloads an updated copy each time.