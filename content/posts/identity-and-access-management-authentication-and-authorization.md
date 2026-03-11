+++
date = '2025-10-05T15:17:45+01:00'
title = 'Identity and Access Management: Authentication and Authorization'
+++
Welcome to the first part of my short trilogy about the topic of identity and access management (IAM). In this series, we’ll look at the best ways to manage users. We’ll also discuss how to handle secure user sessions in today’s IT world. 

The first part gives an overview. It covers authentication and authorization. You will gain the fundamental knowledge required for the more complex topics. 

The second part will be about how the workflow looks for authorizing users. We’ll look at how to enable secure user authorization. We’ll use standards like OAuth and OpenID Connect (OIDC). Passkeys will also be a very interesting topic here, so stay tuned. 

In the last part, we’ll have a hands-on tutorial. It will show us how to keep a secure user session in ASP.NET Core. We’ll use JSON Web Tokens (JWT) in a cookie for this.

# Authentication vs. Authorization - What is the Difference?

The difference between authentication and authorization is best explained with a real-life example. Imagine you want to log into your favorite social media platform. The first thing you'd have to do is authenticate yourself. Here, the only thing you are proving is that you are who you say you are. If your credentials match a database entry, authentication is successful. 

The second step is authorization, and this is happening behind the scenes. Here it gets decided what you may or may not do in the application. You can see other people's posts, your own posts, and even your private info. But you cannot see the private information of others because you are not authorized to do that.

Authentication tells who you are. Authorization tells what you can do. Pretty easy, right? Let's look at the different techniques for authentication. We will also cover the standards for authorization.

# Common Authentication Techniques & Standards

## Passwords

Most important rule in IAM: never save your passwords in clear text in your database. It is way too insecure, and you'll get in trouble. If someone gets access to your database, your users are all compromised. And if you are in Europe, you'll also get in trouble with the law because this practice is not GDPR compliant.

We keep passwords in a hashed format. Hashes make it costly to retrieve the plaintext password from them. It’s easy to check if a password matches a hash. So, we won’t have any trouble verifying the credentials. I like to compare them to fingerprints. They are very easy to identify but impossible to recreate from scratch.

Once you save passwords as hashes in the database, convert the user’s entered password into a hash. Then, check if the two hashes match. If they do, the password is correct. You would also want to use a service for this. Doing this completely from scratch is not necessary. Many solutions exist for hashing passwords and storing users in a database. There is no need to reinvent the wheel here.

## Multi-Factor Authentication

Multi-factor authentication (MFA) adds extra security. It helps protect against hackers by requiring more than just a password. Most MFA methods rely on one of three types of extra information:

1. Things you know (knowledge), such as a password or PIN.

2. Things you have (possession), such as a badge or a smartphone.

3. Things you are (inherent), such as biometrics, like fingerprints or voice recognition.

Some users find this solution cumbersome for one simple reason. It is a complete and utter waste of your time. Picture this: you need to open an app on your phone each time you log into GitHub or your online banking. How much time have some people wasted in their lives just approving the MFA question? There are better solutions today; for example, passwordless login. Still, every platform should have MFA, simply because not everyone uses passwordless authentication.

## Single Sign-On

Single sign-on (SSO) is a useful tool. It allows a user to log in once. They can then access many apps or systems without logging in each time.

Think about what you can access as soon as you log into your Google account: Mail, Calendar, Drive, YouTube, ... The list goes on and on. That's SSO.

Now you might think "Hm, so when I log into GitHub with my Google account, thats SSO, right?". Not quite. You're using OAuth 2.0. It’s an authorization framework. It lets GitHub access your Google account data. It is similar, but not the same.

## Directory-based Auth

In some cases, you manage users for internal systems instead of a web application. This includes managing virtual machines. You can enter each virtual machine via SSH to manage users locally, but it is not efficient. 

It's common to have an Active Directory. Virtual machines connect to it to manage users and groups. Used in enterprise or infrastructure settings, it works with Windows, macOS, and Linux.

# Common Authorization Techniques & Standards

After the user is authenticated, there are different approaches to authorization. We will cover the four most common and foundational authorization models:

- Role-Based Access Control

- Rule-Based Access Control

- Attribute-Based Access Control

- Mandatory-Based Access Control

## Role-based Access Control

Role-Based Access Control (RBAC) is the most used method for managing user access to systems and resources based on their role within a team or larger organization. At its core, every role-based access control system follows the same basic principles:

- Each user is assigned one or more roles.

- User roles are assigned permissions.

- Users gain access to permissions by being active members of a role.

In many cases, RBAC models establish a role hierarchy, in which the role structure resembles the hierarchy of the organization and may include roles for administrators, end users, and guests, and any specialized group in between. Some role hierarchies may be inheritance hierarchies, where more senior user roles are automatically grated the roles beneath them along with their privileges. In other cases, the hierarchy may be arbitrary and users granted a senior role do not necessarily inherit descendent roles by default.

## Rule-based Access Control (RuBAC)

In this model, access is granted or denied based on a set of predetermined rules that involves individual user criteria or conditions. Implementation includes identifying user-specific needs, creating [identity and access management (IAM])(https://www.redhat.com/en/topics/security/what-identity-and-access-management-iam) policies that define the rules for these needs, and applying conditions to enhance security. Rules are typically based on conditions such as time of access, IP address, and multi-factor authentication. For example, access may be granted only during business hours, if the user is securely logged into a Virtual Private Network (VPN), or in on-premise applications only.

## Attribute-based Access Control (ABAC)

ABAC is based on user, resource, and environment attributes and evaluates these multiple characteristics simultaneously. For example, when a user provides credentials to log into a cloud service, the system will retrive the user's attributes - such as their roles or permissions - from their profile. Then, it will identify the resource attributes such as file type, ownership, and sensitivity with contextual attributes such as time of access, user location, and if the user is requesting access on a secure network. The system will then evaluate whether the combined attributes meet the security policies and make a decision to grant or deny access.

## Mandatory-based Access Control (MAC)

This security technique enforces access restrictions based on fixed mandatory policies and rules set by system admins where permissions can't be modified by users. Disabling user access modifications ensures that sensitive information is protected according to non-negotiable security protocols. In this system, each user is assigned a predetermined security level and resources are classified by its level of sensitivity. Access is granted or denied depending on whether a user's security level matches or surpasses the resource's classification level.

# Conclusion

Here’s a revised version of your text:

First, authentication proves the user's identity. It’s crucial to store hashed passwords in your database, not plain text. We also learned that MFA and SSO are helpful. They allow us to verify many users at once using directory-based authentication.

Next, authorization checks if a user can access a resource. We discussed four common authorization techniques. While they may look similar, each has unique differences. Sometimes, you may need something other than RBAC. However, for most cases, I recommend sticking with RBAC.

In the next part, we’ll dive deeper into OAuth and OIDC. These technologies have changed how users log into their accounts.
