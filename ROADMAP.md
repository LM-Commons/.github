# LM-Commons Roadmap

This file contains the roadmap for the LM-Commons components and for the organization itself.

## Coding Standards

Since the LM-Commons components are based on Laminas and Mezzio components, LM-Commons components will use the [Laminas
Coding standards](https://github.com/laminas/laminas-coding-standard).

## Continuous Integration

All components must have continuous integration testing set up using GitHub workflows.

Tools used:
- phpunit for testing
- squizlabs/php_codesniffer for coding standards
- vimeo/psalm for static analysis

In addition, where possible, [Laminas Continuous Integration Action](https://github.com/laminas/laminas-continuous-integration-action) will be used.

## Namespace

All components shall use a namespace that starts with `Lmc` followed by the component name. This is to add consistency among Lmc components.
All changes in namespace will be the subject of a major release of the component due to the breaking changes they cause for users.

For example, LmcRbac shall use the `Lmc/Rbac` namespace. LmcRbacMvc shall use `Lmc/Rbac/Mvc`. 

## Starter kits

Started kits shall be provided that start from the Mezzio Skeleton that will incorporate
the Lmc components to provide a starting point the user management, access control and admin modules.

This is work in progress

## LmcUser and user management

LmcUser for Laminas MVC is now in Maintenance-only mode and no further features will be developed. This also applies to LmcUserDoctrineORM and LmcUserMongoODM.

A LmcUser version for Mezzio will be developed.

## LmcRbac and LmcRbacMvc access control

LmcRbacMvc and LmcRbacMvcDeveloperTools for Laminas MVC are now in Maintenance-only mode and no further features will be developed. 

## LmcMail

This package is abandoned. It is a MVC based package with little adoption by users which also relies on the discontinued laminas-mail.

## LmcCors

LmcCors for Laminas MVC is now in Maintenance-only mode and no further features will be developed. 

## LmcAdmin

LmcAdmin for Laminas MVC is now in Maintenance-only mode and no further features will be developed.

A LmcAdmin package for Mezzio is likely to be developed.
