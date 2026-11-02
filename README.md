# iselect

Select from candidates incrementally, like helm.

## iselect-read

`(iselect-read PROMPT CANDIDATES &key initial-input history input-fn limit
delay layout buffer)` reads a candidate in the minibuffer, and shows the
candidates matching the input in a child frame, or a new frame on text
terminals.

- CANDIDATES is a list of `(STRING . DATA)`.  STRING is shown and matched.
- The input is a regexp, but spaces match any string, as `.*`.  It is
  converted by `iselect-regexp-function`, and matches ignoring case as
  `case-fold-search`.
- LIMIT, if non-nil, is the maximum number of matches.  Filtering stops at
  it, which keeps large CANDIDATES fast.
- DELAY is the seconds to wait after the last command before updating the
  matches automatically, `iselect-delay` (0.2) by default.  If negative,
  they are not updated automatically.
- LAYOUT is `child-frame` or `frame`, `iselect-layout` by default, or nil
  not to show the candidates.
- BUFFER is the buffer of the candidates or its name, `*iselect*` by
  default.  It is kept after exiting, to be viewed.

In the minibuffer:

- The commands moving point, such as `C-n`, `C-p`, `M-<`, `M->`, `C-v`,
  `M-v` and `C-l`, select a candidate.
- `RET` returns the selected candidate.
- `C-M-j` returns the value of INPUT-FN called with the input, or signals
  `user-error` without INPUT-FN.
- `C-c C-u` updates the candidates, and `C-c C-s` suspends updating them
  automatically.
- Quitting signals `quit`.

## Tools

- `iselect-complete` completes the minibuffer input, to be bound in the
  maps of `completing-read`.
- `iselect-company` inserts a company candidate, to be bound in
  `company-active-map`.
- `iselect-history` replaces the input of the minibuffer, Comint or Eshell
  with one of its history.  More modes can be added to
  `iselect-history-alist`.
- `iselect-files` switches to a buffer, opens a known project in Dired,
  jumps to a bookmark, or opens a recent file.
- `iselect-project-files` switches to a buffer or opens a file of the
  current project, or of a project read with `C-u`.

## Configuration

To highlight the selected candidate and show the line numbers:

```elisp
(add-hook 'iselect-buffer-mode-hook #'hl-line-mode)
(add-hook 'iselect-buffer-mode-hook #'display-line-numbers-mode)
(add-hook 'iselect-update-hook #'hl-line-highlight)
(add-hook 'iselect-move-hook #'hl-line-highlight)
```
