# newman-reporter-github-pullrequest

[![NPM Version](https://img.shields.io/npm/v/newman-reporter-github-pullrequest.svg?style=flat-square)](https://www.npmjs.com/package/newman-reporter-github-pullrequest)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=dktunited_newman-reporter-github-pullrequest&metric=alert_status&token=f25ebda872835bf65726cd70f77a84bb808dec9d)](https://www.npmjs.com/package/newman-reporter-github-pullrequest)
[![NPM Weekly Downloads](https://img.shields.io/npm/dw/newman-reporter-github-pullrequest.svg?style=flat-square)](https://www.npmjs.com/package/newman-reporter-github-pullrequest)
[![NPM Downloads](https://img.shields.io/npm/dt/newman-reporter-github-pullrequest.svg?style=flat-square)](https://www.npmjs.com/package/newman-reporter-github-pullrequest)

## Description

Report [newman](https://github.com/postmanlabs/newman) collection results on an extra check in the Pull Request Checks tab.
If you need a way to export your Postman collection results into Github Pull Request, and you're using github workflow to handle your CI then you're in the right place !  

PullRequest's Check example :  

![PullReques's Check example](https://user-images.githubusercontent.com/45691655/122566992-768b5e80-d048-11eb-9296-abe3b2086be7.png)

## HIGHLIGHTED FEATURES
  
* Handle deep nested postman folder structure
* Group requests by their folder's name in the generated markdown

## Getting Started

1. Install `newman`
2. Install `newman-reporter-github-pullrequest`
3. Run your github workflow and extract from it :
     * GITHUB TOKEN from your running job. Usually, you can get it from ${{ secrets.GITHUB_TOKEN }}.

### Prerequisites

1. `node` and `npm`
2. `newman` - `npm install -g newman`
3. A github token either from :
	* your running github workflow provided as a secret (see above)
	* your own specific Github App (see Notes what issue this option solves)

---

## Installation

```console
npm install -g newman-reporter-github-pullrequest
```

> Installation should be done globally if newman is installed globally, otherwise install without `-g` option

---

## Usage

Specify `-r github-pullrequest` option while running the collection

In non export mode (it means you actually want to update github pull request) :  

```bash
newman run <collection-url> --environment=<env-url> -r github-pullrequest \
  --reporter-github-pullrequest-repo <repo> \
  --reporter-github-pullrequest-token <github-token> \
  --reporter-github-pullrequest-checkname <check-name> \
  --reporter-github-pullrequest-refcommit <ref-commit> \
  --suppress-exit-code
```

In export mode :  

```bash
newman run <collection-url> --environment=<env-url> -r github-pullrequest \
  --reporter-export <export-path> 
```

### Options:

**Option** | **Remarks**
--- | --- 
`--reporter-github-pullrequest-repo` | (Required) Usually you can get it from ${{ github.repository }}. It follows this pattern : "organization/repository"
`--reporter-github-pullrequest-token` | (Required) Github token : Usually you can get it from ${{ secrets.GITHUB_TOKEN }} while job is **running**. For more details : https://docs.github.com/en/actions/reference/authentication-in-a-workflow#about-the-github_token-secret
`--reporter-github-pullrequest-checkname` | (Optional : Default `newman-check`) Name you want to give to the check name that will be created by the reporter. This parameter is useful if you want to wait for a specific check name to be completed inside your workflow. See, for example, the following github action : fountainhead/action-wait-for-check@v1.0.0
`--reporter-github-pullrequest-refcommit` | (Required) Long Commit hash. When you run this reporter from a Pull Request. You should use : ${{ github.event.pull_request.head.sha }}
`--reporter-debug` | (Optional : Default `false`) Reporter debug mode
`--suppress-exit-code` | (Required) Ensure that asynchronous github API is called before reporter termination.

---

## Notes:

PullRequest's check report can appear on the wrong check suite. This is due to a known github limitation. See here : https://github.community/t/specify-check-suite-when-creating-a-checkrun/118380  
To solve this issue, you can use a token from your own created Github App (and not the one used in the github workflow) so this way, the check run will be automatically created on a specific check suite. 


---

## Development

- `npm pack`
- `npm i -g newman-reporter-github-pullrequest.<version>.tgz`

---