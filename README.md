# Highlights

Automatically email yourself a random Amazon Kindle highlight from your collection with this script. I run it on Heroku for free and email you a highlight each day.

## Heroku Setup Instructions

1. Clone this repository.

2. [Install the Heroku toolbelt.](https://toolbelt.heroku.com/) (and run ```heroku login```)

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
heroku addons:create scheduler
```

6. Add the free Heroku Mailgun add on to your app. This will automatically set your email environment variables as well:
```
heroku addons:create mailgun
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

9. Verify your [Mailgun SMTP Domain](https://help.mailgun.com/hc/en-us/articles/202052074-How-do-I-verify-my-domain-)


Optionally you can test the script via ```heroku run bundle exec rake```

## Notes
The default rake task first downloads and updated list of your highlights, then emails a random one to the TO address. This is because the file system on the Heroku Cedar stack is ephemeral. Each time a dyno is spun up, the file system is wiped, including any previous copies of data.json, the JSON file containing your highlights. For this reason, the script naively downloads an updated copy each time.
