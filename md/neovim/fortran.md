# Fortran
```
> vi snippets/fortran.lua
```
```
local ls = require("luasnip")
local s = ls.snippet
local t = ls.text_node
local i = ls.insert_node

return {
  s("fmod", {
    t({ "module " }), i(1, "module_name"),
    t({ "", "  " }),
    t({ "", "  implicit none", "" }),
    t({ "", "contains", "" }),
    t({ "", "", "end module " }), rep(1),
  }),

  s("fsub", {
    t({ "subroutine " }), i(1, "name"), t("("), i(2, "args"), t({ ")", "" }),
    t({ "", "  implicit none", "" }),
    t({ "", "end subroutine " }), rep(1),
  }),

  s("func", {
    t({ "function " }), i(1, "name"), t("("), i(2, "args"), t({ ") "}), t({ "result()", "" }),
    t({ "", "  implicit none", "" }),
    t({ "", "end function " }), rep(1),
  }),

  s("fprog", {
    t({ "program " }), i(1, "main"),
    t({ "", "  " }),
    t({ "", "  implicit none", "" }),
    t({ "", "  " }), i(0),
    t({ "", "", "end program " }), rep(1),
  }),

  s("fdo", {
    t("do "), i(0),
    t({ "", "  " }),
    t({ "", "enddo" }),
  }),

  s("fif", {
    t("if ("), i(1, "condition"), t({ ") then", "" }),
    t("  "), i(0),
    t({ "", "endif" }),
  }),

  s("(", {
    t("(/ "), i(0), t({ " /)"}),
  }),
}
```

