Storage accounts let you create a group of data management rules and apply them all at once to a group of Azure Blobs, Azure Files, Azure Queues, and Azure Tables. 

If you tried to achieve the same thing without storage accounts, it would be tedious and error-prone. For example, what are the chances that you could successfully apply the exact same ruleset to thousands of blobs?

Instead, you capture the rules in the settings for a storage account, and those rules are automatically applied to every data service in the account.