If using Vi you find yourself editing a read only file, you can force a save using sudo:

Instead of simply using `:w` to save, use:

```
:w !sudo tee %
```

This will write a (temporary) file and then `tee` will move it to the "real" file location, with `sudo` allowing to complete the write.
