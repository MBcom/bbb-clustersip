# What is it?

astregbackend is a small http service which can be used as realtime backend for Asterisk.

It generates PJSIP-Endpoints (endpoint, aor, auth) within a specific range of numbers when Asterisk asks for it.
It takes a parameter 'digits'.
Whith digits=5, it generates configuration for Endpoints 00000-99999.
Recommend is a value of 11 because for breakout rooms the room number is attached to the PIN. So if you have a 5 digit default PIN length, you need minimum 6 digits for breakout rooms.

After generating the configuration it is stored in a redis.
This is neccessary to serve Asterisk a list of "all" generated endpoints.
When executing 'pjsip show endpoints' for example.

# Installation

```
# install golang and redis
sudo apt install golang-go redis-server git

# get and build astregbackend
git clone https://github.com/MBcom/bbb-clustersip.git
cd bbb-clustersip/astregbackend
go build

cp astregbackend /usr/local/sbin/
cp astregbackend.sample /etc/default/astregbackend
cp astregbackend.service /etc/systemd/system/
cp astregbackend.conf /etc
systemctl daemon-reload
systemctl enable astregbackend
systemctl start astregbackend
```

# Operating

* ensure that the RedisExpiration is bigger than your SIP registration expirationtime! see `astregbackend.conf`
* ensure that astregbackend parameter *digits* matches your BBB setting *defaultNumDigitsForTelVoice* (Defaults to 5 and https://github.com/MBcom/enforce-authentication-greenlight is using 5 digits too)
