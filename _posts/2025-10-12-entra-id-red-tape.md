---
layout: post
title: Cutting Through the Red Tape: Entra, Managed Identities, and RBAC in the Real World
author: Emir Taletovic
---

If you've been building in Azure lately, you've probably noticed the same thing I have:  
every Microsoft document, quickstart, or architecture guide starts with the same mantra:

*"Use Entra for identity. Use managed identities for your apps."*

And honestly, it makes perfect sense. Managed identities are secure, elegant, and remove the need to deal with messy secrets or connection strings. I love that part. On paper, it's one of those rare moments where security and developer experience actually align. But here's the thing. The moment you try to use it in a real organization, you hit a wall. A big one. And it's called RBAC permissions.

## The Promise That Got Everyone Excited

Let's start with the good stuff. Microsoft's pitch is simple and strong: managed identities are the modern, secure way to connect Azure resources without credentials. They come with a ton of benefits:

* You don't have to paste secrets or keys into config files.
* Azure SDKs and services (like Functions, App Service, or Logic Apps) already support them.
* Security teams get centralized identity management and clean audit trails.

When I first started using managed identities, I was all in. It felt like the right way to build cloud apps. And it is, at least in theory.

## Where It Starts to Fall Apart

The problem isn't really with Entra or managed identities. It's with **RBAC permissions** and how organizations handle them.

Here's what I keep running into: subscription admins use Entra to manage all employee access across Azure. They control who can sign in, what roles exist, and how permissions flow through the hierarchy. And from their perspective, that's how it should be -- centralized and tightly governed.

The challenge comes when developers need to assign roles for managed identities. To let a Function App talk to Cosmos DB or Key Vault, someone has to create a role assignment. That requires RBAC permissions, even if scoped to a single resource group.

Now, technically, that's fine. Azure absolutely allows you to scope permissions narrowly. You can give developers the ability to manage access just within their resource group, without touching anything else.  
But in practice, that's rarely how it goes.

Most organizations don't want to deal with the nuance. Whether it's lack of understanding, time, or just risk aversion, the easier path is to say, *"no one outside the admin team touches RBAC."* So even though Microsoft built the system to be flexible and safe, companies tend to use it in the most restrictive way possible.

The result is predictable: every role assignment turns into a ticket, every ticket turns into a delay, and developers lose autonomy over something that should have been routine.

## The Awkward Developer Dilemma

This is the moment where I usually stop and think, *"Wait a second... wasn't this supposed to be easier?"*

I've done everything right:

* Followed Microsoft's recommendations.
* Used managed identities instead of secrets.
* Scoped access tightly to a single resource.

And yet, I'm blocked. I can't even test my code because someone in another team needs to approve a role assignment.

At that point, I completely understand why some developers give up and just drop a connection string in local.settings.json. It's not ideal, but it works. The irony is that the modern and secure way ends up being slower and harder to use than the old and insecure one.

## Why It's Not Just a Developer Problem

This friction isn't just an annoyance. It's a symptom of something deeper.

Many organizations proudly say they're adopting cloud-native best practices. They invest in Entra, talk about Zero Trust, and send out slide decks about identity-first architectures. But when it comes to actually giving developers the tools to make that happen, the governance model hasn't caught up.

Here's what that misalignment looks like in practice:

* Security teams overcorrect and lock everything down.
* Developers lose the ability to move quickly.
* The whole point of managed identities, simplicity and security, gets lost in process.

The result? Frustration on both sides and slower delivery for everyone.

## Finding a Better Balance

I'm not saying every developer should have unrestricted access to Azure RBAC. But there has to be a middle ground that recognizes developers need some autonomy to build securely without endless gatekeeping.

Here are a few things that I've seen work well:

* **Trust scoped roles.** Giving developers RBAC permissions scoped to a single resource group is usually safe and hugely empowering.
* **Shift permissions into pipelines.** Instead of giving individual developers elevated access, use CI/CD pipelines with a Service Connection that has the necessary roles assigned. This way, all infrastructure changes go through code review and pull requests, providing full auditability and traceability.
* **Educate both sides.** Developers need to understand why RBAC exists, and security teams need to see how lack of access drives people to bad practices.

When teams find that balance, things just flow. Developers can use managed identities as intended, and security teams still sleep at night.

## Wrapping It Up

Microsoft has made their position clear: **Entra and managed identities are the future.**

They're secure, scalable, and developer-friendly, but only if organizations make it possible to use them.

If your developers have to file a ticket every time they need to assign a role to a managed identity, you're not really embracing the cloud-native model. You're recreating the old on-prem change-control system in Azure.

So if you're in a position to influence how your organization handles RBAC, ask this question:  
*Are our processes helping developers build securely, or just slowing them down?*

Because at the end of the day, modern identity management isn't just about technology.  
It's about trust, collaboration, and removing enough red tape so everyone can actually do their job.