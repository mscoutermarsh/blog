---
layout: post
title: Storing Emails in Postgres + Rails - Use Citext
---

Text fields in Postgres are case sensitive. This is a problem when storing
emails because `coutermarsh.mike@gmail.com` and `Coutermarsh.Mike@Gmail.com` are not equal.

If you do any lookups by email address, you have a few options.

1. always use downcase on write
2. use a functional index, lower(email)
3. use citext

Here are my experiences with each of them.

### Option 1: Use downcase (not ideal)
You could `email.downcase` each time you store an email or do a lookup. This
works well, but you (+ your entire team) will have to always remember to do
this.

### Option 2: Use a functional index, lower(email) - (also, not ideal)

The 2nd solution is to create a functional index with `lower(email)` and
forever have to query ActiveRecord like this: `User.where("lower(email) = ?, email")`.
This is my least favorite solution. It seems like it would work really well. But in practice can cause a lot of problems
when combined with ActiveRecord.
You can no longer use `where`, `find_by`, or `create` in the ways you expect to.

For example: `User.create(attributes)` is very common in a Rails app. If you require
email addresses to be `unique` (which you probably do). Rails will run a
query first to see if a record exists.

```sql
SELECT 1 as ONE FROM users WHERE email = 'test@example.com' LIMIT 1;
```

See the problem in that query? Our index is on `lower(email)`. This query misses the index and you're now doing a full table scan each time a
user is created.

Other seemingly innocent ActiveRecord queries will also miss your index. Here's a common one: `User.find_by(email: 'test@example.com')`.

The problem with lower is similar to using downcase, you & your entire team have to remember to always
use lower.

### Option 3: Use Citext!

Here's my favorite solution (this is what we use at Product Hunt).

 > The citext module provides a case-insensitive character string type, citext.
 > Essentially, it internally calls lower when comparing values. Otherwise, it
 > behaves almost exactly like text.
 > -- [Postgres Docs](https://www.postgresql.org/docs/9.1/static/citext.html)

Citext makes your email column case-insensitive.

If you want to change an existing column to Citext. Here's an example Rails
migration.

```ruby
# Example Rails migration
class ChangeEmailToCitext < ActiveRecord::Migration
  def change
    enable_extension("citext")

    change_column :users, :email, :citext

    ## **Warning!** changing a columns datatype locks the table. This can cause
    ## downtime in production (your app will not be able to write while the change is
    ## being made). Read: https://www.braintreepayments.com/blog/safe-operations-for-high-volume-postgresql

    ## If locking the table is not an option for you, **do not** use this migration. You'll
    ## instead need to create a new citext column, change your code to write to both, backfill the new
    ## column and then switch reads/writes to the new column.
  end
end
```

Remember to add an index to this column.

```ruby
class AddIndexToUsersEmail < ActiveRecord::Migration
  disable_ddl_transaction!

  def change
    add_index :users, :email, unique: true, algorithm: :concurrently
  end
end
```

I hope you find this helpful! If you've come across any better solutions, or
have ways to improve this one, please comment below.
