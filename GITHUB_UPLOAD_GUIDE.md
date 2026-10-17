# Upload instructions — Windows 11 / VS Code / Git

## Option A: Git from VS Code terminal (recommended)

### 1. Extract the prepared folder

Unzip the provided package into a working folder, e.g. `Documents\GitHub\NovaEdge-Secure-Enterprise-Network`. **Extract the folder**, not just the ZIP file. Open that directory in VS Code.

### 2. Create a GitHub repository

Go to GitHub → **New repository**. Set the repository name to `NovaEdge-Secure-Enterprise-Network`, add the description from `README.md`, choose **Private** until validation if preferred, and **do not initialize with a README, .gitignore, or license** (these are already local). Create the repository. Copy the HTTPS repository URL; it has the form:

`https://github.com/YOUR-USERNAME/NovaEdge-Secure-Enterprise-Network.git`

Replace `YOUR-USERNAME` below with your real GitHub account. Never invent another user's URL.

### 3. Initialize and push from the repo root

Use PowerShell or Git Bash in VS Code **inside the extracted project folder**:

```powershell
git init
git branch -M main
git add .
git commit -m "Initialize documented NovaEdge network lab"
git remote add origin https://github.com/YOUR-USERNAME/NovaEdge-Secure-Enterprise-Network.git
git push -u origin main
```

If Git tells you to configure your identity, set `git config --global user.name "YOUR NAME"` and `git config --global user.email "YOUR GITHUB EMAIL"` first. Authenticate with GitHub using the credential manager/browser login or an appropriate token when prompted; do not paste a GitHub password into repository files.

### 4. Add authentic evidence after Packet Tracer troubleshooting

Copy your real files to these paths:

- `packet-tracer/NovaEdge-Secure-Enterprise-Network.pkt`
- `diagrams/network-topology.png`
- `configs/core-switch.txt`, `configs/edge-router.txt`, `configs/isp-router.txt`
- `configs/access-switches/sw-sales.txt`, `sw-guest.txt`, `sw-it.txt`, `sw-administration.txt`, `sw-finance.txt`, `sw-servers.txt`
- `evidence/screenshots/*.png`
- `evidence/test-results/validation-matrix.csv` (update results, evidence references)

Edit the README, security policy and troubleshooting report to match actual evidence, then commit and push:

```powershell
git status
git add .
git commit -m "Add final Packet Tracer configs and validation evidence"
git push
```

**Do not push empty, invented, or renamed placeholder `.pkt` / screenshot files.** No automatic tool has generated the real Cisco project file.

## Option B: GitHub web upload

Create the repository and select **Add file → Upload files**. Drag in the **contents of the extracted project folder**, preserving directory structure, then commit. A **ZIP upload alone** creates a ZIP attachment, **not** a browsable README/docs folder structure. GitHub's browser interface accepts individual files up to **25 MiB**; command-line Git accepts files up to **100 MiB** in ordinary Git, after which Git LFS may be needed. The browser also limits the number of files per upload; Git is easier for this portfolio project.

## Final public release

After the SSH issue and final tests are documented, update the README project status and validation matrix. If you worked in a private repository, open repository **Settings → General → Danger Zone → Change repository visibility** to make it public. Review it for exposed credentials before doing so.

## Useful references

- [GitHub: import a local project](https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github)
- [GitHub: upload files through the website](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository)
- [GitHub: large file limits](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github)
