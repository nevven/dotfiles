# dotfiles

## Simple setup for my dotfiles
`python collects > pwsh run > commit > push`

### Collection Script (Python)
```python
"""Collect dotfiles from various locations and copy to the dotfiles repo."""

import shutil
from pathlib import Path

# Repository location
repo_root = Path("PATH_TO_YOUR_LOCAL_REPO")

# Map: destination_in_repo -> source_path
files = {
    "SOURCE_PATH_01": "DESTINATION_PATH_01",
    "SOURCE_PATH_02": "DESTINATION_PATH_02",
}

def main():
    print(f"Target repo: {repo_root}\n")
    
    for dest, source in files.items():
        source_path = Path(source)
        dest_path = repo_root / dest
        
        if not source_path.exists():
            print(f"Source not found: {source}")
            continue
        
        # Ensure destination directory exists
        dest_path.parent.mkdir(parents=True, exist_ok=True)
        
        # Copy file
        shutil.copy2(source_path, dest_path)


if __name__ == "__main__":
    main()
```

### PowerShell Function
```powershell
# Commit and push dotfiles to github
function Sync-Dotfiles {
    Write-Host "`nSyncing dotfiles to GitHub..." -ForegroundColor Cyan
    
    # Run collection script
    python PATH_TO_YOUR_SCRIPY\script.py
    
    if ($LASTEXITCODE -ne 0) {
        Write-Host "Collection failed" -ForegroundColor Red
        return
    }
    
    # Git operations
    Push-Location PATH_TO_YOUR_LOCAL_REPO
    
    git add .
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm"
    git commit -m "Update configs - $timestamp"
    git push
    
    Pop-Location
    Write-Host "Dotfiles synced`n" -ForegroundColor Green
}
Set-Alias -Name dotfiles -Value Sync-Dotfiles
```

Run `dotfiles` every time you want to push changes
