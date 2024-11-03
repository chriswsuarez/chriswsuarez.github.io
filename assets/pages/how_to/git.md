# Git checkout and pull from a separate fork of repo
    git remote add <new_remote_name> <other_repo_url>
    git fetch <new_remote_name>
    git checkout <new_remote_name> <branch_name>
    git pull

# Remove last git commit (reserved for dire situations)

    git reset --hard HEAD~1
    git push origin HEAD --force

# Remove submodule from repo

- Delete the relevant section from the .gitmodules file
- Stage the .gitmodules changes
    - `git add .gitmodules`
- Delete the relevant section from .git/config
- Git remove the associated submodule files 
    - `git rm --cached path_to_submodule` (no trailing slash)
- Delete the submodule files 
    - `rm -rf .git/modules/path_to_submodule` (no trailing slash)
- Git commit  
    - `git commit -m "Removed submodule"`
- Delete the now untracked submodules file. 
    - `rm -rf path_to_submodule`