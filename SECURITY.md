# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this repository (e.g. exposure of
API keys/secrets, unsafe handling of fetched GitHub data, prompt-injection
risk in LLM-processed content, etc.), please report it privately rather than
opening a public issue.

- **Preferred:** Use GitHub's private vulnerability reporting
  (Security and Quality tab → "Report a vulnerability") on this repository.


Please do not disclose the issue publicly until it has been triaged.


## Scope

This project consists of notebooks that call third-party LLM APIs
(Anthropic, OpenAI) and the GitHub API. Relevant concerns include:
- Accidental leakage of API keys/secrets in notebooks or exported results
- Injection risks from untrusted issue/advisory text passed to LLM prompts
- Handling of GitHub tokens and rate-limit credentials
