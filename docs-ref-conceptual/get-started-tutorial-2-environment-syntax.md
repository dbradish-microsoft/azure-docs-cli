---
title: Write scripts for Bash, PowerShell and Windows Cmd | Microsoft Docs
description: Learn about quoting differences, line continuation and debugging in Bash, PowerShell and Windows Cmd environments.
manager: jasongroce
author: dbradish-microsoft
ms.author: dbradish
ms.date: 11/15/2023
ms.topic: conceptual
ms.service: azure-cli
ms.tool: azure-cli
ms.custom: devx-track-azurecli
keywords: azure, 
---
# Learn Azure CLI syntax differences in Bash, PowerShell and Cmd

Azure CLI commands can be executed in both [Bash](https://opensource.com/resources/what-bash), [PowerShell](/powershell/scripting/overview), and [Windows command shell (Cmd)](/windows-server/administration/windows-commands/windows-commands) environments. However, there are subtile scripting differences. This tutorial will teach you how to create your first Azure Storage Account and format Azure CLI parameter values for all three environments.

## Prerequisites

* You have completed the prerequisites to [prepare your environment](./get-started-tutorial-1-prepare-environment.md).
* You have access to a resource group with `contributor` permissions.

## Be aware of line continuation characters

Most Azure CLI documentation is written and tested in Bash using Azure Cloud Shell.  One of the first things to remember when copying Azure CLI syntax is to remove or verify line continuation characters for your chosen environment. Line continuation characters are environment-specific and are not interchangeable.

| Environment | Line continuation character |
| - | - |
| Bash | Backslash (`\`)
| PowerShell | Backtick (````), (`\```), (``\``), (`\``)
| Cmd | Carrot (`^`)

 The **Copy** button in the upper right corner of Azure CLI code blocks removes the backslash (`\`) and the backtick (`\``) by design. If you want to copy a formatted code block, use your keyboard or mouse to select and copy the example.

## Understand syntax differences when using variables

The syntax for using variables varies slightly between environments.

| Environment | Variable syntax | Example
| - | - |
| Bash | variableName=variableValue | varResourceGroup=msdocs-rg-123
| PowerShell | $variableName=variableValue | $varResourceGroup=msdocs-rg-123
| Cmd | set variableName=variableValue | set varResourceGroup=msdocs-rg-123

There are several different ways to return variable information to your console screen in each environment, but `echo` works in most circumstances.  Here is a comparison:

* Bash: `echo $varResourceGroup`
* PowerShell: `echo $varResourceGroup`
* Cmd: `echo %varResourceGroup%`

Learn tips and see in-depth examples for variable syntax in Bash and PowerShell when you work through tutorial step three, [Use variables in commands](./get-started-tutorial-3-use-variables.md).

## Learn about quoting differences between environments

Every Azure CLI parameter is a string. However, each environment has its own rules for handling single and double quotes, spaces and parameter names.

|String value|Azure CLI|PowerShell|Cmd
|-|-|-|-|
|Text|'abc' or "abc"|'abc' or "abc"|"abc"
|Number|\\\`50\\`|\`\`50\`\`|\`50\`
|Boolean|\\\`true\\`|\`\`false\`\`|\'true\'
|Date|'2021-11-15'|'2021-11-15'|'2021-11-15'
|JSON|'{"key":"value"}' or "{\"key\":\"value\"}" |'{"key":"value"}'|"{\"key\":\"value\"}"
|Variable name/create|variableName=|$variableName=| set variableName=
|Variable as parameter value |variableName|$variableName|%variableName%
|Variable in `--query` string|'$variableName'|'$variableName'|'$variableName'

Many Azure CLI parameters accept a space-separated list of values. This impacts quoting.

* Unquoted space-separated list: `--parameterName firstValue secondValue`
* Quoted space-separated list: `--parameterName "firstValue" "secondValue"`
* Passing values that contain a space: `--parameterName "value1a value1b" "value2a value2b" "value3"`

If you aren't sure how your string will be evaluated by your environment, return the value of a string to your console or use `--debug` as explained in [Debug and error handling](#use---debug-parameter).

## Create a storage account to apply what you've learned

The remainder of this tutorial demonstrates quoting rules in Azure CLI commands. Create an Azure storage account to use for these examples.

These examples use the resource group created in [Prepare your environment for the Azure CLI](./get-started-tutorial-1-prepare-environment.md) and demonstrates syntax differences for the following:

* Line continuation
* Variable usage
* Random identifiers
* `echo` command

# [Bash](#tab/bash)

```azurecli-interactive
# Variable block
let "randomIdentifier=$RANDOM*$RANDOM"
location=eastus
resourceGroup="msdocs-tutorial-rg-00000000"
storageAccount="msdocssa$randomIdentifier"

# Create a storage account.
echo "Creating storage account $storageAccount in resource group $resourceGroup"
az storage account create --name $storageAccount \
                          --resource-group $resourceGroup \
                          --location $location \
                          --sku Standard_RAGRS \
                          --kind StorageV2 \
                          --output json
```

# [PowerShell](#tab/powershell)

```azurecli-interactive
# Variable block
let "randomIdentifier=$RANDOM*$RANDOM"
$location=eastus
$resourceGroup="msdocs-tutorial-rg-00000000"
$storageAccount="msdocssa$randomIdentifier"

# Create a storage account.
echo "Creating storage account $storageAccount in resource group $resourceGroup""
az storage account create --name $storageAccount `
                          --resource-group $resourceGroup `
                          --location $location `
                          --sku Standard_RAGRS `
                          --kind StorageV2 `
                          --output json
```

# [Cmd](#tab/cmd)

```azurecli-interactive
:: Variable block
set randomIdentifier=%RANDOM%
set resourceGroup="msdocs-tutorial-rg-00000000"
set location=eastus
set storageAccount="msdocssa%randomIdentifier%"

:: Create a storage account.
echo "Creating storage account %storageAccount% in resource group %resourceGroup%"
az storage account create --name %storageAccount% ^
                          --resource-group %resourceGroup% ^
                          --location %location% ^
                          --sku Standard_RAGRS ^
                          --kind StorageV2 ^
                          --output json
```

---

The Azure CLI returns at least 100 lines of JSON as output when a new storage account is created. The following JSON dictionary output has fields omitted for brevity.

```output
{
"accessTier": "Hot",
"allowBlobPublicAccess": false,
"creationTime": "yyyy-mm-ddT19:14:26.962501+00:00",
"enableHttpsTrafficOnly": true,
"id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ msdocs-tutorial-rg-00000000/providers/Microsoft.Storage/storageAccounts/msdocssa00000000",
"keyCreationTime": {
  "key1": "yyyy-mm-ddT19:14:27.103127+00:00",
  "key2": "yyyy-mm-ddT19:14:27.103127+00:00"
},
"kind": "StorageV2",
"location": "eastus",
"name": "msdocssa00000000",
"primaryEndpoints": {
  "blob": "https://msdocssa00000000.blob.core.windows.net/"
},
"primaryLocation": "eastus",
"provisioningState": "Succeeded",
"resourceGroup": "msdocs-tutorial-rg-00000000",
"sku": {
  "name": "Standard_RAGRS",
  "tier": "Standard"
},
"statusOfPrimary": "available",
"statusOfSecondary": "available",
"tags": {},
"type": "Microsoft.Storage/storageAccounts"
}
```

## Create tags to learn quoting differences

Using [az storage account update](/cli/azure/storage/account#az-storage-account-update), add tags to help you identify your storage account and learn about quoting differences. These examples demonstrate syntax differences for the following:

* Values containing spaces
* Quoting blank spaces
* Escaping special characters
* Using variables

The `--tags` parameter accepts a space-separated list of key:value pairs.

# [Bash](#tab/bash)

```azurecli-interactive
# Create new tags without quotes.
az storage account update --name <msdocssa00000000> \
                          --resource-group <msdocs-tutorial-rg-00000000> \
                          --tags Team=t1 Environment=e1

# Create new tags containing spaces.
az storage account update --name <msdocssa00000000> \
                          --resource-group <msdocs-tutorial-rg-00000000> \
                          --tags "Floor number=f1" "Cost center=cc1"

# Create a new tag with an empty value.
az storage account update --name <msdocssa00000000> \
                          --resource-group <msdocs-tutorial-rg-00000000> \
                          --tags "Department="''""

# Create a new tag containing special characters resulting in "Path": "$G:\\myPath".
az storage account update --name <msdocssa00000000> \
                          --resource-group <msdocs-tutorial-rg-00000000> \
                          --tags "Path=\$G:\myPath"

# Create a tag from a variable.
newTag="tag1=tag value with spaces"
az storage account update --name <msdocssa00000000> \
                          --resource-group <msdocs-tutorial-rg-00000000> \
                          --tags "$newTag"
```

If you don't want to overwrite previous tags while you work through this tutorial step, use the [az tag update](/cli/azure/tag#az-tag-update) command setting the `--operation` parameter to `merge`.

```azurecli-interactive
# Get the resource ID of your storage account.
saID=$(az resource show --resource-group <msdocs-tutorial-rg-00000000> \
                        --name <msdocssa00000000> \
                        --resource-type Microsoft.Storage/storageAccounts \
                        --query "id" \
                        --output tsv)

echo My storage account ID is $saID

# Append new tags.
az tag update --resource-id $saID \
              --operation merge \
              --tags <tagName>=<tagValue>

# Get a list of all tags.
az tag list --resource-id $saID
```

# [PowerShell](#tab/powershell)

```azurecli-interactive
# Create new tags without quotes.
az storage account update --name <msdocssa00000000> `
                          --resource-group <msdocs-tutorial-rg-00000000> `
                          --tags Team=t1 Environment=e1


# Create new tags containing spaces.
az storage account update --name <msdocssa00000000> `
                          --resource-group <msdocs-tutorial-rg-00000000> `
                          --tags "Floor number=f1" "Cost center=cc1"

# Create a new tag with an empty value.
az storage account update --name <msdocssa00000000> `
                          --resource-group <msdocs-tutorial-rg-00000000> `
                          --tags "Floor number="''""

# Create a new tag containing special characters resulting in "Path": "$G:\\myPath".
# Nate the backtick as both the line continuation and the PowerShell escape character.
az storage account update --name <msdocssa00000000> `
                          --resource-group <msdocs-tutorial-rg-00000000> `
                          --tags "Path=`$G:\myPath"

# Create a tag from a variable.
# In PowerShell, prefix your variable name with a dollar sign.
$newTag="tag1=tag value with spaces"
az storage account update --name <msdocssa00000000> `
                          --resource-group <msdocs-tutorial-rg-00000000> `
                          --tags "$newTag"
```

If you don't want to overwrite previous tags while you work through this tutorial step, use the [az tag update](/cli/azure/tag#az-tag-update) command setting the `--operation` parameter to `merge`.

```azurecli-interactive
# Get the resource ID of your storage account.
$saID=$(az resource show --resource-group <msdocs-tutorial-rg-00000000> `
                         --name <msdocssa00000000> `
                         --resource-type Microsoft.Storage/storageAccounts `
                         --query "id" `
                         --output tsv)

echo My storage account ID is $saID

# Append new tags.
az tag update --resource-id $saID `
              --operation merge `
              --tags <tagName>=<tagValue>

# Get a list of all tags.
az tag list --resource-id $saID
```

# [Cmd](#tab/cmd)

```azurecli-interactive
:: Create new tags.
az storage account update --name <msdocssa00000000> ^
                          --resource-group <msdocs-tutorial-rg-00000000> ^
                          --tags Team=t1 Environment=e1

:: Create new tags containing spaces.
az storage account update --name <msdocssa00000000> ^
                          --resource-group <msdocs-tutorial-rg-00000000> ^
                          --tags "Floor number=f1" "Cost center=cc1"

:: Create a new tag with an empty value.
az storage account update --name <msdocssa00000000> ^
                          --resource-group sdocs-tutorial-rg-00000000> ^
                          --tags "Floor number="''""

```

If you need to modify an Azure resource using a variable, we suggest using Bash. Cmd often does not interpret variable values with special characters as expected. You often receive an "InvalidCharacters" error or your tag value is empty.

---

## Compare more environment-specific scripts

Take a deeper look at these script differences. These examples demonstrate quoting differences in the following:

* Complex string parameters
* `--query` filtering
  * Numbers
  * Boolean values
  * Dates


# [Bash](#tab/bash)

Example of using double quotes within a complex parameter value.

```azurecli
az rest --method patch \
        --url https://management.azure.com/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.HybridCompute/machines/<machineName>?api-version=yyyy-mm-dd-preview \
        --resource https://management.azure.com/ \
        --headers Content-Type=application/json \
        --body '{"properties": {"agentUpgrade": {"enableAutomaticUpgrade": false}}}'
```

Example of filtering for a numeric value.

```azurecli
az vm list --resource-group <myResourceGroup> \
           --query "[?storageProfile.osDisk.diskSizeGb >=\`50\`].{Name:name,  admin:osProfile.adminUsername, DiskSize:storageProfile.osDisk.diskSizeGb }" \
           --output table
```

Example of filtering for a boolean value.

```azurecli
az storage account list --resource-group <myResourceGroup> \
    --query "[?allowBlobPublicAccess == \`true\`].id"
```

Examples of filtering for a date.

```azurecli
# include time
az storage account list --resource-group <myResourceGroup> \
    --query "[?creationTime >='2021-11-15T19:14:27.103127+00:00'].{saName:name, saID: id, sku: sku.name}"

# exclude time
az storage account list --resource-group <myResourceGroup> \
    --query "[?creationTime >='2021-11-15'].{saName:name, saID: id, sku: sku.name}"

# subtract days and use a variable
saDate=$(date +%F -d "-30days")
az storage account list --resource-group msdocs-tutorial-rg-00000000 \
    --query "[?creationTime >='$saDate'].{saName:name, saID: id, sku: sku.name}"
```

# [PowerShell](#tab/powershell)

Example of using double quotes within a complex parameter value.

```azurecli
az rest --method patch `
        --url https://management.azure.com/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.HybridCompute/machines/<machineName>?api-version=yyyy-mm-dd-preview `
        --resource https://management.azure.com/ `
        --headers Content-Type=application/json `
        --body '{\"properties\": {\"agentUpgrade\": {\"enableAutomaticUpgrade\": false}}}'
```

Example of filtering for a numeric value.

```azurecli
az vm list --resource-group <myResourceGroup> `
           --query "[?storageProfile.osDisk.diskSizeGb >=``50``].{Name:name,  admin:osProfile.adminUsername, DiskSize:storageProfile.osDisk.diskSizeGb }" `
           --output table
```

Example of filtering for a boolean value.

```azurecli
az storage account list --resource-group <myResourceGroup> `
                       --query "[?allowBlobPublicAccess == ``true``].id"
```

Examples of filtering for a date.

```azurecli
# include time
az storage account list --resource-group msdocs-tutorial-rg-00000000 `
    --query "[?creationTime >='2021-11-15T19:14:27.103127+00:00'].{saName:name, saID: id, sku: sku.name}"

# exclude time
az storage account list --resource-group msdocs-tutorial-rg-00000000 `
    --query "[?creationTime >='2021-11-15'].{saName:name, saID: id, sku: sku.name}"

# subtract days and use a variable
$saDate=$saDate.AddDays(-30).tostring("yyyy-mm-dd")
az storage account list --resource-group msdocs-tutorial-rg-00000000 `
    --query "[?creationTime >='$saDate'].{saName:name, saID: id, sku: sku.name}"
```

# [Cmd](#tab/cmd)

Example of quoting within a complex parameter value.

```azurecli-interactive
az vm list --resource-group QueryDemo ^
    --query "[?storageProfile.osDisk.diskSizeGb >=`50`].{Name:name,  admin:osProfile.adminUsername, DiskSize:storageProfile.osDisk.diskSizeGb }" ^
    --output table
```

Example of filtering for a numeric value.

```azurecli
az vm list --resource-group <myResourceGroup> `
           --query "[?storageProfile.osDisk.diskSizeGb >=`50`].{Name:name,  admin:osProfile.adminUsername, DiskSize:storageProfile.osDisk.diskSizeGb }" `
           --output table
```

Example of filtering for a boolean value.

```azurecli
az storage account list --resource-group msdocs-tutorial-rg-00000000 ^
    --query "[?allowBlobPublicAccess == `true`].id"
```

Examples of filtering for a date.

```azurecli
# include time
az vm list --resource-group DevEx-Data-Analysis2 ^
           --query "[?storageProfile.osDisk.diskSizeGb >=`50`].{Name:name,  admin:osProfile.adminUsername, DiskSize:storageProfile.osDisk.diskSizeGb }" ^
           --output table

az storage account list --resource-group msdocs-tutorial-rg-55276056 ^
    --query "[?creationTime >='2021-11-15T19:14:27.103127+00:00'].{saName:name, saID: id, sku: sku.name}"

# exclude time
az storage account list --resource-group msdocs-tutorial-rg-55276056 ^
    --query "[?creationTime >='2021-11-15'].{saName:name, saID: id, sku: sku.name}"

# subtract days and use a variable
saDate=$(date +%F -d "-30days")
az storage account list --resource-group msdocs-tutorial-rg-55276056 ^
    --query "[?creationTime >='$saDate'].{saName:name, saID: id, sku: sku.name}"
```

---

## Debug a reference command

### Use `--debug` parameter

The Azure CLI offers a `--debug` parameter that can be used with any command. Debug output is extensive, but it will give you more information on execution errors. Use the Bash `clear` command to remove console output between tests.

# [Bash](#tab/bash)

These examples reveal the actual arguments received by the Azure CLI in Python's syntax.

This example is **correct**.

```azurecli-interactive
az '{"key":"value"}' --debug
```

See what the Azure CLI is interpreting in the `Command arguments` line of the output.

```output
Command arguments: ['{"key":"value"}', '--debug']
```

This second example is also **correct**. Use the Bash `clear` command to remove console output between tests.

```azurecli-interactive
clear
az "{\"key\":\"value\"}" --debug
```

```output
Command arguments: ['{"key":"value"}', '--debug']
```

These next two examples are **incorrect** as quotes and spaces are interpreted by Bash.

|Incorrect format|Problem|Console output|
|-|-|-|
|az {"key":"value"} --debug |Missing single quotes or escape characters| Command arguments: ['{key:value}', '--debug']
|az {"key": "value"} --debug |Missing single quotes or escape characters, and contains extra space | Command arguments: ['{key:', 'value}', '--debug']

# [PowerShell](#tab/powershell)

This example is **correct** in both bash and PowerShell although the double quotes missing around the output `key:value` pair is a known issue in PowerShell.

```azurecli-interactive
az '{"key":"value"}' --debug
```

See what the Azure CLI is interpreting in the `Command arguments` line of the output.

```output
Command arguments: ['{key:value}', '--debug']
```

These examples are all **incorrect**. Use PowerShells's `cls` command to remove console output between tests.

|Incorrect format|Problem |Console output|
|-|-|-|
|az "{\\"key\\":\\"value\\"}" --debug | Contains escape characters | Command arguments: ['{\\', 'key\\:\\value\\}', '--debug']
|az {"key":"value"} --debug | Missing single quotes | Unexpected token ':"value"' in expression or statement.
|az {"key": "value"} --debug | Missing single quotes and contains an extra space | Error: Unexpected token ':' in expression or statement.

# [Cmd](#tab/cmd)

This example is **correct**.

```azurecli-interactive
az "{\"key\":\"value\"}" --debug
```

See what the Azure CLI is interpreting in the `Command arguments` line of the output.

```output
Command arguments: ['{"key":"value"}', '--debug']
```

These examples are all **incorrect**. Use the Cmd's `cls` command to remove console output between tests.

|Incorrect format|Problem |Console output|
|-|-|-|
|az "{"key":"value"}" --debug | Missing escape characters | Command arguments: ['{key:value}', '--debug']
|az '{"key":"value"}' --debug |Missing escape characters and is using single quotes where double quotes are expected.| Command arguments: ["'{key:value}'", '--debug']
|az {"key":"value"} --debug | Missing escape characters and double quotes | Command arguments: ['{key:value}', '--debug']

---

### Use `echo` command

Although `--debug` tells you exactly what the Azure CLI is interpreting, a second option is to return the value of an expression to your console. This method is very helpful when verifying the results of `--query` that is covered in detail in [Use variables in commands](./get-started-tutorial-3-use-variables.md).

# [Bash](#tab/bash)

```azurecli-interactive
strExpression='{"key":"value"}'
echo $strExpression
```

```output
{"key":"value"}
```

# [PowerShell](#tab/powershell)

```azurecli-interactive
$strExpression='{"key":"value"}'
echo $strExpression
```

```output
{"key":"value"}
```

# [Cmd](#tab/cmd)

In Cmd, the `echo` command returns the literal string including escape characters. Compare the `--debug` and `echo` output of Cmd:

|CLI|Output|
|-|-|
|az "{\"key\":\"value\"}" --debug | Command arguments: ['{"key":"value"}', '--debug']
|set strExpression='"{\"key\": \"value\"}"' |
| echo %strExpression% | "{\\"key\\": \\"value\\"}"

---

## Troubleshooting

There are common errors when an Azure CLI reference command is not written properly.

* "Bad request ...{something} is invalid" might be caused by a space, single or double quotation mark, or lack of a quote.

* "Unexpected token..." is seen when there is an extra space or quote.

* "Invalid jmespath_type value" error often comes from incorrect quoting in the `--query` parameter.

* "Unrecognized arguments" is often caused by an incorrect line continuation character.

* "Missing expression after unary operator" is seen when a line continuation character is missing.

* "Variable reference is not valid" is received when a string is not formatted properly often due to concatenation or a missing escape character.

## Get more detail

Do you want more detail on one of the topics covered in this tutorial step? Use the links in this table to learn more.

|Topic| Learn more|
|-|-|
|Scripting differences | [Bash quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)|
| | [PowerShell quoting](/powershell/module/microsoft.powershell.core/about/about_quoting_rules)|
| | [Quoting issues with PowerShell](https://github.com/Azure/azure-cli/blob/dev/doc/quoting-issues-with-powershell.md)
| | [Windows command-line tips](https://ss64.com/nt/syntax-esc.html)
| | [Use quotation marks in Azure CLI parameters](./use-cli-effectively.md#use-quotation-marks-in-parameters)
| | Compare syntax of Cmd, PowerShell and Bash in [Query command output using JMESPath](./query-azure-cli.md)

## Next Step

Now that you've learned how to modify parameter values for Bash, PowerShell and Cmd, proceed to the next step to learn how to extract values to a variable.

> [!div class="nextstepaction"]
> [Propagate variables for use in scripts](./get-started-tutorial-3-use-variables.md)