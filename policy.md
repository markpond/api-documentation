## Policy

**Disclaimer**: the API is currently in beta and liable to change at any time. And so these policies may be edited at any time without notice as features are added and needs change.

### Terms of Implementation

Apps (web, native, or otherwise) must never store the user's password. Passwords can only be collected for authorisation with the native app workflow, and then must be discarded immediately. Apps must also make a reasonable effort to make sure passwords are kept securely.

Apps should not add bookmarks to users' accounts without the users' specific permission.

Markpond reserves the right to revoke an applications API tokens either temporarily or permanently for 

### Reasonable Constraints

Apps must not swamp Markpond with requests. At the moment there is no rate limiting in place, but one will be put in place if necessary. For now, please don't request the API more than once a second.

### Spam

Apps must not use the API to spam anyone or anything in any way. Markpond reserves the right to deem whether or not an application's behaviour is considered spam, and to terminate or revoke the apps API tokens either temporarily or permanently.