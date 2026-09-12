# Local MiroFish operations

Updated 2026-09-08. This checkout is the optional hypothesis workbench for
`stock-research`, not a trading signal provider. The published Research Lab lives
at https://stock-research-ecru.vercel.app/#research-lab; there is no fourth site.

Start Docker manually with
`C:\Users\User\Documents\my-knowledge-base\scripts\start-docker.ps1`, then run
`docker compose up -d --wait --wait-timeout 120` in this directory. Both
http://127.0.0.1:3000 and http://127.0.0.1:5001/health must answer; a container
marked running alone does not establish health. The override binds both ports
to loopback and caps memory at 3 GiB, CPU at two cores, and processes at 256.
There are no new scheduled jobs. Docker's existing `unless-stopped` policy remains.

The current override mounts `patched/llm_client.py`, extracted from the running
image, to preserve JSON output headroom for reasoning models. The override pins
that tested image by SHA-256 digest so an upstream `latest` update cannot silently
break the mounted patch. The old
`patched/config.py` and disabled override record a superseded workaround; do not
re-enable them just to hide missing Zep configuration. Serving the UI does not
prove graph generation or simulations work.

Stock Research's `scripts/publish_mirofish.py` imports completed local simulation
logs separately from the weekly build. Review source hygiene and the export
before publishing. LLM personas are synthetic arguments, never measured public
sentiment, price forecasts, or permission to trade. Do not run additional
Gemini/Zep generations merely to refresh the dashboard.

Keep `.env` and all environment backups private. `.env.example` is the only
environment file allowed in version control. Local uploads, prompts, simulation
databases and logs remain ignored. Preserve the completed synthetic example
`sim_1832f35a7504`; no simulation was regenerated during this audit.

After changing the upstream image, verify the mounted patch matches the image's
module API, then repeat the two HTTP probes and check `RestartCount`/`OOMKilled`.
Do not reset Docker to factory defaults, prune volumes, or delete database data
to fix Desktop's Windows socket startup defect.

## Audit verification

2026-09-08: Compose reported healthy; both HTTP services responded; restart count
remained zero. The 28 existing repository tests passed in an isolated Linux
container with networking disabled, using the installed image. On Windows,
26 pass and two symlink tests require an OS privilege unavailable to this shell;
those same symlink protections passed in Linux. No tests were weakened.

**Correction, 2026-09-13: "28 tests" was the root `tests/` directory only.** It
left out `backend/tests` entirely - 129 tests at the current checkout, including
the ones covering `backend/app/utils/llm_client.py`, the module a local patch was
then mounted over. A pass count that omits the tests for the patched code does
not certify the patch.

Measured 2026-09-13 against the container now built from this checkout
(`mirofish-local:checkout`, no mounted patch):

    backend/tests   129 passed   (inside the running container)
    tests/           26 passed in the container; the 2 others read
                     .github/workflows/update-star-history.yml, which
                     .dockerignore keeps out of the image. From the checkout
                     on the host, that file's tests pass (8 of 8).

The two container failures are the build context, not the code: a test that reads
the repository cannot pass inside an image that deliberately excludes that part of
the repository.

Local integration changes are pushed to `fork` (`Chi944/MiroFish`) on
`local/no-zep-boot`. The historical branch name predates the working Zep
configuration; consult this file, not that name, for current behavior.
