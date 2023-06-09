# sharepointuserprovisioning
Powershell Script to automate user account provisioning in sharepoint

# Function to create a new user account in SharePoint
Function New-SharePointUser {
    param(
        [Parameter(Mandatory=$true)]
        [string]$Username,
        
        [Parameter(Mandatory=$true)]
        [string]$Email,
        
        [Parameter(Mandatory=$true)]
        [string]$Department,
        
        [Parameter(Mandatory=$true)]
        [string]$Permissions
    )
    
    # Connect to the SharePoint site
    Connect-PnPOnline -Url "https://your-sharepoint-site-url.com" -UseWebLogin
    
    # Create the user
    $user = New-PnPUser -LoginName $Username -Email $Email -Title $Username -SendEmails $false
    
    # Set the user's department
    $user.UserProfileProperties["Department"].Value = $Department
    $user.Update()
    
    # Grant permissions to the user
    Set-PnPListItemPermission -List "YourListName" -Identity 1 -User $Username -AddRole $Permissions
}

# Prompt for user details
$Username = Read-Host -Prompt "Enter the username"
$Email = Read-Host -Prompt "Enter the user's email address"
$Department = Read-Host -Prompt "Enter the department"
$Permissions = Read-Host -Prompt "Enter the permissions (e.g., Contribute)"

# Call the function to create the user
New-SharePointUser -Username $Username -Email $Email -Department $Department -Permissions $Permissions

