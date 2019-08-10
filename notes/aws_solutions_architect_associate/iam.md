# Identity access management (IAM)

- Centralised control of your AWS account
- Shared access
- Granular permissions
- Identity Federation (LDAP, Facebook, Linkedin, etc)
- Multi factor Authentication (MFA)
- Temporary access to users
- Implement Password policies
- PCI/DSS Compliance

# Key items

- users
- groups: collection of users
- policies: are made of documents called Policy documents. The format is JSON.
- roles: you create roles that have policies and then are asign to particular resources.

# Exam tips

- IAM is global, does not apply per region.
- root account is the first account (keep it safe, of course)
- New users have no permissions by default
- New users are assigned access key id and secret keys (API) when created
- Always set MFA on the root account
- You can customised password policy