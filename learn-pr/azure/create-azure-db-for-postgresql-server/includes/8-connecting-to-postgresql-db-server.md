Lets' assume you're using an on-premises PostgreSQL database. You're managing all security aspects and locked down all access to your servers using the standard PostgreSQL server level firewall rules. You now have a good understanding of how to configure the same server level firewall rules in Azure.

You now have the chance to connect to one of the previous Azure Databases for PostgreSQL servers you created using `psql`.

## Allow Azure service access

Before we begin. You'll have to allow access to Azure services if you want to use PowerShell and `psql` to connect to your server. Recall, that you can allow access in two ways.

Your first option is to enable **Allow access to Azure services**. Allowing access will create a firewall rule even though the rule isn't entered in the list of custom rules you create.

Your second option is to create a firewall rule that allows access to all IP addresses. The rule will include the IP address for the client running PowerShell that you'll use to execute `psql` from.

You also need to disable the **Enforce SSL connection**.

Let's begin.

Sign in to [the Azure portal](https://portal.azure.com?azure-portal=true). Navigate to the server resource for which you would like to create a firewall rule.

Select the **Connection Security** option to open the connection security blade to the right.

![Screenshot of the Azure portal showing the Connection security section of the PostgreSQL database resource blade.](../media-draft/7-db-security-settings.png)

Recall, you want to allow access to PowerShell clients running `psql`.

You can choose to either:

- Set **Allow access to Azure services** to **ON**
- Set **Enforce SSL connection** to **DISABLED**
- Click the **Save** button to save your changes

Or, you can add a firewall rule to allow access to all IP addresses by adding a firewall rule. Use the following values:

- Rule Name: `AllowAll`
- Start IP: `0.0.0.0`
- End IP: `255.255.255.255`
- Set **Enforce SSL connection** to **DISABLED**
- Click the **Save** button to save your changes

> [!Warning]
> Creating this firewall rule will allow any IP address on the Internet to attempt to connect to your server. Even though clients will not be able access the server without the username and password, enable this rule with caution and make sure you understand the security implications. In production environments, you'll only allow access to specific client IP addresses.

For the next steps, you'll start an Azure Cloud Shell session. This lab uses `bash` as the command-line environment.

- Open the Cloud Shell from the Azure portal. Go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:

- In case you have several subscriptions, check to make sure you're using the correct subscription when asked. You can also run the following command to set the active subscription. Remember to replace the zeros with your subscription identifier.

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

- Connect psql to your server using the following command:

  ```bash
  psql --host=<server-name>.postgres.database.azure.com --username=<admin-user>@<server-name> --dbname=postgres
  ```

   Recall, `server-name`, and `admin-user` are the values you chose for the administrator account when you created the server. `postgres` is the default management database every PostgreSQL server is created with. You'll be prompted for the password you provided when you created the server.

- Once successfully connected, execute the `\l` command to list all databases.

   This command will result in two or more default databases returned from.

- When you're finished executing psql operations on your server, execute the command `\q` to quit `psql`.

- Finally, to exit Cloud Shell, execute the command `exit`.
