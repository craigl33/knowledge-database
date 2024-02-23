<- [Home](home.md)

## Initial GitLab connection
To access the GitLab WebApp, Single-Sign-in can be used via Windows. Just as if you were accessing the intranet or Office 365. That probably also worked, otherwise you would not see this page.

However, it does not work to establish a local connection to a repo. For this you can either use SSH keys or your own GitLab password. How to setup SSH keys is described [here](https://python.iea.org/doc/getting-started/doc-gitlab-iea/sdlc.html#gitlab-on-premise-at-the-iea) by ISU. However, the second option is the easier one. Own GitLab password just means you set a password for GitLab, and this one will work for authentification. Your Windows password will never work.

Then you can use your email (e.g. lukas.trippe@iea.org) and the password you set when git asks you for it. So with the first `git clone` command. This authentication only needs to happen once.

1. Setup password [here](https://gitlab.iea.org/-/profile/password/edit). 
2. Use password and email as username to login, whe ngit is asking for it.


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