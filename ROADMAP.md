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

Started kits shall be provided that start from the Laminas Skeleton and from the Mezzio Skeleton that will incorporate
the Lmc components to provide a starting point the user management, access control and admin modules.

This is work in progress

## LmcUser and user management

Roadmap to come

## LmcRbac and LmcRbacMvc access control

Roadmap to come

## LmcMail

Roadmap to come

## LmcCors

Roadmap to come

## LmcAdmin

Roadmap to come
