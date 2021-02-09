
<div style="position: absolute; top: 0px; right: 0px;">
    <img width="200" height="200" src="https://redislabs.com/wp-content/uploads/2020/12/RedisLabs_Illustration_HomepageHero_v4.svg">
</div>

<div style="height: 150px"></div>


# Spring(java) Redis rate-limiting Example
Show how the redis works with Spring(java).

# Redis rate-limiting example (front)

![alt text](preview.png)

## Try it out
<p>
    <a href="https://heroku.com/deploy" target="_blank">
        <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy to Heorku" width="200px"/>
    <a>
</p>

Don't forget to add add-ons heroku redis  

<p>
    <a href="https://deploy.cloud.run" target="_blank">
        <img src="https://deploy.cloud.run/button.svg" alt="Run on Google Cloud" width="200px"/>
    </a>
    (See notes: How to run on Google Cloud)
</p>


## How to run on Google Cloud

<p>
    If you don't have redis yet, plug it in  (https://spring-gcp.saturnism.me/app-dev/cloud-services/cache/memorystore-redis).
    After successful deployment, you need to manually enable the vpc connector as shown in the pictures:
</p>

1. Open link google cloud console.

![1 step](docs/1.png)

2. Click "Edit and deploy new revision" button.

![2 step](docs/2.png)

3. Add environment.

![3 step](docs/3.png)

4.  Select vpc-connector and deploy application.

![4  step](docs/4.png)

<a href="https://github.com/GoogleCloudPlatform/cloud-run-button/issues/108#issuecomment-554572173">
Problem with unsupported flags when deploying google cloud run button
</a>

---

# How it works?
## 1. How the data is stored:
<ol>
    <li>New responses are added key-ip:<pre> SETNX 127.0.0.1:PING 10</pre></li>
    <li>Set a timeout on key:<pre>EXPIRE 127.0.0.1:PING 10</pre></li>
</ol>
<br/>

## 2. How the data is accessed:
<ol>
    <li>Next responses are get bucket: <pre>GET 127.0.0.1:PING</pre> </li>
    <li> Next responses are changed bucket: <pre>DECRBY 127.0.0.1:PING </pre> </li>
</ol>
 
#### See: https://redis.io/commands/INCR#pattern-rate-limiter-2       

---

## How to run it locally?

## Development

```
git clone https://github.com/deliveryweb/redis-rate-limiting-java.git
```

### Run docker compose or install redis manually
Install docker (on mac: https://docs.docker.com/docker-for-mac/install/)
```sh
docker network create global
docker-compose up -d --build
```

#### If you install redis manually open src/main/resources/ folder and provide the values for environment variables in application.properties
    spring.redis.database=
    spring.redis.host=
    spring.redis.port=
    spring.redis.password=
    spring.redis.timeout=

#### else  
    copy .env.example .env
    export $(cat .env | xargs)

#### Setup and run 

Install gradle (on mac: https://gradle.org/install/)


Install JDK (on mac: https://docs.oracle.com/javase/10/install/installation-jdk-and-jre-macos.htm)
``` sh
gradle wrapper
./gradlew build
./gradlew run
```