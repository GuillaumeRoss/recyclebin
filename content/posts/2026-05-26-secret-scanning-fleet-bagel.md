+++
title = "Detecting dangerous secrets on dev workstations with Fleet and Bagel before Shai-Hulud does"
date = "2026-05-26"
author = "Guillaume Ross"
description = "Software supply chain malware keeps targeting secrets on developer laptops. In this article I explain how to track workstations with dangerous secrets with a combination of Fleet/osquery and bagel"
+++

## Credential theft from developer workstations
Credential harvesting via developer workstations is causing widespread havoc, as malware is able to hop from PC to published packages, into more laptops and so on. I write "developer laptops" in airquotes because they could be desktops, but also because a random PC from the finance team also has claude pulling 1000 npm packages to vibe-code a web dashboard to a CSV file, and is now also a developer workstation.

PyPI, npm, VS Code Extensions, OpenVSX, brew all expand the attack surface on workstations while companies still lie to themselves that "all code changes are reviewed", it remains that millions of developers out there have code execution access to laptops, and those developers can get compromised by various threat actors.

There are plenty of controls we can apply to those laptops, and to the packages that make it on said laptops, but it is difficult to do so especially for smaller organizations who don't mirror all the packages they may need, rely on appsec tooling which is not present between laptops and upstream package repositories to identify malware.

An excellent control to avoid secrets from being stolen is to remove secrets, or at least, keep them in a safer place than in text files. 

| Date | Campaign | Ecosystem | Secrets Stolen | From Where | Reference |
|---|---|---|---|---|---|
| May 19, 2026 | Laravel-Lang VS Code ext. (GitHub breach, TeamPCP) | VS Code Marketplace | Cloud keys, SSH keys, browser-stored creds, crypto wallets | Trojanized extension injected into 200+ versions of Laravel-Lang packages; reads `~/.aws/`, `~/.ssh/`, browser profile dirs, wallet extension storage, `.env` | [GitHub Breached via VS Code Extension (Aikido)](https://www.aikido.dev/blog/github-breached-vs-code-extension) |
| May 12, 2026 | `mistralai` client package | PyPI | Developer credentials and access tokens (Linux only); contains wiper logic for hosts geo-locked to Israel/Iran | Modified `mistralai/client/__init__.py` triggers at import; downloads `transformers.pyz` to `/tmp/`, runs as background process | [Malware in Mistral AI PyPI Package Steals Credentials (gncrypto / Microsoft Threat Intel)](https://www.gncrypto.news/news/malware-mistral-ai-pypi-credential-stealer/) |
| Mar 24, 2026 | `litellm` 1.82.7 / 1.82.8 (TeamPCP) | PyPI | 230+ env vars: `PYPI_API_TOKEN`, `GITHUB_TOKEN`, `AWS_*`, `AZURE_*`, GCS keys, `HCP_VAULT_TOKEN`, `INFISICAL_TOKEN`, `DOCKERHUB_*`, DB URLs, every LLM provider API key, Slack/Sentry/Datadog/PagerDuty | `litellm_init.pth` (34,628 B) dropped into `site-packages/` — runs on **every Python startup**, no `import` needed; exfil to `models.litellm.cloud` | [TeamPCP Compromises LiteLLM (Boost Labs)](https://labs.boostsecurity.io/articles/teampcp-litellm-supply-chain-compromise) |
| Mar 19, 2026 | Trivy v0.69.4 (TeamPCP) | GitHub Releases / Docker / `trivy-action` | Whatever was reachable in the build env (GitHub Releases tokens, Docker Hub creds, signing keys, CI secrets); also `aqua-bot` service-account creds | Backdoor Go source injected at runtime by impostor `actions/checkout` during the Release build (never committed); poisoned binary then exfils CI env vars wherever it runs | [20 Days Later: Trivy Compromise, Act II (Boost Labs)](https://labs.boostsecurity.io/articles/20-days-later-trivy-compromise-act-ii) |
| Nov 24, 2025 | Shai-Hulud 2.0 ("Second Coming") | npm (+ PyPI) | GitHub PATs, npm creds, cloud keys (AWS/GCP/Azure), SSH keys; also registers host as GitHub Actions self-hosted runner for persistence | `preinstall` → `setup_bun.js` drops/uses Bun runtime to run obfuscated `bun_environment.js`; reads env vars, `~/.aws/credentials`, `~/.ssh/id_*`, `~/.npmrc`, `~/.config/gcloud/*`, `.env`; wiper fallback on `$HOME` if exfil fails | [Defensive Research, Weaponized (Boost Labs)](https://labs.boostsecurity.io/articles/defensive-research-weaponized-the-2025-state-of-pipeline-security) |
| Oct 17, 2025 | GlassWorm (wave 1) | OpenVSX / VS Code | npm tokens, GitHub tokens, Git creds, OpenVSX tokens, crypto-wallet data from 49 wallet extensions | VS Code extension activation code (hidden in invisible Unicode PUA chars); reads `~/.npmrc`, `~/.gitconfig`, browser/wallet extension storage; C2 on Solana blockchain | [Defensive Research, Weaponized (Boost Labs)](https://labs.boostsecurity.io/articles/defensive-research-weaponized-the-2025-state-of-pipeline-security) |

## Mitigating the issue with open-source tools

### Bagel

An open-source tool by Boost Security, [bagel](https://github.com/boostsecurityio/bagel) scans for secrets typically related to development work in a user's home directory. It is configurable, with probes for SSH keys, Github credentials, cloud providers and much more. It outputs in JSON, and includes finding severity and location but of course does not log the actual secrets.

As it is a command line tool, it can easily be scheduled with a LaunchAgent, and as its output is JSON, it is easy to integrate with other tools to provide for a more managed solution than just telling developers to "run `bagel` and clean up`.


### Fleet

[Fleet](https://github.com/fleetdm/fleet) is an open-source platform for IT and security, which uses [osquery](https://osquery.io) for telemetry. Think of it as a MDM (mobile device management) and osquery server, managed via GitOps (optionally, but you should take that option).

Fleet provides the following features which are critical to making our secret scanning more robust than a simple ad-hoc effort:

1. Package deployment.
2. Policy queries, or "checks".
3. A table that is not part of standard osquery, meant specifically to [parse JSON](https://fleetdm.com/tables/parse_json).

### Fleebag 
Fleebag (for *fleet-bagel* - I am an expert at naming things) is a simple vibe-coded repository containing the following:

1. Scripts to create a macOS installation package of `bagel`, with a LaunchAgent running it on a schedule, outputting logs to a standard destination.
2. A Fleet query for all secrets `bagel` found on each workstation.
3. A Fleet policy query that passes if `fleebag` results are fresh and contain no critical findings, which you can tune to your desired severity.
4. An example macOS profile to grant `bagel` full disk access, to not depend on end-users accepting security prompts on macOS.



{{< goatcounter >}}