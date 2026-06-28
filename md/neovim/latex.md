# LaTeXのスニペット
```
> vi snippets/tex.lua
```
```
local ls = require("luasnip")
local s  = ls.snippet
local t  = ls.text_node
local i  = ls.insert_node

return {

  s({trig = "eq", priority = 2000}, {
    t("\\begin{equation}"), 
		t({ "", "  " }), 
		i(1),
    t({ "", "\\end{equation}" }),
  }),

  s({trig = "al", priority = 2000}, {
    t("\\begin{align}"), 
		t({ "", "  " }), 
		i(1),
    t({ "", "\\end{align}" }),
  }),

  s({trig = "eqal", priority = 2000}, {
    t("\\begin{equation}"), 
    t({ "", "  \\begin{aligned}" }), 
		t({ "", "    " }), 
		i(1),
    t({ "", "  \\end{aligned}" }),
    t({ "", "\\end{equation}" }),
  }),

  s({trig = "item", priority = 2000}, {
    t("\\begin{itemize}"), 
		t({ "", "  \\item " }), 
		i(1),
    t({ "", "\\end{itemize}" }),
  }),

  s({trig = "enu", priority = 2000}, {
    t("\\begin{enumerate}"), 
		t({ "", "  \\item " }), 
		i(1),
    t({ "", "\\end{enumerate}" }),
  }),

  s({trig = "fig", priority = 2000}, {
    t("\\begin{figure}[H]"), 
		t({ "", "  \\centering" }), 
		t({ "", "  \\caption{}"}), 
		i(1),
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "tab", priority = 2000}, {
    t("\\begin{table}[H]"), 
		t({ "", "  \\centering" }), 
		t({ "", "  \\makeatletter" }), 
		t({ "", "  \\def\\@captype{table}" }), 
		t({ "", "  \\makeatother" }), 
		t({ "", "  \\caption{}"}), 
		t({ "", "  \\begin{tabular}{ccc}" }), 
		i(1),
		t({ "", "    \\hline\\hline" }), 
		t({ "", "    &   &   \\\\\\hline" }), 
		t({ "", "    &   &   \\\\" }), 
		t({ "", "    &   &   \\\\" }), 
		t({ "", "    \\hline\\hline" }), 
		t({ "", "  \\end{tabular}" }), 
    t({ "", "\\end{table}" }),
  }),

  s({trig = "figtab", priority = 2000}, {
    t("\\begin{figure}[H]"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\caption{}" }), 
		i(1),
    t({ "", "  \\end{minipage}"}), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\makeatletter" }), 
		t({ "", "    \\def\\@captype{table}" }), 
		t({ "", "    \\makeatother" }), 
		t({ "", "    \\caption{}"}), 
		t({ "", "    \\begin{tabular}{ccc}" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "    \\end{tabular}" }), 
    t({ "", "  \\end{minipage}" }),
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "itemtab", priority = 2000}, {
    t("\\begin{tabular}{cc}"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\begin{itemize}" }), 
		t({ "", "				\\item " }), 
		i(1),
		t({ "", "    \\end{itemize}" }), 
    t({ "", "  \\end{minipage}"}), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\makeatletter" }), 
		t({ "", "    \\def\\@captype{table}" }), 
		t({ "", "    \\makeatother" }), 
		t({ "", "    \\caption{}"}), 
		t({ "", "    \\begin{tabular}{ccc}" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      &   &   \\\\" }), 
		t({ "", "      \\hline\\hline" }), 
		t({ "", "    \\end{tabular}" }), 
    t({ "", "  \\end{minipage}" }),
    t({ "", "\\end{tabular}" }),
  }),

  s({trig = "itemfig", priority = 2000}, {
    t("\\begin{figure}[H]"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
    t({ "", "    \\begin{itemize}" }),
    t({ "", "      \\item " }),
    t({ "", "    \\end{itemize}" }),
    t({ "", "  \\end{minipage}" }),
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\caption{}" }), 
		i(1),
    t({ "", "  \\end{minipage}"}), 
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "enufig", priority = 2000}, {
    t("\\begin{figure}[H]"), 
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
    t({ "", "    \\begin{enumerate}" }),
    t({ "", "      \\item " }),
    t({ "", "    \\end{enumerate}" }),
    t({ "", "  \\end{minipage}" }),
    t({ "", "  \\begin{minipage}{0.5\\textwidth}"}), 
		t({ "", "    \\centering" }), 
		t({ "", "    \\caption{}" }), 
		i(1),
    t({ "", "  \\end{minipage}"}), 
    t({ "", "\\end{figure}" }),
  }),

  s({trig = "vs", priority = 2000}, {
    t(""), 
		t({ "", "\\vspace{-10pt}" }), 
		t({ "", ""}), 
		i(1),
  }),

  s({trig = "col", priority = 2000}, {
    t("\\textcolor{"), 
		i(1),
    t("}{}"), 
  }),

  s({trig = "frac", priority = 2000}, {
    t("\\frac{"), 
		i(1),
    t("}{}"), 
  }),

}
```

