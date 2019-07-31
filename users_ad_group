Import-Module activedirectory  
$ADUsers = Import-csv C:\Users\Administrator\Desktop\Useradd\users.csv
foreach ($User in $ADUsers)
{
		
	$Username 	= $User.username
	$Password 	= $User.password
	$Firstname 	= $User.firstname
	$Lastname 	= $User.lastname
	$OU 		= $User.ou 

	if (Get-ADUser -F {SamAccountName -eq $Username})
	{
		 Write-Warning "A user account with username $Username already exist in Active Directory."
	}
	else
	{
		#User does not exist then proceed to create the new user account
        #Account will be created in the OU provided by the $OU variable read from the CSV file
		New-ADUser `
            -SamAccountName $Username `
            -UserPrincipalName "$Username@domain.local" `
            -Name "$Firstname $Lastname" `
            -GivenName $Firstname `
            -Surname $Lastname `
            -Enabled $True `
            -DisplayName "$Lastname, $Firstname" `
            -Path $OU `
            -AccountPassword (convertto-securestring $Password -AsPlainText -Force) -ChangePasswordAtLogon $False

        #Account will be added to VPN group
        Add-ADGroupMember -Identity "OpenVPN" -Members $Username
        Write-Output "Successful!"
            
	}
}
