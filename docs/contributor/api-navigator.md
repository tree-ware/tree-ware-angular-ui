---
layout: default
title: API Navigator
nav_order: 3
parent: Contributor Docs
---

# Introduction

The API Navigator helps users navigate the tree-ware API and build API requests.

# Navigator UI

The navigator UI is the [auto-generated UI](auto-generated-ui.md) of the entire model tree. But how does this let users
build API requests? There is an option in the auto-generation UI framework to show API buttons.

When this option is enabled, each tree-ware UI component shows a checkbox to allow users to select the fields to include
in the API requests. A "Show APIs" button is displayed at the bottom of the page. This button opens a dialog with tabs
for the get-request and set-request. The API requests include only the selected fields. The tabs allow you to issue the
requests, and see the responses.

# UML Diagram

The API Navigator shows a UML diagram of the meta-model.

# Roadmap

* Make the UML diagram an interactive (but still read-only) diagram. The goalo is to make it easier to follow links in
  large diagrams.
* Make fields in the UML diagram selectable so that API requests can be created from those selections.
* Control which RBAC roles have access to the navigator. Note that this is different from RBAC for individual nodes in
  the model tree. The latter is automatically supported by the auto-generated UI. This is about hiding the entire
  navigator UI from everyone except certain roles.
* A UI for defining a new meta-model or updating an existing meta-model. Thanks
  to [self-hosting](https://github.com/tree-ware/tree-ware-kotlin-core/issues/24), this is basically a navigator UI for
  the meta-meta-model.