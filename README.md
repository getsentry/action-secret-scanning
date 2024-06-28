# action-secret-scanning
Perform Secret Scanning on Pull Requests

This action uses Trufflehog OSS secret scanner to perform scanning on pull requests.

## Usage
Example of using this action app
```
Any secrets detected will be highlighted in the files changes in pull requests, as the following screenshot.
![Example](/secret_scanning_example.png)

If you want the scan result commented on the PR, you can do:

```
- uses: getsentry/action-secret-scanning@v1
    id: call_trufflehog
    continue-on-error: true
- uses: mshick/add-pr-comment@b8f338c590a895d50bcbfa6c5859251edc8952fc
    if: steps.call_trufflehog.outcome != 'success'
    with:
        message: |
        🚨🚨🚨 Secret found 🚨🚨🚨
        Please review your commit
- name: Fail workflow if secret detected
    if: steps.call_trufflehog.outcome != 'success'
    run: exit 1
```

## Benefits
This action pulls the latest version of TruffleHog OSS from https://api.github.com/repos/trufflesecurity/trufflehog/releases/latest, and uses Cosign to verify the checksum of the downloaded release. The action is ran directly in GitHub Action, not using docker or any container.