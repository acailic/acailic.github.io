---
title: AWS Cognito
layout: post
tags: [aws, cda, cognito]
date: 2020-09-17
---
## Amazon Cognito
- We want to give our users an identity so that they can interact with our
application. like third party
- Cognito User Pools:
• Sign in functionality for app users
• Integrate with API Gateway & Application Load Balancer
- Cognito Identity Pools (Federated Identity):
• Provide AWS credentials to users so they can access AWS resources directly
• Integrate with Cognito User Pools as an identity provider
- Cognito Sync:
• Synchronize data from device to Cognito.
• Is deprecated and replaced by AppSync
- Cognito vs IAM: “hundreds of users”, ”mobile users”, “authenticate with SAML”

### Cognito User Pools (CUP) – User Features
- Create a serverless database of user for your web & mobile apps
- Simple login: Username (or email) / password combination
- Password reset
- Email & Phone Number Verification
- Multi-factor authentication (MFA)
- Federated Identities: users from Facebook, Google, SAML…
- Feature: block users if their credentials are compromised elsewhere
- Login sends back a JSON Web Token (JWT)
### Cognito User Pools (CUP) - Integrations
- CUP integrates with API Gateway and Application Load Balancer
### Cognito User Pools – Lambda Triggers
- User Pool Flow Operation Description
- Authentication Events
- Pre Authentication Lambda Trigger Custom validation to accept or deny the sign-in request
- Post Authentication Lambda Trigger Event logging for custom analytics
- Pre Token Generation Lambda Trigger Augment or suppress token claims
- Sign-Up 
- Pre Sign-up Lambda Trigger Custom validation to accept or deny the sign-up
request
- Post Confirmation Lambda Trigger Custom welcome messages or event logging for
custom analytics
- Migrate User Lambda Trigger Migrate a user from an existing user directory to user
pools
- Messages Custom Message Lambda Trigger Advanced customization and localization of messages
- Token Creation Pre Token Generation Lambda Trigger Add or remove attributes in Id tokens
### Cognito User Pools – Hosted Authentication UI
- Cognito has a hosted
authentication UI that you can
add to your app to handle signup
and sign-in workflows
- Using the hosted UI, you have a
foundation for integration with
social logins, OIDC or SAML
- Can customize with a custom
logo and custom CSS
### Cognito Identity Pools (Federated Identities)
- Get identities for “users” so they obtain temporary AWS credentials
- Your identity pool (e.g identity source) can include:
• Public Providers (Login with Amazon, Facebook, Google, Apple)
• Users in an Amazon Cognito user pool
• OpenID Connect Providers & SAML Identity Providers
• Developer Authenticated Identities (custom login server)
• Cognito Identity Pools allow for unauthenticated (guest) access
- Users can then access AWS services directly or through API Gateway
• The IAM policies applied to the credentials are defined in Cognito
• They can be customized based on the user_id for fine grained control
### Cognito Identity Pools – Diagram Cognito Web
### Cognito Identity Pools – Diagram with CUP
### Cognito Identity Pools – IAM Roles
- Default IAM roles for authenticated and guest users
- Define rules to choose the role for each user based on the user’s ID
- You can partition your users’ access using policy variables
- IAM credentials are obtained by Cognito Identity Pools through STS
- The roles must have a “trust” policy of Cognito Identity Pools
###  Cognito User Pools vs Identity Pools
- Cognito User Pools:
• Database of users for your web and mobile application
• Allows to federate logins through Public Social, OIDC, SAML…
• Can customize the hosted UI for authentication (including the logo)]
• Has triggers with AWS Lambda during the authentication flow
- Cognito Identity Pools:
• Obtain AWS credentials for your users
• Users can login through Public Social, OIDC, SAML & Cognito User Pools
• Users can be unauthenticated (guests)
• Users are mapped to IAM roles & policies, can leverage policy variables
- CUP + CIP = manage user / password + access AWS services
### Cognito Sync
- Deprecated – use AWS AppSync now
- Store preferences, configuration, state of app
- Cross device synchronization (any platform – iOS, Android, etc…)
- Offline capability (synchronization when back online)
- Store data in datasets (up to 1MB), up to 20 datasets to synchronize
- Push Sync: silently notify across all devices when identity data changes
- Cognito Stream: stream data from Cognito into Kinesis
- Cognito Events: execute Lambda functions in response to events
### Questions
- You need to synchronize data offline between your mobile devices. You should use: Cognito sync
- You need your clients to log in with Twitter and directly interact with your DynamoDB tables. You should use:Cognito Identity Pools
- You would like to provide a Facebook login before your users call your API hosted by API Gateway. You need seamlessly authentication integration, you will use:Cognito User Pools
- You would like to store the users that have successfully logged in to Cognito in RDS. What should you do?: Write a Post-Authentication hook with Lambda
- What can you NOT customize in the Cognito hosted UI?:The underlying Javascript

