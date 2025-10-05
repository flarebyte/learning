# GitHub Topics

**GitHub Topics** are short tags you add to a repository (e.g., `react`, `rest-api`, `cli`). They help people discover your project by powering GitHub Search, the **Explore** pages, and topic filters. Using clear, widely used topics improves your ranking and visibility both inside GitHub and in external search engines that index repo metadata.

## Best practices

- Add **5–15** accurate, specific topics (language, framework, domain, purpose).
- Use **common, canonical names** (`react`, not `reactjs-framework`); prefer lowercase; hyphenate multi-word terms (`data-visualization`).
- Reuse topics you see on similar popular repos (consistency boosts discoverability).
- Avoid internal jargon and duplicates (singular vs plural—pick the common one).
- Review/update topics when features or tech stack change.

## Recommended lifecycle topics

- `experimental` — Very early stage; APIs likely to change, not for production use. Remove once direction is clear.
- `prototype` — Proof-of-concept to validate ideas/feasibility; short-lived, may be thrown away.
- `wip` — Actively evolving with breaking changes expected; drop this tag once you cut a first pre-release.
- `alpha` (optional) — Pre-release with incomplete features and instability; good for early testers.
- `beta` (optional) — Feature-complete but still stabilizing; feedback welcome, expect minor breaking changes.
- `production-ready` — Intended for real-world use; stable APIs, docs/tests in place, versioned releases.
- `stable` (optional) — Extra signal that the API surface is mature; pair with `production-ready`.
- `deprecated` — Superseded; document the replacement and planned removal timeline in the README.
- `unmaintained` — No active maintenance; security fixes and PR reviews are unlikely (set expectations clearly).
- `archived` — Development frozen; set the repository to **Archived** in Settings and add this topic for search.

## topics cheatsheet

### See topics

- Current repo (pretty print):

  ```bash
  gh repo view --json repositoryTopics --jq '.repositoryTopics.nodes[].topic.name'
  ```

- Any repo:

  ```bash
  gh repo view OWNER/REPO --json repositoryTopics --jq '.repositoryTopics.nodes[].topic.name'
  ```

### Add / remove topics

- Add one or more:

  ```bash
  gh repo edit --add-topic experimental --add-topic wip
  ```

- Remove one or more:

  ```bash
  gh repo edit --remove-topic wip --remove-topic experimental
  ```

### Search repos by topic

- Global search:

  ```bash
  gh search repos --topic=production-ready
  gh search repos --topic=experimental,python        # AND across topics
  gh search repos -- -topic:archived                 # NOT
  ```

- List your org’s repos that have a topic:

  ```bash
  gh repo list YOUR_ORG --topic=production-ready -L 1000 --json name,repositoryTopics \
    --jq '.[] | select(.repositoryTopics.totalCount>0) | .name'
  ```
