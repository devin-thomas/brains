---
url: https://github.com/mattpocock/skills/blob/5beb35b040e3de97e54fa9e386d7e42d15c295c2/skills/engineering/implement/SKILL.md
title: "implement"
kind: repo
license: MIT
corpus_scope: licensed-full-text
method: git-source
retrieved: 2026-10-20
source_commit: 5beb35b040e3de97e54fa9e386d7e42d15c295c2
source_file_last_commit: 6d6a5b9072f47ee7ba1d972d8ce0ee98b7ed4394
source_file_last_modified: 2026-10-07
---
# implement

**Attribution:** Matt Pocock skill in `mattpocock/skills`. Git history records commits by Matt Pocock; the latest file commit is by Matt Pocock on 2026-10-07.
**Publication date:** undated in the source. Latest file modification is recorded above; that is not a publication date.

## Original source frontmatter

```yaml
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
```

## Source text

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Call the Skill tool with "tdd" where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, call the Skill tool with "code-review" to review the work.

Commit your work to the current branch.

## Included license notice

The following license notice is included with the copied source text.

```text
MIT License

Copyright (c) 2026 Matt Pocock

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
