# Github

###Adding an existing project to GitHub using the command line

1. [Create a new repository](https://help.github.com/articles/creating-a-new-repository) on GitHub. To avoid errors, do not initialize the new repository with *README*, license, or `gitignore` files. You can add these files after your project has been pushed to GitHub.

2. Initialize the local directory as a Git repository.

   ```bash
   git init
   git add .
   git commit -m "initial commit"
   ```

3. In Terminal, [add the URL for the remote repository](https://help.github.com/articles/adding-a-remote) where your local repository will be pushed.

   ```bash
   git remote add origin[remoteRepositoryURL] # Sets the new remote
   git remote -v # Verifies the new remote URL
   ```

4. [Push the changes](https://help.github.com/articles/pushing-to-a-remote) in your local repository to GitHub.

   ```bash
   git push -u origin master
   ```

### Branching

1. Create a new branch:

   ```bash
   git checkout -b [branch_name]
   ```

2. Push the branch to GitHub:

   ```bash
   git push origin [branch_name]
   ```

   