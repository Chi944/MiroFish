# Local research integration

Read `LOCAL-OPERATIONS.md` before changing this checkout. This is the local
MiroFish workbench used by Stock Research's optional Research Lab, never a weekly
screening dependency or source of trade evidence.

`origin` is the upstream project `666ghj/MiroFish`; `fork` is the owner's
`Chi944/MiroFish`. Publish local changes to `fork`, never upstream. Check
`git branch -vv`, `git status --short` and `git log -5 --oneline` each session.

Do not commit `.env`/backups, uploads, simulation databases or private inputs.
Do not trigger model/Zep calls just to test serving or rendering. Preserve
loopback bindings, Docker resource limits, dual HTTP health checks and the
patched client's image compatibility. Do not activate inherited upstream jobs.
