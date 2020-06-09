---
linktitle: Infrastructure - Principle of Least Privilege
title: Infrastructure - Principle of Least Privilege
date: 2020-06-09T17:00:00-04:00
author: "Sean Alexander"
categories: [ "Architecture", "Best Practices" ]
tags: ["architecture", "microservices", "security", "scale"]
weight: 10
draft: false
---

## Infrastructure - A Service Cannot

* Create mutable or shared resources.
* Drop mutable or shared resources.
* Alter mutable or shared resources.

## Infrastructure - A Service must

On Start up:

* Have all resource requirements defined
* Verify all required resources are accessible.
* If possible, verify RBAC roles are configured.
* Alert and fail if resources are not available or accessible.

## Data - A Service must

Release / Updates

* Perform any Data Updates (Migrations) outside of the application logic
* Create compatible or versioned migrations.
* On Startup, verify the the schema version is compatible

### Applying Migrations in Production

#### EF Core

https://docs.microsoft.com/en-us/aspnet/core/data/ef-rp/migrations?view=aspnetcore-2.1&tabs=visual-studio#applying-migrations-in-production

Microsoft recommends the following best practices:

* Production apps do not call "Database.Migrate" at application startup.
* Migrate should not be called from an app in a server farm configuration (more than 1 instance running)
* Data Migrations should be completed as part of a Deployment
* If a Data Migration fails, the rest of the pipeline should not proceed
* Use tools such as: ```dotnet ef database update```
* Automatically create Migration Artifacts SQL Scripts per release

For a multi-tenant architecture, creating artifacts for deployment is the preferred method, as the results will be consistent.  If there are any "snowflakes" in the data patterns, those should be treated independantly.
