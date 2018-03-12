---
id: custom-view-models
title: $models
---

The prop `$models` is passed to the top-level React component that is used as a
custom view. It is a dictionary of Object Key -> [Object Model](#ObjectModel) defined in the running app.

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

#### static destroy(options?: { where?: [OptionWhere](#OptionWhere) }): Promise\<void>

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

#### static findAll(options?: { where?: [OptionWhere](#OptionWhere), include?: [OptionInclude](#OptionInclude)[], page?: [OptionPage](#OptionPage) }): Promise<[ObjectModel](#ObjectModel)[]>

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

#### static findAndCountAll(options?: { where?: [OptionWhere](#OptionWhere), include?: [OptionInclude](#OptionInclude)[], page?: [OptionPage](#OptionPage) }): Promise<{ records: [ObjectModel](#ObjectModel)[], total: number }>

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

#### static findById(id: string, options?: { include?: [OptionInclude](#OptionInclude)[] }): Promise<[ObjectModel](#ObjectModel)>

Find a record in the database by id.

Examples:

```js
const customer = await $models.Customer.findById('1', {
  include: [
    { as: 'contacts' },
  ],
});
```

#### static findOne(options?: { where?: [OptionWhere](#OptionWhere), include?: [OptionInclude](#OptionInclude)[] }): Promise<[ObjectModel](#ObjectModel)>

Find a record in the database matching the query.

Examples:

```js
const customer = await $models.Customer.findOne({
  include: [
    { as: 'contacts' },
  ],
  where: {
    name: 'Coles',
  },
});
```

#### static update(values: [RecordObject](#RecordObject), options?: { where?: [OptionWhere](#OptionWhere) }): Promise\<void>

Update records in the database matching the query.

Examples:

```js
$models.Contact.update(
  { name: 'Bob' },
  {
    where: {
      name: 'Alice',
    },
  },
);
```

## Types

### RecordObject: { [fieldName: string]: any }

A RecordObject is an object (key/value pairs) used to represent a full or
partial record. Keys of a RecordObject are Field names of an Object, except for
a Reference Field the key is the Field name + `_id`.

Example:

```js
const contactRecordObject = {
  name: 'Alice',
  customer_id: '1',
};
```

### OptionInclude: { as: string, include?: [OptionInclude](#OptionInclude)[], where?: [OptionWhere](#OptionWhere) }

The `include` option lets you specify relationships you want to include in the
record.

`as` is the Relationship name. It is required.

You can use nested `include` and `where` to include and filter nested
relationships.

Example:

```js
const contactsFromColesNamedAlice = await $models.Contact.findAll({
  where: {
    name: 'Alice',
  },
  include: [
    {
      as: 'customer',
      where: {
        name: 'Coles',
      },
    },
  ],
});
```

### OptionWhere

The `where` option lets you filter a query.

#### Basic

```js
// Find all contacts of customer 1
$models.Contact.findAll({
  where: {
    customer_id: '1',
  },
});

// Find all contacts whose birthday is in July 1980
$models.Contact.findAll({
  where: {
    birthday: {
      $between: ['1980-07-01', '1980-07-31'],
    },
  },
});
```

#### Operators

| Operator      | Operands                                     | SQL                     |
| :-----------: | :------------------------------------------: | ----------------------- |
| `$and`        | OptionWhere[]                                | AND                     |
| `$or`         | OptionWhere[]                                | OR                      |
| `$gt`         | string &#124; number                         | >                       |
| `$gte`        | string &#124; number                         | >=                      |
| `$lt`         | string &#124; number                         | <                       |
| `$lte`        | string &#124; number                         | <=                      |
| `$ne`         | string &#124; number                         | !=                      |
| `$eq`         | string &#124; number                         | =                       |
| `$not`        | boolean                                      | IS NOT TRUE             |
| `$between`    | [string &#124; number, string &#124; number] | BETWEEN *a* AND *b*     |
| `$notBetween` | [string &#124; number, string &#124; number] | NOT BETWEEN *a* AND *b* |
| `$in`         | string[] &#124; number[]                     | IN [*a*, *b*, ...]      |
| `$notIn`      | string[] &#124; number[]                     | NOT IN [*a*, *b*, ...]  |
| `$iLike`      | string                                       | ILIKE                   |
| `$notILike`   | string                                       | NOT ILIKE               |

#### Combinations

```js
$models.Contact.findAll({
  where: {
    $or: [
      {
        birthday: {
          $between: ['1980-07-01', '1980-07-31'],
        },
      },
      {
        $and: [
          {
            customer_id: '1',
          },
          {
            name: {
              'iLike': 'alice%',
            },
          },
        ],
      },
    ],
  },
});
```
