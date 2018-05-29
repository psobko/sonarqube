# sonarqube
Based on OWASP SonarQube Project - this is a fork which pre-configures a docker image running SonarQube CE to support static analysis of Swift/Obj-C code using the non-commercial [sonar-swift](https://github.com/Backelite/sonar-swift) plugin.

### Prerequisites
- sonar-swift dependancies installed:
	- brew install swiftlint
	- brew install tailor
	- gem install slather
	- sudo -H pip install lizard
	

### Launching

To quickly evaluate the image without persistance:

 `$ docker build -t "sonarqube" docker/.`

`$ docker run -d -p 9000:9000 -p 9092:9092 psobko/sonarqube`

> SonarQube will display a warning that the database is not production-ready. This is expected behaviour.

Otherwise [docker-compose](https://github.com/docker/compose) will start and configure containers for SonarQube and a PostgreSQL database:

`$ docker-compose up`

If any changes are made to the docker configuration (updating plugins, etc) you may need to restart SonarQube for the changes to take affect:

Because the PostgreSQL data is persistant between instances you may run into a scenario where you want to wipe the data and start fresh:

```	sh
$ docker-compose stop
$ docker-compose rm -f
$ docker-compose pull
$ docker-compose up -d --force-recreate
```

### Usage

Access the web interface via: [http://localhost:9000](http://localhost:9000)

##### Initial Configuration

On the first run you will need to generate an auth token:

- Open [http://localhost:9000](http://localhost:9000) in a browser
- Login with "admin" for username and "admin" for password
- If you do not see a form pop up follow this URL: http://localhost:9000/account/security/
- In both cases you should see a name field and a generate button, pick any name and click **generate**
- **Copy the token** - you will only see it once

> If you get errors with `lizard` or can't execute the command you'll need to manually install it.

##### Analyzing Code

First run: `$ sh setup-sonar-swift.sh`

After that: `$ sonar-scanner`

> If you get an error saying swift isn't supported try to restart the docker image and check the webpanel to see if Swift is visible.







