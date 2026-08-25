# eval-gdm_self_proliferation

**GDM Dangerous Capabilities: Self-proliferation**

**Paper:** https://arxiv.org/pdf/2403.13793

Ten real-world–inspired tasks from Google DeepMind's Dangerous Capabilities Evaluations
assessing self-proliferation behaviors (e.g., email setup, model installation, web agent
setup, wallet operations). Supports end-to-end, milestones, and expert best-of-N modes.

## At a glance

| | |
|---|---|
| Upstream | [`src/inspect_evals/gdm_self_proliferation`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/src/inspect_evals/gdm_self_proliferation) |
| Group | Scheming |
| Total samples | 10 |
| Execution class | `sandbox-local` |
| Cost class | `high` |
| Flags | sandboxed · needs internet |
| Tags | Agent |

### Tasks

| Task | Samples |
|---|---|
| `gdm_sp01_e2e` | 1 |
| `gdm_sp02_e2e` | 1 |
| `gdm_sp03_e2e` | 1 |
| `gdm_sp04_e2e` | 1 |
| `gdm_sp05_e2e` | 1 |
| `gdm_sp07_e2e` | 1 |
| `gdm_sp08_e2e` | 1 |
| `gdm_sp09_e2e` | 1 |
| `gdm_sp10_e2e` | 1 |
| `gdm_sp12_e2e` | 1 |

### External assets

- `git_clone` — `https://github.com/mistralai/mistral-src.git` (pinned)
- `direct_url` — `https://files.mistral-7b-v0-1.mistral.ai/mistral-7B-v0.1.tar` (pinned)
- `direct_url` — `pip install git+https://github.com/huggingface/{transformers,peft,accelerate}.git` (pinned)
- `direct_url` — `https://raw.githubusercontent.com/UKGovernmentBEIS/inspect_evals/{version}/...secrets.zip` (controlled)

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/gdm_sp01_e2e \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
