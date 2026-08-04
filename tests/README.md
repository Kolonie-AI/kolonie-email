# tests

Two things need tests before anything else does, because both fail silently:

- retention actually destroys content, and the envelope actually survives
- a tombstoned address is rejected and never reissued
