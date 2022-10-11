Tags: #security 
Created: 2022-10-11 11:10
References: 

# Create Keycloak User Account via the REST API

This note describes the flow of creating a new user account via [[Keycloak]]'s [[RESTful API]] and add client role mappings to that user.

First off, you need to have the token of a role that has the required ream management permissions. If the token does not have permissions for doing the current operation it is trying to do, then the request will fail.

Once you have the token, you need to use it to make the requests rescribed in the following steps:
1. Create a new user with a `POST {keycloak_base}/auth/admin/realms/{realm}/users` with this payload structure:

```js
{ 
	email: 'emailString', 
	emailVerified: true, 
	enabled: true, 
	firstName: 'firstnameString', 
	lastName: 'lastnameString', 
	username: 'usernameString', 
	credentials: [
		{
			temporary: false, 
			value: 'passwordString' 
		}, 
		...
	] 
}
```

**Note:** `realm` is the name of the realm.

2. Find the *id of the client* by doing a `GET {keycloak_base}/auth/admin/realms/{realm}/clients` and filtering for your client.
3. Find the *ids of the client roles* you want the user to have by doing a `{keycloak_base}/auth/admin/realms/{realm}/clients/{id}/roles` and searching for the roles you need.
4. Once you created the user, you need to add the roles it has (in the case of client roles). You can do this with a `POST {keycloak_base}/auth/admin/realms/{realm}/users/{id}/role-mappings/clients/{client}` with this payload structure:

```js
[
	{
		id: 'roleId',
		name: 'roleName'
	},
	...
]
```

**Note:** `realm` is the name of the realm, `id` is the id of the user to which you want to add the roles, `client` is the id of the client in which the roles exist.