+++
date = '2026-03-05T15:17:45+01:00'
title = 'Identity and Access Management: Authentication and Authorization'
+++

Welcome to the first part of my short trilogy about the topic identity and access management. In this series, we will walk through the best practices in todays world of IT regarding the management of users and persisting user sessions securely. 

The first part will give you a general outlook of the landscape of authentication and authorization. You will gain fundamental knowledge that is required for the more complex topics. The second part will be about how the workflow looks like for authorizing a newly logged-in user, where we'll take a closer look at standards like OAuth and OpenID Connect (OIDC) to understand how they enable secure and seamless user access across applications. Passkeys will also be a very interesting topic here, so stay tuned. And in the third and last part, a hands-on tutorial will guide us on how to securely persist a session in Asp.Net Core using Json Web Tokens (JWT) inside a cookie.

# Authentication vs Authorization - What is the Difference?

The difference between authentication and authorization is best explained with a real life example. Imagine you want to log into your favourite social media platform. First, you'd have to log into your account. This step is called authorization, and here you are only proving that you are who you say you are. If the credentials match a database entry, then you are authenticated and may use the platform as a logged in user. The second step is authorization, and this is happening behind the scenes. Here gets decided what you are allowed to see. For example you can see other peoples posts, your own posts and even your private informations. But you can not see the private information of others, because you are not authorized.

In a nutshell, authentication decides who you are and authorization decides what you are allowed to do. Pretty easy, right? Now let's take a look on the different techniques & standards for authentication and authorization.

# Common Authentication Techniques & Standards

## Passwords

The first rule when it comes to authentication is to never ever save your passwords in a clear text format in you database. It is just way to insecure. If someone got access to your database, your users are all compromised. And if you are in Europe, you'll also get in trouble with the law because this practice is not DSGVO conform.

Instead, we are saving the passwords in a [hashed format](https://www.geeksforgeeks.org/dsa/introduction-to-hashing-2/). For anyone wondering what hashes do, they make it very computationally expensive to get the plain text password out of it. It is however very easy to check if a password matches a hash, so we won't have any problem when we want to check if the credentials are right. I like to compare them to fingerprints. They are very easy to identify, but basically impossible to recreate from scratch.

After you saved your passwords as hashes in the database, the only thing you have to do is to turn the entered password from the user in its hash equivalent and check if the two hashes match. If they do, the password is correct and the user is authenticated. You'd also want to use a service for this. Doing this completely from scratch is not necessary, since there are already tons of solutions for hashing passwords and storing users in a database.

## Multi-Factor Authentication

Multi-factor authentication (MFA) doubles the protection against hackers by not only checking your password, but also another point of knowledge that only you have access to. Most MFA authentication methodology is based on one of three types of additional information:

1. Things you know (knowledge), such as a password or PIN
2. Things you have (possession), such as a badge or smartphone
3. Things you are (inherence), such as a biometric like fingerprints or voice recognition

Personally, I hate this solution for one simple reason. It is a complete and utter waste of your time. Imagine having to pull up some app on your phone every time you want to log into your GitHub or online banking account. I really wonder how much time some people have wasted in their life just approving the MFA question. There are better solutions today, for example passwordless login. Nevertheless, every platform should have MFA, simply because not everyone uses passwordless authentication.

## Single Sign-On

Single sign-on (SSO) is another great invention that lets a user log in once and then access multiple applications or systems without having to log in again for each one.

Think about what you can access as soon as you log into your Google account. Mail, Calendar, Drive, YouTube, ... The list goes on and on. That's SSO.

Now you might think "Hm, so when I log into GitHub with my Google account, thats SSO, right?". Not quite. What you are using here is called OAuth 2.0, which is an authorization framework that only allows GitHub to use the data of your Google account. It is similar, but not the same.

## Directory-based Auth

There are some cases where instead of managing users for a web application, you manage them for internal systems, for virtual machines. Now you could enter each virtual machine via SSH and manage the users locally, but that is not very efficient. 

Instead it is common practice to have an Active Directory somewhere and the virtual machines connect and manage their users and groups via the Active Directory. Very often used in enterprise or infrastructure environments and compatible with Windows, MacOS and Linux.

# Common Authorization Techniques & Standards

After the user is authenticated, there are different approaches on authorization. We will cover the four most common and foundational authorization models:

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

In this model, access is granted or denied **based on a set of predetermined rules** that involves individual user criteria or conditions. Implementation includes identifying user-specific needs, creating [identity and access management (IAM])(https://www.redhat.com/en/topics/security/what-identity-and-access-management-iam) policies that define the rules for these needs, and applying conditions to enhance security. Rules are typically based on conditions such as time of access, IP address, and multi-factor authentication. For example, access may be granted only during business hours, if the user is securely logged into a Virtual Private Network (VPN), or in on-premise applications only.

## Attribute-based Access Control (ABAC)

ABAC is **based on user, resource, and environment attributes** and evaluates these multiple characteristics simultaneously. For example, when a user provides credentials to log into a cloud service, the system will retrive the user's attributes - such as their roles or permissions - from their profile. Then, it will identify the resource attributes such as file type, ownership, and sensitivity with contextual attributes such as time of access, user location, and if the user is requesting access on a secure network. The system will then evaluate whether the combined attributes meet the security policies and make a decision to grant or deny access.

## Mandatory-based Access Control (MAC)

This security technique enforces access restrictions **based on fixed mandatory policies and rules set by system admins** where permissions can't be modified by users. Disabling user access modifications ensures that sensitive information is protected according to non-negotiable security protocols. In this system, each user is assigned a predetermined security level and resources are classified by its level of sensitivity. Access is granted or denied depending on whether a user's security level matches or surpasses the resource's classification level.

# Conclusion

While the concept of authentication and authorization does seem pretty simple at first, there are just so many techniques for implementation that one can quickly lose sight, but I think the blog gives a pretty good outline on how it works. Let's wrap this up. 

We learned that authentication is responsible for proving the identity of the user. There are many things to consider when authenticating a user. The most important one is to store hashed passwords in your database instead of plain text. We also learned about the usefulness of MFA and SSO, and we learned how we can authenticate a multiple of users centrally in an enterprise environment using directory-based authentication.

On the other hand, there is authorization for checking if the user should be allowed to access the requested resource. We covered four common authorization techniques. While they do seem similar at first, they have minor differences and sometimes, you really do want to use something different than RBAC. But for most use-cases, I'd stick with RBAC.

In the next part, we will go deeper into OAuth and OICD, two technologies that revolutionized the way how users can log into their accounts.
