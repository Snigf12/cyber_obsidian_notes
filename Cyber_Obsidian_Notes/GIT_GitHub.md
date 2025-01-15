
## Interacting with GitHub from Terminal

Install GitHub CLI: If you haven't already, you can download and install the GitHub CLI from GitHub's official site.

Authenticate: Open your terminal and authenticate with your GitHub account using:

```bash
gh auth login
```

Create a New Repository: Use the following command to create a new repository:
```bash
gh repo create my-new-repo --public
```

Replace my-new-repo with your desired repository name. You can also add flags like ```
--private``` if you want to create a private repository.

Clone the Repository: Once the repository is created, you can clone it to your local machine:

```bash
git clone https://github.com/your-username/my-new-repo.git
```

## Using Git

Open your terminal and navigate to the directory where you want to create your project:
```bash
cd /path/to/your/project
```

Initialize a new Git repository:
```bash
git init
```
This command sets up a new Git repository in your project directory, creating a .git subdirectory that contains all the necessary metadata for the repository.

See changes
```bash
git status
```

Add files to your repository:
```bash
git add .
```
This command stages all the files in your project directory for the initial commit.

Commit the files:

```bash
git commit -m "Initial commit"
```

```bash
git branch -M main
```

```bash
git remote add origin git@github.com:USER/PROJECT_NAME.git
```

```bash
git push -u origin main
```
