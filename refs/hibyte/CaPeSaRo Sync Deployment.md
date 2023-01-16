## Requirements (Sync Machine)
- Google Chrome (`yay -S google-chrome`)
- Google Chrome WebDriver that matches the version of installed Google Chrome (check with `google-chrome-stable --version`)
- Java 11 JDK
- JAR with the microservice

## Requirements (MediQ Backend Machine)
- `APP_BASEURL` environment variable set to the URL of the instance on which backend is deployed
- `CAPESARO_SYNC_MP_REST_URI` URL of CaPeSaRo sync microservice

**Note:** If instances can't communicate, setup firewalls properly on both machines (or disable with `systemctl stop firewalld`).

## Sync Machine Setup
After disabling or setting up the firewall, create this script and replace the values of the environment variables with appropriate values:

*start.sh*
```sh
export WD_PATH="/home/hibyte/Downloads/chromedriver"
export CPSR_USER="user"
export CPSR_PASSWORD="password"
export KC_USER="admin"
export KC_PASSWORD="admin"

java -jar mediq-capesaro.jar
```

After that make the script executable and run it:

```sh
chmod +x ./start.sh
./start.sh
```