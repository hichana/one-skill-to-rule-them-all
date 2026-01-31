# GitHub Stats Skill

Display GitHub repository statistics.

## Description

Fetches and displays statistics for any public GitHub repository, including stars, forks, and recent activity.

## Usage

"Show me stats for github.com/torvalds/linux"

## Implementation

```bash
#!/bin/bash

REPO="$1"  # Format: "owner/repo"

# Fetch from GitHub API (whitelisted domain)
curl -s "https://api.github.com/repos/${REPO}" | jq '{
  name: .name,
  stars: .stargazers_count,
  forks: .forks_count,
  open_issues: .open_issues_count,
  language: .language,
  description: .description
}'
```

## Features

- Repository information
- Star and fork counts
- Primary language
- Open issue count
- Description

## Requirements

- `jq` for JSON parsing (install via: `brew install jq` or `apt-get install jq`)
- Internet connection

## Privacy

Only accesses public GitHub API endpoints. No authentication required. No user data collected.
