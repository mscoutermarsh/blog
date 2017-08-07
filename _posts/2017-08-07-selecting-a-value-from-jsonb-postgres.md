---
layout: post
title: Selecting a value from JSONB - Postgres
---

Quick postgres tip.

If you have a **JSONB** column named "options" and it has a key "favorite_color". Here is
an example on how you select the value from it.

```sql
SELECT options->'favorite_color' FROM table_name;
```

Filter by a jsonb value:

```sql
SELECT COUNT(*) FROM table_name WHERE (options @> '{"favorite_color":"blue"}');
```

If you're doing this in Rails, here's an example with ActiveRecord.

```Ruby
YourModel.where('options @> ?', { favorite_color: 'blue' }.to_json) }
```

To learn more, take a look at this post: [What can you do with PostgreSQL and JSON?](http://clarkdave.net/2013/06/what-can-you-do-with-postgresql-and-json/)
