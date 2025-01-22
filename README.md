# TRMNL BYOS - Ruby/Sinatra
with this project you can point a TRMNL (https://usetrmnl.com) device to your own server, either in the cloud or on your local network.

**requirements**

- TRMNL device
- TRMNL firmware (forked with new base url - your server)
- Ruby installed on your machine (or Docker)

**quickstart**

```
bundle config --set # installs gems/libs
bundle exec rake db:setup # creates db + Devices table
bundle exec ruby app.rb # => runs server, visit http://localhost:4567
```

**docker-based setup**
You may optionally edit the Dockerfile to enable sqlite.
```
docker build -t trmnl_byos -f Dockerfile .
docker run --name trmnl -p 4567:4567 trmnl_byos
```

**debugging / building**

first access the console: `rake console`

```
Device.count # => 0 (interact with db ojects via ActiveRecord ORM)
ScreenFetcher.call (fetch upcoming render)
ScreenGenerator.new("<p>Some HTML here</p>").process # => creates img in /public/images/generated
```

**deploying**

on your local network:

1. run app in production mode (`ruby app.rb`)
2. retrieve your machine's local IP, ex 192.168.x.x (Mac: `ifconfig | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}'`)
3. confirm it works by visiting `http://192.168.x.x:4567/devices` from any device also on the network
4. set `BASE_URL` inside `app.rb` to this domain (`http://192.168.x.x:4567`, no trailing slash)
4. point your [forked FW's](https://github.com/usetrmnl/firmware) `API_BASE_URL` ([source](https://github.com/usetrmnl/firmware/blob/2ee0723c66a3468b969c83d7663ffb3f8322ad99/include/config.h#L56)) to same value as `BASE_URL`

in the cloud:

TBD
