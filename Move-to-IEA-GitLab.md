<- [Home](home)

For the switch to the iea gitlab, two things are needed:


1. You need to set up a password [here](https://gitlab.iea.org/-/profile/password/edit). You can login to gitlab with your single sign in windows account, but for the connection to your local repos, this is needed.
2. In all of your local repos you might have, you have to change the remote origin url now. The old url leads to the GitHub repo, which is not active anymore. Which means for each repo one of those commands:

- `git remote remove origin`
- `git remote add origin <repo_url>`

And the repo_url is one of:
- https://gitlab.iea.org/iea/ems/rise/gis-script
- https://gitlab.iea.org/iea/ems/rise/knowledge-database
- https://gitlab.iea.org/iea/ems/rise/phase-classification
- https://gitlab.iea.org/iea/ems/rise/riselib
- https://gitlab.iea.org/iea/ems/rise/solution-file-processing