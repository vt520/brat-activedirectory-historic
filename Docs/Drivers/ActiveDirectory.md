﻿# Brat - Active Directory Driver

## Scriptlet API

### *ActiveDirectoryUser*[] **AD_GetActiveUsers** ( )

Returns a list of all Current Users in Active Directory using the following LDAP Criteria
```
	objectCategory: User
	objectClass: person
	userAccountControl: ! 0x02
	userPrincipalName: ! null
	homeDirectory: ! null
```

Sample Usage
```javascript
	let CurrentUsers = AD_GetActiveUsers();
	CurrentUsers.forEach(user => {
		if(user.Location == "Watsonville") { ... }
	});
```

### *ActiveDirectoryUser* **AD_GetUser** ( *identifier* )
Returns a single user record, if and only if, the provided identifier maps to a single LDAP result.  Current resolution order is by checking the following LDAP attrbutes in order for equivilance: distinguishedName, userPrincipalName, objectSid, mail, name

Sample Usage
```javascript
	let byEmail = AD_GetUser("gulam@energyservices.org");
	let byLdap = AD_GetUser("CN=Mark Roduner,OU=Management,OU=Accounts,DC=energyservices,DC=org");
	let byName = AD_GetUser("Patrice Roduner");
	let notEnough = AD_GetUser("Mark");

	LogMessage("AD User Info", LogFlags.Important, {
		EmailResult: byEmail,
		LdapResult: byLdap,
		NameResult: byName,
		failureResult: notEnough
	});

```

## *ActiveDirectoryGroup*[] **AD_GetGroups** ( )

Returns an array of all groups in the Active Directory

Sample Usage
```javascript
	let CurrentGroups = AD_GetGroups();
	CurrentGroups.forEach(group => {
		if(group.Memebers.Count == 0) { ... }
	});
```

### *ActiveDirectoryGroup* **AD_GetGroup** ( *identifier* ) 
Returns a single group record, if and only if, the provided identifier maps to a single LDAP result.  Current resolution order is by checking the following LDAP attrbutes in order for equivilance: distinguishedName, name, objectSid
Sample Usage
```javascript
	let Administrators = AD_GetGroup("Administrators");
	Administrators.Members.forEach(ldapName => {
		let user = AD_GetUser(ldapName);
		let boxUser = box.UserDetails(user.Email);
		...
	});
```