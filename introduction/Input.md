# Input

This program accepts various options of input:

- *Commands*: Commands have the following format: `command[space]arg1,[optional space]arg2,argN`.

- *Variables*: Variables optionally start with a `$` and can only store one DWORD (QWORD on x64).

- *Registers*: All registers (of all sizes) can be used as variables.

- *Memory locations*: You can read/write from/to a memory location by using one of the following expressions:
   - `[addr]` read a DWORD/QWORD from `addr`.
   - `n:[addr]` read n bytes from `addr`.
   - `seg:[addr]` read a DWORD/QWORD from a segment at `addr`.

   **REMARKS**:
   - `n` is the amount of bytes to read, this can be anything smaller than 4 on x32 and smaller than 8 on x64 when specified, otherwise there will be an error.
   - seg can be `gs`, `es`, `cs`, `fs`, `ds`, `ss`. Only `fs` and `gs` have an effect.


- *Flags*: Debug flags (interpreted as integer) can be used as input. Flags are prefixed with an `_` followed by the flag name. Valid flags are: `_cf`, `_pf`, `_af`, `_zf`, `_sf`, `_tf`, `_if`, `_df`, `_of`, `_rf`, `_vm`, `_ac`, `_vif`, `_vip` and `_id`.

- *Numbers*: All numbers are interpreted as hex by default. If you want to be sure, you can `x` or `0x` as a prefix. Decimal numbers can be used by prefixing the number with a dot: `.123=7B`.

- *Expressions*: See the expressions section for more information.

- *Labels/Symbols*: User-defined labels and symbols are a valid expressions (they resolve to the address of said label/symbol).

## Module Data

-  *DLL exports*: Type `GetProcAddress` and it will automatically be resolved to the actual address of the function. To explicitly define from which module to load the API, use: `[module].dll:[api]` or `[module]:[api]`. In a similar way you can resolve ordinals, try `[module]:[ordinal]`. Another macro allows you to get the loaded base of a module. When `[module]` is an empty string `:GetProcAddress` for example, the module that is currently selected in the CPU will be used.

- *Loaded module bases*: If you want to access the loaded module base, you can write: `[module]:0`, `[module]:base`, `[module]:imagebase` or `[module]:header`.

- *RVA/File offset*: If you want to access a module RVA you can either write `[module]:0+[rva]` or you can write `[module]:$[rva]`. If you want to convert a file offset to a VA you can use `[module]:#[offset]`. When `[module]` is an empty string `:0` for example, the module that is currently selected in the CPU will be used. 

- *Module entry points*: To access a module entry point you can write `[module]:entry`, `[module]:oep` or `[module]:ep`. Notice that when there are exports with the names `entry`, `oep` or `ep` the address of these will be returned instead.

   **Notice**:
   - Instead of the `:` delimiter you can also use a `.` If you need to query module information such as `[module]:imagebase` or `[module]:entry` you are advised to use a `?` as delimiter instead: `[module]?entry`. The `?` delimiter does checking for named exports later, so it will still work when there is an export called `entry` in the module.

**Input for arguments can always be done in any of the above forms, except if stated otherwise.**

