---
id: custom-view-global
title: $global
---

The prop `$global` is passed to the top-level React component that is used as a
custom view. It can be used to get global data, e.g. current user.

## Properties

<AUTOGENERATED_TABLE_OF_CONTENTS>

---

# Reference

## Properties

### `currentUser`

An object containing current user information. Fields include: `activeStatus, email, firstName, lastName, id, userId, name` etc.

Note `id` is for the `User` object of the app, whereas userId is the globally unique user id.
