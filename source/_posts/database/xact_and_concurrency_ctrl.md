---
title: Transaction and Concurrency Control
date: 2020-11-13
categories: [Database]
---

## Transaction

### Definition

**Transaction:** A sequence of multiple actions to be executed as an *atomic* unit.

Transaction in SQL view:

- Begin transaction
- Sequence of SQL statements
- End transaction

Transaction Manager controls excution of transactions. Program logic is invisible to DBMS. For example:

``` sql
1. start transaction
2. read(R)
3. R = R - 100  ------> Invisible
4. write(R)
5. read(S)
6. S = S + 100  ------> Invisible
7. write(S)
8. end transaction
```

### ACID Properties

**A** tomicity: All actions in the transaction happen, or none happen.
**C** onsistency: If the database starts our consistent, it ends up consistent at the end of the transaction.
**I** solation: Execution of each transaction is isolated from that of others.
**D** urability: If a transaction commits, its effect persist.

Note:

1. For `isolation`, it's **just** the *excution* not being affected.
2. `Consistency` in database system refers to the requirement that any given DB transaction must change affected data only in allowed ways. ([wiki](https://en.wikipedia.org/wiki/Consistency_(database_systems)#:~:text=Consistency%20in%20database%20systems%20refers,triggers%2C%20and%20any%20combination%20thereof.))

## Concurrency Control

**Serial schedule:** Each transaction runs from start to finish without any intervening actions from other transactions.

**Equivalent:** Two Schedules are *equivalent*, when:

1. involve same transaction
2. each individual transaction's actions are ordered the same
3. both schedules leave the DB in the same final state

**Serializable**: A serial is equivalent to some serial schedule.

**Conflict:** Two *oprations* are conflict, when:

1. Are by different transactions
2. Are on the same object
3. At least one is write

> Order of non-conflict operations has no effect on the final state of the DB.

**Conflict equivalent:**

1. involve the same actions of the same transactions
2. every pair of conflicting actions is ordered the same way

**Conflict serializable:**

1. the serial is conflict equivalent to some serial schedule
2. the serial is also serializable

> Some serializable schedule is not conflict serializable.

Example:

a. conflict serializable

``` shell
T1: R(A) W(A)           R(B) W(B)
T2:           R(A) W(A)           R(B) W(B)
```

b. not conflict serializable

``` shell
T1: R(A)           W(A)
T2:      R(A) W(A)

It is serializable but not conflict serializable
```

**View serializable:**

1. same initial reads
2. same dependent reads
3. same winning writes (If Ti writes finial value of A in S1, then Ti also writes final value of A in S2)

## Two Phase Locking (2PL)

Rules: transaction must obtain as S (shared) lock before reading, and an X (exclusive) lock before writing.

**2PL guarantees conflict serializability.**

2PL ----------> Release one by one
strict 2PL ---> Release at the same time
