---
layout: default
title: Auto Generated UI
nav_order: 2
parent: Contributor Docs
---

# Introduction

The UI for a tree-ware model can be auto-generated. A UI can be generated for the entire model tree or for a specific
subtree. The latter allows parts of the UI to be auto-generated and integrated into a custom UI.

# UI Generation

The model is a tree and so is a UI. So the [tree-ware UI Framework](ui-framework.md) can create tree-ware UI components
for each node in the model tree and [bind them together](ui-framework.md#ui-path-bindings). This results in a
fully-functional auto-generated UI.

# Meta-Model Aux Data

The auto-generation framework chooses UI components based on field types in the meta-model. For example, it uses a text
field for string fields. But there could be cases where the default is not suitable. For example, a text area might be a
more suitable UI component than a text field for some string fields. There could be other cases where there is no
default. For example, there is no default UI component for blob fields. In such cases, task-specific aux data can be
used in the meta-model to specify or override a component.

You can also specify field ordering in this task-specific aux data to override the default ordering of fields in its
entity. This is especially useful when new fields are added to an entity.

# Roadmap

* Map multiple model fields to a single custom UI component.