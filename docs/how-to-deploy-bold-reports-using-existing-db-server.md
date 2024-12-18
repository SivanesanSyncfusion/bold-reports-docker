# How to deploy Bold Reports using existing DB server?

In the following section, we are going to run the Bold Reports application by passing the application startup backend configuration environment variable using the existing DB server details.

1. Download docker compose file using the following command.
   ```sh
   curl -o docker-compose.yml "https://raw.githubusercontent.com/boldreports/bold-reports-docker/master/deploy/single-container-with-env-variable/docker-compose.yml"
   ```

   **Environment variable Usage:**

   | Name               | Required    | Description |
   | -------------      | ------------- | ------------- |
   | `BOLD_SERVICES_UNLOCK_KEY`                 | Yes    | License key for activating Bold Reports. Please refer to [this document](https://support.boldreports.com/kb/article/13271/how-do-i-get-my-offline-license-key-from-our-bold-reports-account-page) to download the key.<br />If you don't have the download key option, please create a support ticket [here](https://support.boldreports.com/support/tickets/create).|
   | `BOLD_SERVICES_DB_TYPE`                    | Yes    | Type of database server can be used for configuring Bold Reports. <br /><br />The following DB types are accepted:<br />1. mssql – Microsoft SQL Server/Azure SQL Database<br />2. postgresql – PostgreSQL Server<br />3. mysql – MySQL/MariaDB Server|
   | `BOLD_SERVICES_DB_HOST`                    | Yes    | Name of the Database Server|
   | `BOLD_SERVICES_DB_PORT`                    | No     | The system will use the following default port numbers based on the database server type.<br />PostgrSQL – 5234<br />MySQL -3306<br /><br />Please specify the port number for your database server if it is configured on a different port.<br /><br />For MS SQL Server, this parameter is not necessary.|
   | `BOLD_SERVICES_DB_USER`                    | Yes    | Username for the database server
   | `BOLD_SERVICES_DB_PASSWORD`                | Yes    | The database user's password|
   | `BOLD_SERVICES_DB_NAME`                    | No     | If the database name is not specified, the system will create a new database called bold services.<br /><br />If you specify a database name, it should already exist on the server.|
   | `BOLD_SERVICES_POSTGRESQL_MAINTENANCE_DB`  | Yes    | For PostgreSQL DB Servers, this is an optional parameter.<br />The system will use the database name `postgres` by default.<br />If your database server uses a different default database, please provide it here.|
   | `BOLD_SERVICES_DB_ADDITIONAL_PARAMETERS`   | No     | If your database server requires additional connection string parameters, include them here.<br /><br />Connection string parameters can be found in the official document.<br />My SQL: https://dev.mysql.com/doc/connector-net/en/connector-net-8-0-connection-options.html<br />PostgreSQL: https://www.npgsql.org/doc/connection-string-parameters.html<br />MS SQL: https://docs.microsoft.com/en-us/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring<br /><br /><b>Note:</b> A semicolon(;) should be used to separate multiple parameters.|
   | `BOLD_SERVICES_USER_EMAIL`                 | Yes    | It should be a valid email.|
   | `BOLD_SERVICES_USER_PASSWORD`              | Yes    | It should meet our password requirements.<br /><br /><b>Note:</b>Password must meet the following requirements. It must contain at least 6 characters, 1 uppercase character, 1 lowercase character, 1 numeric character, 1 special character|

2. Run docker compose up command.
   ```sh
   docker-compose up -d
   ```
   ![docker-compose-up](./images/docker-compose-up.png)
   > **Note:** The docker volumes `boldservices_data` and `db_data` persists data of Bold Reports and PostgreSQL respectively. [Learn more about docker volumes](https://docs.docker.com/storage/volumes/)

3. Now, access the Bold Reports application by entering the URL as `http://localhost:8090` or `http://host-ip:8090` in the browser. When opening this URL in the browser, it will configure the application startup in the background and display the page below within a few seconds. The default port number mentioned in the compose file is 8090. If you are making changes to the port number, then you need to use that port number for accessing the Bold Reports application.

   ![docker-startup](../docs/images/docker-startup.png)
   > **Note:** The Bold Reports site is not immediately available on port 80 because the containers are still being initialized and may take a couple of minutes for the first load.

## Shutdown and Cleanup
The command `docker-compose down` removes the containers and default network, but preserves the volumes of Bold Reports and PostgreSQL.

The command `docker-compose down --volumes` removes the containers, default network, and all the volumes.