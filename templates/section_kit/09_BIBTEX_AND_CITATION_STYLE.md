# 09 — BibTeX and Citation Style

Top accounting / economics journals use **Chicago author-date** with `natbib`. AER, QJE, JF, JFE, RFS, JAE, TAR, JAR, RAST, JoE all converge on this style modulo journal-specific punctuation. Get this right once and you can submit anywhere.

## Preamble setup

```latex
\usepackage[round, authoryear]{natbib}
\bibliographystyle{aer}           % or 'apalike', 'chicago'; AER preferred
\bibpunct{(}{)}{;}{a}{,}{,}
```

## Citation commands cheatsheet

| Command | Output | Use when |
|---------|--------|----------|
| `\citet{key}`        | Hoopes, Mescall, and Pittman (2012) | The authors appear in the narrative |
| `\citep{key}`        | (Hoopes, Mescall, and Pittman, 2012) | Citation supports a parenthetical claim |
| `\citep{a, b, c}`    | (A, 2010; B, 2014; C, 2021) | Multiple papers in one parenthetical |
| `\citet[ch.~3]{key}` | Smith (2010, ch. 3) | Page/chapter cite in narrative |
| `\citep[p.~234]{key}`| (Smith, 2010, p. 234) | Page cite in parenthetical |
| `\citealt{key}`      | Smith, 2010 | Inside a parenthetical you author yourself: "(see Smith, 2010, and \citealt{jones})" |
| `\citeyearpar{key}`  | (2010) | Author already named in sentence |
| `\citeauthor{key}`   | Smith | Author only |

**Rule of thumb.** In a top-journal accounting paper, roughly 60% of citations should be `\citet` and 40% `\citep`. If your paper is 90% `\citep`, you are doing a literature dump rather than synthesizing.

## BibTeX key convention

Use `lastname[lastname[lastname]]YEAR`, lowercase, no punctuation:

```
hoopesmescallpittman2012        % three authors
hanlonhoopesshroff2014
chenchenchengshevlin2010
defondqisizhang2025
limshevlinwangxu2025
heliMonroesi2021                % note: 'li' followed by 'Monroe' -> ambiguous; prefer 'helimonroesi2021'
khavisshenemanszerwo2025
cengizdubelinderzipperer2019    % four authors -> still use all
callawaysantanna2021
sunabraham2021
borusyakjaravelspiess2024
rambachanroth2023
hanlonheitzman2010
```

**Rules:**
1. Lowercase only.
2. No accents (`sant'anna` → `santanna`).
3. For 4+ authors, list all. (Avoids collisions with similar 3-author papers.)
4. No special characters (`%`, `:`, `&`, `'`) in the key.
5. Year is always 4 digits.

## refs.bib — pre-populated skeleton

Save as `refs.bib` in the paper root.

