<h2 align="center">
<a href="https://katie.qa"><img src="https://github.com/wyona/katie-backend/blob/main/src/main/webapp/assets/img/katie_logo.svg"/></a> (Backend)
</h2>

## About

<strong>[Katie](https://katie.qa)</strong> is an Open Source AI-based question-answering platform that helps companies and organizations make their private domain knowledge accessible and useful to their employees and customers.

<strong>Katie</strong> is integrated with [Discord](https://app.katie.qa/discord.html), [Slack](https://app.katie.qa/slack.html), [MS Teams](https://app.katie.qa/ms-teams.html), [Matrix](https://app.katie.qa/matrix.html), [E-Mail](https://app.katie.qa/email-integration.html), Wordpress and [TOPdesk](https://app.katie.qa/topdesk-integration.html), and also provides a web interface for expert users.

<strong>Katie</strong> can be connected with all your applications and data repositories, like for example Websites, Confluence, SharePoint, OneNote, Outlook, Supabase, Directus, Discourse, etc.

<strong>Katie</strong> is based on state of the art AI and supports embedding and large language models of your choice.

## Quickstart

* Download docker compose [file](https://github.com/wyona/katie-backend/blob/main/env/docker/run/docker-compose.yml)
* Open docker compose file and configure volume path (search for TODO_REPLACE_DIRECTORY_PATH)
* Optional: Open docker compose file and customize environment variables
    * For example outgoing mail configuration (APP_MAIL_HOST, ...)
* Run 'docker-compose up -d'
* Check log 'docker-compose logs -f'
* Katie will be available at http://localhost:8044
* Login with the following credentials U: superadmin, P: Katie1234%

## Requirements

The <strong>Katie</strong> backend webapp is based on Spring Boot and to build and run locally you need

* JDK: 11
    * https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html
* Maven version: 3.3.3 (IMPORTANT: Please double check Maven .m2/settings.xml)
    * https://maven.apache.org/download.cgi
    * https://maven.apache.org/install.html

## Build and Run from Command Line

* Open file 'src/main/resources/application-dev.properties' and configure property 'volume.base.path'
* Set environment variable: export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.11.jdk/Contents/Home
* Set environment variable: export PATH=/Library/Java/JavaVirtualMachines/jdk-11.0.11.jdk/Contents/Home/bin:$PATH
* Set environment variable: export SPRING_PROFILES_ACTIVE=dev
* Configure your JDK version inside the shell script 'build.sh' (search for JAVA_VERSION_REQUIRED)
* Build Katie webapp as war file: <em>sh build.sh</em>
* Startup Katie: <em>java -jar target/askkatie-webapp-1.353.0-SNAPSHOT.war</em>
* Katie will be available at: http://localhost:8044 or https://localhost:8443 (see SSL properties inside src/main/resources/application.properties)
* Login with the following credentials: U: superadmin, P: Katie1234% (see volume/iam/users.xml)

Optionally you can run Katie with an outgoing proxy configuration enabled (https://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html)

* Startup Katie with proxy configuration: <em>java -Dhttp.proxyHost=proxy.wyona.com -Dhttp.proxyPort=8044 -Dhttps.proxyHost=proxy.wyona.com -Dhttps.proxyPort=443 -Dhttp.nonProxyHosts="*aleph-alpha.com|*cohere.ai" -Dhttp.proxyUser=USERNAME -Dhttp.proxyPassword=PASSWORD -Dhttps.proxyUser=USERNAME -Dhttps.proxyPassword=PASSWORD -jar target/askkatie-webapp-1.353.0-SNAPSHOT.war</em>

## IntelliJ (Ultimate)

* Download IntelliJ IDEA Ultimate: https://www.jetbrains.com/idea/download/other.html
* Open file 'src/main/resources/application-dev.properties' and configure property 'volume.base.path'
* Open IntelliJ: File > Open (select directory "katie-backend")
* Set JDK version: File > Project Structure > Project SDK: 11
    * Also see https://www.jetbrains.com/help/idea/maven-support.html#change_jdk
* Run Maven (askkatie-webapp > Lifecycle): clean + install
    * Optional: Disable tests (Preferences > Build, Execution, Deployment > Build Tools > Maven > Runner > Skip tests)
* Click on "Add Configuration..."
    * Click "+" to add new configuration: Spring Boot
    * Set Name, e.g. "Katie"
    * Main class: com.erkigsnek.webapp.Server
    * Environment, JRE: /Library/Java/JavaVirtualMachines/jdk-11.0.11.jdk/Contents/Home
    * Active profiles: dev
        * Optional: Set environment variable SPRING_PROFILES_ACTIVE=dev
* Close configuration and run "Katie"
* Open http://localhost:8044 inside your browser and login with U: superadmin, P: Katie1234%

Make sure you have Lombok configured

https://www.baeldung.com/lombok-ide

In case startup fails, then delete the .idea directory and the file askkatie-webapp.iml, and reopen the project.

## Eclipse (Version: 2022-06 (4.24.0))

* Go to Help | Eclipse Marketplace...
* Search for "Spring" and install "Spring tools 4" (by VMware) https://www.eclipse.org/community/eclipse_newsletter/2018/february/springboot.php
* Import the project from intelliJ to Eclipse, inside intelliJ go to File | Export | Project to Eclipse...
* Select "Convert selected modules into Eclipse-compatible format" and "Export non-module libraries"
* In Eclipse, go to File | Import and select General->Projects from Folder or Archive
* Select the root directory katie-backend for the Import source directory. Eclipse should detect katie-backend as a Project
* Install Lombok for Slf4j by downloading lombok.jar from here https://projectlombok.org/download
* (WINDOWS) And then executing the installer in a powershell with "java -jar lombok.jar" and re-open eclipse
* Configure 'volume.base.path' and comment or disable 'server.servlet.session.cookie.secure' inside src/main/resources/application-dev.properties
*(WINDOWS) Your volume.base.path MUST be absolute like for example: "G:/katie/katie-backend/volume". If you want to start the app in any other configuration, for example prod, don't forget to change the volume.base.path from "/katie-backend/volume" to "./katie-backend/volume"
* Click on "Run | Run configurations..."
    * Click "New launch configuration" to add new configuration: Spring Boot
    * Main class: com.erkigsnek.webapp.Server
    * !!! Select Profile dev

* Apply configuration and run
* Open http://localhost:8044 inside your browser and login with U: superadmin, P: Katie1234%

## Run Katie within Tomcat

* Configure Tomcat path inside build.sh (see TOMCAT_HOME)
* Configure 'volume.base.path' and comment/disable 'server.servlet.session.cookie.secure' inside src/main/resources/application-prod.properties
* Set base href to "/katie/" inside src/main/webapp/index.html
* Set i18n path to "./assets/i18n/" and TinyMCE path to 'base_url:"./tinymce"' inside src/main/webapp/main.js
* Build webapp as war, run: 'sh build.sh'
* Startup Tomcat: ./bin/startup.sh
* Request in browser: http://127.0.0.1:8080/katie/
* tail -F logs/*

## Generate Katie Docker image and run Katie Docker container

* Optional: Comment/disable 'server.servlet.session.cookie.secure' inside src/main/resources/application.properties
* Set environment variable: export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-11.0.11.jdk/Contents/Home
* Configure your JDK version inside the shell script 'build.sh' (search for JAVA_VERSION_REQUIRED)
* Build webapp as war, run: 'sh build.sh'
* Build image: docker build -t katie .
* Tag image: docker tag katie wyona/katie:1.353.0
* Push image to Docker Hub: docker login -u USERNAME -p PASSWORD docker.io && docker push wyona/katie:1.353.0
* Run image:
    * docker run -p 7070:8080 -v /Users/michaelwechner/src/katie-backend/volume:/ask-katie katie ("/ask-katie" is set inside application-prod.properties)
    * docker run -p 7070:8080 katie (If you do not mount a volume, then Katie creates the necessary files and directories within the docker container, which gets reset upon restart though)
* Open http://localhost:7070 inside your browser and login with U: superadmin, P: Katie1234%
* REST interfaces: http://127.0.0.1:7070/swagger-ui/
* Health check endpoint: http://127.0.0.1:7070/actuator/health
    * Other actuator endpoints: http://127.0.0.1:7070/actuator (whereas set management.endpoints.web.exposure.include=* inside application(-dev).properties)

Or as another alternative run:

sh pull-down-up.sh

whereas make sure to configure VOLUME_KATIE inside the script accordingly.

## Docker using Tomcat

* src/main/webapp/index.html: &lt;base href="/katie/"&gt;
* src/main/webapp/main.js: "./assets/i18n/" (1 location) and 'base_url:"./tinymce"' (6 locations)
* sh build.sh
* Check available Tomcat version at https://dlcdn.apache.org/tomcat/tomcat-8/ and update inside Dockerfile_Java11_Tomcat
* Build image containing Tomcat: docker build -t katie-tomcat -f Dockerfile_Java11_Tomcat .
* docker run -p 7070:8080 -v /Users/michaelwechner/src/katie-backend/volume:/ask-katie katie-tomcat
* http://localhost:7070/katie/

## API and Testing API

* https://app.katie.qa/swagger-ui/
* Postman: env/postman/AskKatie.postman_collection.json

## Database / Flyway

Create / migrate Database on startup of Katie web app

* pom.xml (flyway dependency)
* src/main/resources/application.properties (flyway and h2 configuration, volume/askkatie-h2.mv.db)
* src/main/java/com/erkigsnek/webapp/config/DataSourceConfig.java
* SQL Scripts: src/main/resources/db/migration

When running Katie as Docker

* Mount volume to access h2 database (volume/askkatie-h2.mv.db) from outside of docker container: -v /Users/michaelwechner/src/katie-backend/volume:/ask-katie

Documentation

* https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-execute-flyway-database-migrations-on-startup
* https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-data-access

Backup and restore h2 database:

* http://www.h2database.com/html/tutorial.html#upgrade_backup_restore
* https://stackoverflow.com/questions/2036117/how-to-back-up-the-embedded-h2-database-engine-while-it-is-running

Access database from command line (WARN: Might not work properly when database is already in use by server, either stop server or make a copy and connect with copy)

* Run: java -cp /Users/michaelwechner/.m2/repository/com/h2database/h2/1.4.199/h2-1.4.199.jar org.h2.tools.Shell
* Enter:
    * jdbc:h2:file:/Users/michaelwechner/src/katie-backend/volume/askkatie-h2
    * org.h2.Driver
    * Username and Password see application.properties
    * show tables;
    * show columns from QUESTION;
    * select * from public."flyway_schema_history";
    * select * from REMEMBERME;
    * quit

## Update Angular Frontend

* git clone git@github.com:wyona/katie-expert-frontend-angular.git
* cd katie-expert-frontend-angular
* npm install
* ng build
* ../katie-backend
* cp -r ../katie-expert-frontend-angular/dist/admin-backend/* src/main/webapp/.
* sh build.sh

## Create Release

Update the version in the following files:

* pom.xml
* src/main/resources/application.properties
* env/docker/run/docker-compose.yml
* README.md

## Elasticsearch 6.6.1

Basic configuration: src/main/resources/application.properties

Implementation: src/main/java/com/erkigsnek/webapp/handlers/ElasticsearchQuestionAnswerImpl.java

List all indices https://elasticsearch-vt.wyona.com/_cat/indices
Get all hits of a particular index: E.g. https://elasticsearch-vt.wyona.com/askkatie_1b3805a8-84db-452d-ae00-b13755686d30/_search

## Slack App

Also see https://app.katie.qa/slack.html

Create new App or update existing App:

* Login to "Wyona Workspace" https://wyonaworkspace.slack.com
* Create new App: https://api.slack.com/apps
* Update existing App: https://api.slack.com/apps/A0184KMLJJE

Update configuration parameters inside docker-compose.yml (overwriting parameters inside src/main/resources/application.properties and src/main/resources/secret-keys.properties)

Config values of (Basic Information) of your App

* SLACK_SIGNATURE (slack.signature)
* SLACK_CLIENT_ID (slack.client.id)
* SLACK_CLIENT_SECRET (slack.client.secret)

Update Slack App configuration

* Config "Request URL" (Menu: Interactivity & Shortcuts, Manifest: interactivity)
    * E.g. https://app.katie.qa/api/v1/slack/interactivity or https://askvt.wyona.com/api/v1/slack/interactivity or https://katie.jmc-software.com/api/v1/slack/interactivity

* Config command (Manifest: slash_commands)
    * /katie
    * https://app.katie.qa/api/v1/slack/command/katie or https://askvt.wyona.com/api/v1/slack/command/katie or https://katie.jmc-software.com/api/v1/slack/command/katie
    * Get help on how to use Katie

* Add New Redirect URL (Menu: OAuth & Permissions, Manifest: redirect_urls)
    * E.g. https://app.katie.qa/api/v1/slack/oauth2-callback or https://askvt.wyona.com/api/v1/slack/oauth2-callback or https://katie.jmc-software.com/api/v1/slack/oauth2-callback
    * Bot Token scopes
        * channels:history
        * chat:write
        * commands
        * im:history
        * incoming-webhook
        * team:read
    * User Token scopes
        * im:history (Permits direct communication with Katie)

* Config Event Subscriptions (Manifest: event_subscriptions)
    * E.g. https://app.katie.qa/api/v1/slack/events or https://askvt.wyona.com/api/v1/slack/events or https://katie.jmc-software.com/api/v1/slack/events
    * Bot Events scopes
        * message.channels
        * message.im

* Activate / Manage Distribution
    * E.g. https://slack.com/oauth/v2/authorize?client_id=1276290213363.2089707188626&scope=channels:history,commands,im:history,chat:write,incoming-webhook,team:read&user_scope=im:history

Slack Katie App Manifest (https://app.slack.com/app-settings/T01848J69AP/A0184KMLJJE/app-manifest):

<pre>
_metadata:
  major_version: 1
  minor_version: 1
display_information:
  name: Katie
  description: Katie is a question answering bot, continuously improving, self-learning and trained by humans.
  background_color: "#000000"
  long_description: Katie answers questions using artificial and natural intelligence, whereas Katie is currently not intended for conversations beyond asking one question at a time. Natural conversations are much more complex than just detecting duplicated/similar questions. Simple dialogs with clear intents, such as for example a restaurant reservation or initiate a phone call, work quite well already, but more complex conversations are much more difficult and users become frustrated and will eventually stop trying to have more complex conversations.
features:
  app_home:
    home_tab_enabled: true
    messages_tab_enabled: false
    messages_tab_read_only_enabled: false
  bot_user:
    display_name: katie
    always_online: false
  slash_commands:
    - command: /katie
      url: https://app.katie.qa/api/v1/slack/command/katie
      description: Get help on how to use Katie
      should_escape: false
oauth_config:
  redirect_urls:
    - https://app.katie.qa/api/v1/slack/oauth2-callback
  scopes:
    user:
      - im:history
    bot:
      - channels:history
      - chat:write
      - commands
      - im:history
      - incoming-webhook
      - team:read
settings:
  event_subscriptions:
    request_url: https://app.katie.qa/api/v1/slack/events
    user_events:
      - message.im
    bot_events:
      - message.channels
      - message.im
  interactivity:
    is_enabled: true
    request_url: https://app.katie.qa/api/v1/slack/interactivity
    message_menu_options_url: https://app.katie.qa/api/v1/slack/options-load
  org_deploy_enabled: false
  socket_mode_enabled: false
  token_rotation_enabled: false
</pre>

## MS Teams App

Also see https://app.katie.qa/ms-teams.html

Open Developer Portal https://dev.teams.microsoft.com/apps

- Click on the "Apps" menu item
- Click on "New App"
- App details
  - Short name: Katie
  - Full name: Katie
  - App ID: Generate App ID, e.g. "a2ae51d0-19a4-416e-ba74-1812533d5be8"
  - Package Name: com.wyona.teams.devapp
  - Version: 1.0.0
  - Short description: Reliable Answers for your Employees and Customers
  - Full description: Reliable Answers for your Employees
  - Developer/Company Name: Wyona AG
  - Website: https://katie.qa
  - Privacy Statement: https://wyona.com/privacy-policy
  - Terms of use: https://katie.qa
  - Application (client) ID, e.g. "e9d6ff18-084d-4891-97ed-8a7667db3d7a"
- Branding
  - Upload logo 192x192 and 32x32 (see src/main/webapp/ms-teams-app)
- App features
  - Click on "Bot"
  - Click on "Existing bot"
  - Connect to a different bot id: e9d6ff18-084d-4891-97ed-8a7667db3d7a
    - Login to Azure Portal, click on katie7, click on Configuration, Copy "Microsoft App ID"
  - Scope
    - Personal
    - Team
    - Group Chat
- Publish
  - Click on "App package"
  - Click on "Download app package"