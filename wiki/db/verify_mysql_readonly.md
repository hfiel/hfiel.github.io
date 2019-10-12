To check if a MySQL DB server is in read only mode, verify the value of this global variable.

Will be at 0 if in read/write, at 1 if in read-only mode.


```
SELECT @@global.read_only;
```

