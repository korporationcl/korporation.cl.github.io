# Cognito

- Federation allows users to authenticate with Web Identity provider (Google, Facebook, Amazon).
- User authenticates first with the Web ID provider and receives an authentication token, which is exchanged for a temporary
  AWS credentials allowing them to assume an IAM role.
- Cognito is an Identity broker which handles interaction between your applications and Web ID provider (you don't need to write your own code)
- User pools is user based; It handles things like user registration, authentication and account recovery.
- Identity pools authorise access to AWS resources.
