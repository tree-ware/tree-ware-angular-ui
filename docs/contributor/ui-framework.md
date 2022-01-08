---
layout: default
title: UI Framework
nav_order: 1
parent: Contributor Docs
---

# Introduction

Tree-ware automates many UI features and development tasks with a custom UI framework and wrappers around regular UI
components.

Users only need to specify which node in the model tree a UI component corresponds to and the tree-ware UI framework
will automate the following:

1. Getting data from the backend and displaying it in the UI
2. Tracking changes
3. Allowing users to review changes
4. Validating user input
5. Tracking errors
6. Allowing users to review errors
7. Sending changes to the backend
8. Displaying backend validation errors down to the component level
9. Controlling Access
10. Defining access control
11. Displaying the UI as a dialog or as a wizard
12. Updating the UI whenever the data changes on the backend

# Tree-Ware API

A quick high-level recap of the tree-ware API: the tree-ware API allows clients to get or set any number of nodes in the
model tree. To get model data, you build a get-request model tree that specifies the nodes to be returned in the
get-response model tree. To set model data, you build a set-request model tree that specifies the nodes and values to be
set.

Note that a node in the model tree can be identified by a path, and when you combine a collection of such paths, you end
up with a model tree.

# UI Path Bindings

When you create a tree-ware UI component, you can specify a model node path that is "bound" to that component. This path
is used by the tree-ware UI framework to fetch data for that UI component, to set data entered by the user in that UI
component, and other automated functionality.

# Component Lifecycle

When a page or a dialog is displayed for the first time, the UI framework walks through the component hierarchy, gathers
their model node paths, and builds a single get-request from those model node paths. The framework issues this single
get-request to get all the data needed by the page or dialog. Once the response arrives, the framework breaks down the
response model tree into individual node paths and distributes them to the components that the paths are bound to.

When the user clicks the save or apply button, the framework does the same thing. It gathers model node paths and values
from components with changes, and builds a single set-request from those model node paths and values. The framework
issues this single set-request. If there is an error response, the framework breaks down the error model tree into
individual node paths and distributes them to the components that the paths are bound to. The components then display
those errors.

# Change Tracking

Each tree-ware component keeps track of whether the user has changed its value or not by maintaining the original value
as well as the new value. The component displays a change indicator if it has changes. The change status is propagated
to parent components so that they can indicate that they have changes in them. For example, a tab can indicate it has
changes in it even if it is not the active tab. This helps users easily navigate to components that have changes in
them.

The framework also provides buttons to navigate to the next/previous change if users want to review changes before they
save/apply them.

Change tracking also helps the framework determine which model nodes have changed and therefore construct a set-request
with just the changed nodes.

# Error Tracking

Error tracking is similar to change tracking: components keep track of errors. The component displays error messages if
it has errors. The error status is propagated to parent components so that they can indicate that they have errors in
them. For example, a tab can indicate it has errors in it even if it is not the active tab. This helps users easily
navigate to components that have errors.

The framework also provides buttons to navigate to the next/previous error to help quickly find errors in a large UI.

There are 2 sources of errors for components:

1. Frontend validation errors: the framework uses constraints defined in the meta-model for the model node path bound to
   a component to automatically validate user input in that component. This way, user errors are caught sooner by the
   frontend rather than later by the backend.
2. Backend validation errors: these errors occur when the backend validates a set-request. The validation errors are
   returned as a model tree in the set-response. As described earlier, these errors are automatically propagated to the
   corresponding components by the framework.

API errors like inability to reach the backend are displayed by the framework at the top of the page or dialog since
such errors are not specific to a particular component.

# Access Control

The framework automatically fetches RBAC definitions using the tree-ware API. Note that the RBAC definition is the same
shape as the model tree. The framework matches the RBAC definitions to components based on their path bindings. Based on
the matching, the framework hides (no access), disables (read-only access), or enables (read-write access) components.

# Access Control Definition

In this mode, every component will show controls for the user to specify whether the role being defined will have
read-only or read-write access to that component. This mode can show an existing role definition, allow it to be edited,
and allow it to be saved. The UI framework will take care of getting and setting the role definition via the tree-ware
API.

This mode improves the UX for role definition. Users do not have to deal with permission names and wonder if they
enabled/disabled the right permission. They don't have to wonder if they correctly disabled access to a portion of the
UI. Instead, they get to see the role in the context of the UI.

This raises the question: what if the UI does not use the entire model tree. How does the user permit/deny access to the
parts of the model tree not used by the UI? The UI framework has sufficient information to determine which parts of the
model tree are not used by the UI. So it can show a separate UI for specifying access to those nodes of the model tree.
It can also show this separate UI for the entire model tree for those users who only use the API and not the UI.

This is a roadmap feature. The detailed design will be documented
after [RBAC](https://github.com/tree-ware/tree-ware-kotlin-core/issues/57) has been implemented in the tree-ware
backend.

# Progressive Disclosure

All parts of the UI page or dialog definition need not be visible when first opened. Parts of the UI, like inactive tabs
and child dialogs, can be defined but not visible until the user interacts with the UI to make them visible. Developers
can specify if data should be pre-fetched for the invisible components. If data is to be pre-fetched, the framework uses
the path bindings of invisible components to fetch data for those components as well.

The framework is able to track changes and errors for invisible components as well. As a result, a single save/apply
button on the main screen is sufficient; save/apply buttons are not necessary in each tab or in each child/grandchild
dialog.

# Different Views

The framework can render the UI as a dialog or as a wizard. Developers do not need to specify 2 separate definitions.
The framework can render either from a single definition.

This is a roadmap feature. The detailed design will be documented later.

# Subscribing to Changes

Once tree-ware supports [pub-sub](https://github.com/tree-ware/tree-ware-kotlin-core/issues/8), the UI framework will
use subscription-requests instead of get-requests to fetch data for the UI. This will automatically update the UI
whenever the model changes in the backend.

This also applies to the RBAC definitions. The framework will subscribe to the RBAC definitions for the logged-in user.
If an admin changes the RBAC definitions in a different UI session, those changes will be received asynchronously by
other UI sessions and the UI framework will automatically change access to components in those UI sessions.