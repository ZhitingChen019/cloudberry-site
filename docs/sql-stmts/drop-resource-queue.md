---
title: DROP RESOURCE QUEUE
---

# DROP RESOURCE QUEUE

Removes a resource queue.

## Synopsis

```sql
DROP RESOURCE QUEUE <queue_name>
```

## Description

This command removes a resource queue from Apache Cloudberry. To drop a resource queue, the queue cannot have any roles assigned to it, nor can it have any statements waiting in the queue. Only a superuser can drop a resource queue.

## Parameters

**`queue_name`**

The name of a resource queue to remove.

## Notes

Use [ALTER ROLE](/docs/sql-stmts/alter-role.md) to remove a user from a resource queue.

To see all the currently active queries for all resource queues, perform the following query of the `pg_locks` table joined with the `pg_roles` and `pg_resqueue` tables:

```sql
SELECT rolname, rsqname, locktype, objid, pid, 
mode, granted FROM pg_roles, pg_resqueue, pg_locks WHERE 
pg_roles.rolresqueue=pg_locks.objid AND 
pg_locks.objid=pg_resqueue.oid;
```

To see the roles assigned to a resource queue, perform the following query of the `pg_roles` and `pg_resqueue` system catalog tables:

```sql
SELECT rolname, rsqname FROM pg_roles, pg_resqueue WHERE 
pg_roles.rolresqueue=pg_resqueue.oid;
```

## Examples

Remove a role from a resource queue (and move the role to the default resource queue, `pg_default`):

```sql
ALTER ROLE bob RESOURCE QUEUE NONE;
```

Remove the resource queue named `adhoc`:

```sql
DROP RESOURCE QUEUE adhoc;
```

## Compatibility

The `DROP RESOURCE QUEUE` statement is a Apache Cloudberry extension.

## See also

[ALTER RESOURCE QUEUE](/docs/sql-stmts/alter-resource-queue.md), [CREATE RESOURCE QUEUE](/docs/sql-stmts/create-resource-queue.md), [ALTER ROLE](/docs/sql-stmts/alter-role.md)
