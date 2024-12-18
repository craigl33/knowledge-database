<- [Home](home)

## Move from GitHub
If you are just moving from GitHub (where the repos have been stored previously), you have to change the remote origin url now. The old url leads to the GitHub repo, which is not active anymore. Which means for each repo one of those commands:

- `git remote remove origin`
- `git remote add origin <repo_url>`

And the repo_url is one of:
- https://gitlab.iea.org/iea/ems/rise/gis-script
- https://gitlab.iea.org/iea/ems/rise/knowledge-database
- https://gitlab.iea.org/iea/ems/rise/phase-classification
- https://gitlab.iea.org/iea/ems/rise/riselib
- https://gitlab.iea.org/iea/ems/rise/solution-file-processing