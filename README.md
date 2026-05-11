# thesupersupersigma.github.io

This document explains the process used to turn a folder inside a parent Git repository into a separate, independent repository while maintaining a link between them using **Git Submodules**.

## The Goal
The objective was to have a main repository (`notes`) and a nested repository (`thesupersupersigma.github.io`) where:
1. Pushing inside the sub-folder updates the **Website Repo**.
2. Pushing inside the root folder updates the **Notes Repo**.
3. GitHub shows a clickable link from the Notes Repo to the Website Repo.

---

## Step 1: Initialize the Nested Repository
First, we created and initialized the sub-project as its own Git entity.

```bash
# Navigate to the target sub-folder
cd "Codeacademy Notes/CodeacademyLocalProjects/thesupersupersigma-github-io"

# Initialize and make the first commit
git init
echo "# thesupersupersigma.github.io" >> README.md
git add README.md
git commit -m "first commit"

# Link to its own dedicated GitHub repository
git remote add origin git@github.com:thesupersupersigma/thesupersupersigma.github.io.git
git push -u origin master

```

---

## Step 2: Register as a Git Submodule

After the sub-repo was live on GitHub, we needed to tell the parent repo (`notes`) to treat that folder as a submodule rather than just a regular directory of files.

```bash
# Go back to the root of the parent repository
cd ../../../..

# Add the sub-repo as a submodule using its relative path
git submodule add git@github.com:thesupersupersigma/thesupersupersigma.github.io.git "Codeacademy Notes/CodeacademyLocalProjects/thesupersupersigma-github-io"

```

### Key Technical Detail:

When we ran `git commit`, Git created two items:

1. `.gitmodules`: A text file that stores the mapping between the local folder and the remote URL.
2. `mode 160000`: A special "gitlink" entry in the parent repo that records exactly which commit of the sub-repo is currently tracked.

---

## Step 3: Pushing Changes to the Parent

Once the submodule was added, we committed the link to the parent repository.

```bash
git add .
git commit -m "Added github.io as a submodule"
git push origin master

```

---

## Daily Workflow

### Updating the Website (Sub-Repo)

1. `cd` into `Codeacademy Notes/CodeacademyLocalProjects/thesupersupersigma-github-io`.
2. Make changes to your code.
3. `git add`, `git commit`, `git push`.
4. This updates the **thesupersupersigma.github.io** repository.

### Updating the Notes (Parent Repo)

1. `cd` to the `notes` root.
2. If you want the parent repo to "remember" the latest commit of your website, run `git add .` and `git commit`.
3. Make changes to any other notes files.
4. `git push origin master`.
5. This updates the **notes** repository.

---

## Troubleshooting & Tips

* **Absolute vs. Relative Paths:** When using `git rm` or `git submodule add`, always use paths relative to the root (e.g., `Codeacademy Notes/...`) rather than full system paths (e.g., `/Users/thesupersupersigma/...`).
* **The "Ghost" Folder:** If you see a folder on GitHub that you cannot open, it usually means a `.git` folder was created inside but not registered as a submodule. Using `git rm -r --cached [path]` and re-adding it as a submodule fixes this.
