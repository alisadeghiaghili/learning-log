# Flashcards

<!-- Cards in Anki format: Question? ; Answer -->

---

## Ch 1 — SQL Execution Order

In what order is a SQL query logically executed? ; FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT. SELECT is evaluated last!

Why can't you use a SELECT alias in WHERE? ; Because WHERE is evaluated before SELECT. The alias doesn't exist yet. You must repeat the expression or use a subquery/CTE.

---

## Ch 1 — JOIN Types

What's the difference between INNER JOIN, LEFT JOIN, and FULL JOIN? ; INNER = only matching rows | LEFT = all left rows + matches | FULL = all rows from both sides. Unmatched rows get NULLs.

What is a CROSS JOIN and when is it used? ; Cartesian product — every row from A paired with every row from B. E.g., 10 customers × 3 products = 30 rows. Used for generating all combinations like scheduling.

What is a SELF JOIN? Give an example. ; Joining a table to itself. E.g., finding an employee and their manager: SELECT e.name, m.name FROM employees e LEFT JOIN employees m ON e.manager_id = m.id.

---

## Ch 1 — Join Algorithms

When is a Nested Loop Join the best choice? ; When the outer input is small and there's an index on the inner table. Each outer row is looked up via index. Cost: O(N × log M).

How does a Hash Join work? ; 1) Build a hash table from the smaller table on the join key, 2) probe it with each row from the larger table. Cost: O(N + M). Best for two large tables with no useful index.

When is a Merge Join used? ; When both inputs are already sorted (e.g., from an index). Both streams are traversed in parallel. Cost: O(N + M) if pre-sorted.

---

## Ch 1 — Correlated Subquery vs JOIN

What is a Correlated Subquery? ; A subquery that references the outer query — executes once per row. Usually slow unless there's an index on the filter column.

What's the difference between EXISTS and IN? ; EXISTS is correlated and compact (first match suffices) and NULL-safe. IN materializes the full subquery result and has weird behavior with NULLs. EXISTS is always safer and usually faster.

Why does `NOT IN (1, 2, NULL)` never return anything? ; Because it's equivalent to x!=1 AND x!=2 AND x!=NULL. x!=NULL is always UNKNOWN, so the whole AND becomes UNKNOWN → row is filtered out. Use NOT EXISTS instead.

---

## Ch 1 — NULL Semantics

What does `NULL = NULL` evaluate to? ; UNKNOWN (neither TRUE nor FALSE). So `WHERE col = NULL` never returns any rows. You must use `WHERE col IS NULL`.

What's the difference between COUNT(col) and COUNT(*)? ; COUNT(col) ignores NULL rows. COUNT(*) counts all rows, including NULLs.

What is `NULL * 0`? ; NULL. Any arithmetic operation with NULL yields NULL. Even 0 * NULL is NULL.

---

## Ch 1 — UNION

What's the difference between UNION and UNION ALL? ; UNION combines results and removes duplicates (requires sort/hash). UNION ALL keeps duplicates (faster). Use UNION ALL unless you actually need dedup.

---

## Ch 1 — SET vs BAG

Does SQL work on Sets or Bags? ; Bag (multiset). SQL tables can have duplicate rows, but mathematical relations cannot. SELECT DISTINCT converts a bag to a set.

---

## Ch 1 — Interview Keys

Why is SELECT * bad in production? ; 1) Extra data (bandwidth, memory), 2) App breaks if schema changes, 3) Covering index won't work, 4) Query intent is unclear.

What's the difference between DELETE vs TRUNCATE vs DROP? ; DELETE = remove specific rows (with WHERE), slow, triggers fire. TRUNCATE = remove all rows, fast, triggers don't fire. DROP = remove the entire table and its schema.