```bibtex
@article{hoopesmescallpittman2012,
  author  = {Hoopes, Jeffrey L. and Mescall, Devan and Pittman, Jeffrey A.},
  title   = {Do {IRS} Audits Deter Corporate Tax Avoidance?},
  journal = {The Accounting Review},
  year    = {2012},
  volume  = {87},
  number  = {5},
  pages   = {1603--1639}
}

@article{hanlonhoopesshroff2014,
  author  = {Hanlon, Michelle and Hoopes, Jeffrey L. and Shroff, Nemit},
  title   = {The Effect of Tax Authority Monitoring and Enforcement on Financial Reporting Quality},
  journal = {The Journal of the American Taxation Association},
  year    = {2014},
  volume  = {36},
  number  = {2},
  pages   = {137--170}
}

@article{chenchenchengshevlin2010,
  author  = {Chen, Shuping and Chen, Xia and Cheng, Qiang and Shevlin, Terry},
  title   = {Are Family Firms More Tax Aggressive than Non-family Firms?},
  journal = {Journal of Financial Economics},
  year    = {2010},
  volume  = {95},
  number  = {1},
  pages   = {41--61}
}

@article{defondqisizhang2025,
  author  = {DeFond, Mark L. and Qi, Baolei and Si, Yi and Zhang, Jieying},
  title   = {Do Signatory Auditors with Tax Expertise Facilitate or Curb Tax Aggressiveness?},
  journal = {Journal of Accounting and Economics},
  year    = {2025},
  note    = {Forthcoming / in press; verify volume and pages upon publication}
}

@article{limshevlinwangxu2025,
  author  = {Lim, Chee Yeow and Shevlin, Terry J. and Wang, Kun and Xu, Yanping},
  title   = {Tax Knowledge Diffusion Through Shared Audit Partners: Evidence From China},
  journal = {Journal of Accounting, Auditing \& Finance},
  year    = {2025},
  volume  = {40},
  number  = {2},
  pages   = {486--514}
}

@article{helimonroesi2021,
  author  = {He, Chang and Li, Chao Kevin and Monroe, Gary S. and Si, Yi},
  title   = {Diversity of Signing Auditors and Audit Quality},
  journal = {Auditing: A Journal of Practice \& Theory},
  year    = {2021},
  volume  = {40},
  number  = {3},
  pages   = {27--52}
}

@article{khavisshenemanszerwo2025,
  author  = {Khavis, Joshua A. and Sheneman, Amy M. G. and Szerwo, Brandon},
  title   = {Does Gender Composition of Audit Teams Matter? An Examination of Audit Quality and Audit Cost},
  journal = {Review of Accounting Studies},
  year    = {2025},
  note    = {Online June 2025; doi 10.1007/s11142-025-09924-1}
}

@article{cengizdubelinderzipperer2019,
  author  = {Cengiz, Doruk and Dube, Arindrajit and Lindner, Attila and Zipperer, Ben},
  title   = {The Effect of Minimum Wages on Low-wage Jobs},
  journal = {Quarterly Journal of Economics},
  year    = {2019},
  volume  = {134},
  number  = {3},
  pages   = {1405--1454}
}

@article{callawaysantanna2021,
  author  = {Callaway, Brantly and Sant'Anna, Pedro H. C.},
  title   = {Difference-in-Differences with Multiple Time Periods},
  journal = {Journal of Econometrics},
  year    = {2021},
  volume  = {225},
  number  = {2},
  pages   = {200--230}
}

@article{sunabraham2021,
  author  = {Sun, Liyang and Abraham, Sarah},
  title   = {Estimating Dynamic Treatment Effects in Event Studies with Heterogeneous Treatment Effects},
  journal = {Journal of Econometrics},
  year    = {2021},
  volume  = {225},
  number  = {2},
  pages   = {175--199}
}

@article{borusyakjaravelspiess2024,
  author  = {Borusyak, Kirill and Jaravel, Xavier and Spiess, Jann},
  title   = {Revisiting Event-Study Designs: Robust and Efficient Estimation},
  journal = {Review of Economic Studies},
  year    = {2024},
  volume  = {91},
  number  = {6},
  pages   = {3253--3285}
}

@article{rambachanroth2023,
  author  = {Rambachan, Ashesh and Roth, Jonathan},
  title   = {A More Credible Approach to Parallel Trends},
  journal = {Review of Economic Studies},
  year    = {2023},
  volume  = {90},
  number  = {5},
  pages   = {2555--2591}
}

@article{hanlonheitzman2010,
  author  = {Hanlon, Michelle and Heitzman, Shane},
  title   = {A Review of Tax Research},
  journal = {Journal of Accounting and Economics},
  year    = {2010},
  volume  = {50},
  number  = {2--3},
  pages   = {127--178}
}
```

## Common BibTeX errors to avoid

1. **Case-sensitive titles.** LaTeX lowercases titles in some `bibstyle`s. Wrap proper nouns in `{}`: `Do {IRS} Audits Deter Corporate Tax Avoidance?`
2. **Page ranges with `-` instead of `--`.** Use double-hyphen: `1603--1639`.
3. **`&` instead of `\&`.** In titles and journal names, escape: `Auditing: A Journal of Practice \& Theory`.
4. **Authors connected with commas instead of `and`.** Always `Author A and Author B and Author C`.
5. **Hidden trailing whitespace** breaks some parsers. Run a `bibtool -s -o out.bib in.bib` clean pass before submission.
6. **Inconsistent author-name format.** Pick `Lastname, Firstname` and stick to it for the whole file.
7. **Missing `volume` or `number`** triggers natbib warnings in some `bibstyle`s.
8. **Citing the working-paper version** when a published version exists. Always cite the journal version unless explicitly a 2024–2026 paper not yet in print.

## Drafting checklist

- [ ] `refs.bib` has one entry per cited paper, no orphans.
- [ ] All author-year-journal triples cross-checked against publisher page or SSRN.
- [ ] No working-paper citations once the published version exists.
- [ ] All proper nouns wrapped in `{}`.
- [ ] All page ranges use `--`.
- [ ] `natbib` loaded with `[round, authoryear]` options.
- [ ] `\bibliographystyle{aer}` (or `chicago`) — never `plain`.
- [ ] At least 60% of citations are `\citet` in the lit-review section.
