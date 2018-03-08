---
id: custom-view-models
title: $models
---

The prop `$models` is passed to the top-level React component that is used as a
custom view. It is a dictionary of [Object Models](#ObjectModel) defined in the running app.

## ObjectModel

ORM-style model that can be used to query records of an Object.

### Static Methods

#### static bulkCreate(records: [RecordObject](#RecordObject)[]): Promise<[ObjectModel](#ObjectModel)[]>

Create multiple records in the database.

Examples:

```js
const newContacts = await $models.Contact.bulkCreate([
  { name: 'Alice', email: 'a@bappo.com', customer_id: '1' },
  { name: 'Bob', email: 'b@bappo.com', customer_id: '1' },
  { name: 'Charles', email: 'c@bappo.com', customer_id: '2' },
]);
```

#### static bulkUpdate(recordUpdates: [RecordObject](#RecordObject)[]): Promise\<void>

Update multiple records in the database. Records are found by matching `id`, so
`id` is required in each `recordUpdate`.

Examples:

```js
$models.Contact.bulkUpdate([
  { id: '1', name: 'a.new@bappo.com' },
  { id: '2', customer_id: '2' },
]);
```

#### static create(record: [RecordObject](#RecordObject)): Promise<[ObjectModel](#ObjectModel)>

Create a record in the database.

Examples:

```js
const newContact = await $models.Contact.create({
  name: 'Alice',
  email: 'a@bappo.com',
  customer_id: '1',
});
```

#### static destroy(options: { where: [OptionWhere](#OptionWhere) }): Promise\<void>

Delete records in the database matching a where clause.

Examples:

```js
$models.Contact.destroy({
  where: {
    customer_id: '1',
  },
});
```

#### static destroyById(recordId: string): Promise\<void>

Delete a record in the database by id.

Examples:

```js
$models.Contact.destroyById('1');
```

#### static findAll(options: { where: [OptionWhere](#OptionWhere), include: [OptionInclude](#OptionInclude), page: [OptionPage](#OptionPage) }): Promise<[ObjectModel](#ObjectModel)[]>

Find records in the database matching the query.

Examples:

```js
const customers = await $models.Customer.findAll({
  include: [
    { as: 'contacts' },
  ],
  where: {
    name: 'Coles',
  },
  page: {
    offset: 100,
    limit: 50,
  },
});
```

#### static findAndCountAll(options: { where: [OptionWhere](#OptionWhere), include: [OptionInclude](#OptionInclude), page: [OptionPage](#OptionPage) }): Promise<{ records: [ObjectModel](#ObjectModel)[], total: number }>

Find records in the database matching the query (with offset and limit applied)
and get the total number of records matching the query.

Examples:

```js
const { records, total } = await $models.Customer.findAndCountAll({
  include: [
    { as: 'contacts' },
  ],
  where: {
    name: 'Coles',
  },
  page: {
    offset: 100,
    limit: 50,
  },
});
```
