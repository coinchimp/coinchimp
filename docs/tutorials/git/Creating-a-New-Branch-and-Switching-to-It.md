# Creating a New Branch and Switching to It and back

Creating a new branch in GitHub and switching your local folder to it (and back) involves a few steps using Git commands. Here’s a step-by-step guide:

### Creating a New Branch and Switching to It

1. **Open your terminal** or command prompt.
2. **Navigate to your local repository**:
   ```bash
   cd path/to/your/repository
   ```
3. **Ensure your local repository is up to date**:
   ```bash
   git fetch origin
   ```
   This fetches updates from the remote repository.

4. **Create a new branch and switch to it**:
   ```bash
   git checkout -b new-branch-name
   ```
   This command creates a new branch called `new-branch-name` and switches to it immediately.

5. **Push the new branch to the remote repository**:
   ```bash
   git push -u origin new-branch-name
   ```
   This pushes your new branch to GitHub and sets up tracking so that future `git push` commands will know which remote branch to push to.

### Switching Back to the Main Branch

1. **Ensure your main branch is up to date**:
   ```bash
   git fetch origin
   ```
   This fetches updates from the remote repository.

2. **Switch to the main branch**:
   ```bash
   git checkout main
   ```
   Replace `main` with `master` if your repository uses `master` as the main branch name.

3. **Pull the latest changes from the remote repository**:
   ```bash
   git pull origin main
   ```
   Again, replace `main` with `master` if necessary. This ensures your local main branch is up to date with the remote repository.

### Example Workflow

Here’s how the entire workflow might look:

1. **Navigate to your local repository**:
   ```bash
   cd path/to/your/repository
   ```

2. **Fetch updates from the remote repository**:
   ```bash
   git fetch origin
   ```

3. **Create a new branch and switch to it**:
   ```bash
   git checkout -b new-feature-branch
   ```

4. **Make your changes and commit them**:
   ```bash
   git add .
   git commit -m "Description of your changes"
   ```

5. **Push the new branch to the remote repository**:
   ```bash
   git push -u origin new-feature-branch
   ```

6. **Switch back to the main branch**:
   ```bash
   git checkout main
   ```

7. **Pull the latest changes from the remote main branch**:
   ```bash
   git pull origin main
   ```

By following these steps, you can effectively create new branches, switch between branches, and keep your local repository synchronized with the remote repository on GitHub.