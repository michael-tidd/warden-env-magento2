Envisa Magento 2 Application
========================================================

| Env | FrontURL | AdminURL |
| --- | :------- | :------- |
| DEV | https://app.envisa-magento.test/  | https://app.envisa-magento.test/backend/  |
| STG | https://stage.envisa-magento.com/ | https://stage.envisa-magento.com/backend/ |
| PRD | https://www.envisa-magento.com/   | https://www.envisa-magento.com/backend/   |

Other useful URLs on DEV:

* https://mailhog.warden.test/
* https://rabbitmq.envisa-magento.test/
* https://elasticsearch.envisa-magento.test/

## Developer Setup

### Prerequisites:

* [Warden](https://warden.dev/) 0.6.0 or later is installed. See the [Installing Warden](https://docs.warden.dev/installing.html) docs page for further info and procedures.
* `pv` is installed and available in your `$PATH` (you can install this via `brew`, `dnf`, `apt` etc)

### Initializing Environment

In the below examples `~/Sites/envisa-magento` is used as the path. Simply replace this with whatever path you will be running this project from. It is recommended however to deploy the project locally to a case-sensitive volume.

 1. Clone the project codebase.

        git clone -b develop git@github.com:michael-tidd/warden-env-magento2.git \
            ~/Sites/envisa-magento

 2. Change into the project directory.

        cd ~/Sites/envisa-magento

 3. Configure composer credentials.

     Manually create `webroot/auth.json` using the following template:

        {
            "http-basic": {
                "repo.magento.com": {
                    "username": "<username>",
                    "password": "<password>"
                }
            }
        }

 4. Run the init script to bootstrap the environment, starting the containers and mutagen sync (on macOS), installing the database (or importing if `--db-dump` is specified), and creating the local admin user for accessing the Magento backend.
    
    To install a Magento 2 Community Environment:
        
        warden bootstrap --clean-install --meta-package magento/project-community-edition
    
    To install a Magento 2 Enterprise Environment:
        
        warden bootstrap --clean-install --meta-package magento/project-enterprise-edition

 5. Load the site in your browser using the links and credentials taken from the init script output. 

    **Note:** If you are using **Firefox** and it warns you the SSL certificate is invalid/untrusted, go to Preferences -> Privacy & Security -> View Certificates (bottom of page) -> Authorities -> Import and select `~/.warden/ssl/rootca/certs/ca.cert.pem` for import, then reload the page.
    
    **Note:** If you are using **Chrome** on **Linux** and it warns you the SSL certificate is invalid/untrusted, go to Chrome Settings -> Privacy And Security -> Manage Certificates (see more) -> Authorities -> Import and select `~/.warden/ssl/rootca/certs/ca.cert.pem` for import, then reload the page.

### Additional Configuration

Information on configuring and using tools such as Xdebug, LiveReload, MFTF, and multi-domain site setups may be found in the Warden docs page on [Configuration](https://docs.warden.dev/configuration.html).

### Destroying Environment

To completely destroy the local environment we just created, run `warden env down -v` to tear down the project’s Docker containers, volumes, and (where applicable) cleanup the Mutagan sync session.
